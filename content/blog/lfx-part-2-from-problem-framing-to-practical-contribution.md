---
title: "LFX: From Problem Framing to Practical Contribution"
date: 2026-05-19
draft: true
tags: ["kubernetes", "sig-docs", "localization", "open-source", "lfx", "ai"]
---

## From framing to building

In the [previous post](/blog/lfx-part-1-from-ambiguity-to-problem-framing/), I wrote about the first half of my LFX 2026 Mentorship for Kubernetes SIG Docs localization: how the work started as a broad, slightly chaotic problem space ("how should localization workflow automation evolve in an AI era?") and gradually narrowed into smaller questions the community could actually answer together. Three directions emerged from that framing: content-based outdated detection, Gen AI guidance for localization teams, and better visibility into team status.

This post is about what happened next: turning that framing into concrete work. Mostly that meant a prototype around the first direction: a triage helper for potentially outdated localized pages, together with a few smaller side outputs (an issue against a content inconsistency, design notes, and PR-level documentation). It is also about what I learned along the way: that "from framing to building" is not really a single step. Each design choice forced me to re-frame the problem at a smaller scale.

## The practical problem: knowing what needs review

Kubernetes documentation is maintained in English first, and many localization teams translate that content into other languages.

Over time, the English source changes. Some changes are small, such as formatting updates or wording improvements. Other changes are more important, such as new feature behavior, command examples, warnings, or version-specific information.

For localization teams, the hard question is not simply whether a file changed. The harder question is: which localized pages probably need human review first?

A simple timestamp or Git history check can help, but it is often too noisy. It may flag pages because of small upstream edits, or miss pages where the localized content was edited recently but still drifted from the English source.

So the goal of the prototype became narrower and more practical: a lightweight triage helper that looks at content and structure signals, then produces a report that helps localization maintainers prioritize review. The task was not "can I detect outdated localized content?" but "can I design a tool that gives useful signals to reviewers without pretending to replace their judgment?" That difference shaped almost every design decision.

## When section-level alignment became the wrong level of abstraction

My first implementation tried to compare English and localized pages section by section. The idea made sense at the time: if a localized page is supposed to follow the English source, then comparing corresponding sections should help detect drift more precisely. So the prototype parsed Markdown, grouped content into sections, tried to align English and localized sections, and then compared structural and content-related signals.

The biggest blocker was finding a clean and elegant way to make section alignment work well.

In theory, headings and anchors should provide useful alignment points. In practice, many localized pages do not preserve anchors in a consistent way. Some anchors are translated, some are omitted, and some pages have heading or anchor structures that no longer match the English source closely enough for a simple alignment strategy to work.

That made section-level comparison noisy. If the script could not reliably decide which English section matched which localized section, then the later comparison logic became fragile too. A page could look outdated because the alignment failed, not because the translation actually missed meaningful content.

Fixing this properly would likely require a much heavier structure-aware approach, maybe closer to an AST-style or tree-based comparison. That felt too expensive for the goal of the project. The tool was not supposed to prove translation correctness — it was supposed to help maintainers triage which pages may deserve attention first. That distinction changed the direction of the work.

## Moving from section alignment to file-level signals

After discussion with my mentors, I stepped back from the section-by-section comparison and moved toward a simpler file-level approach.

Instead of trying to perfectly match each English section with each localized section, the script would look at the whole file and collect lightweight signals, such as:

- heading structure differences
- code block count differences
- missing or changed anchors
- Kubernetes version string differences
- API or feature-state differences
- possible untranslated English-heavy content
- file length or content-ratio gaps

This was less ambitious, but more practical.

The goal became signal-based triage. The script would not say, “this translation is wrong.” It would say, “this page has signals that may be worth checking.”

That wording mattered.

In localization, especially in a community like Kubernetes SIG Docs, automation should support human review instead of replacing it. A tool that sounds too definitive can create unnecessary pressure for localization teams or make reviewers distrust the output.

A tool that clearly presents signals, confidence, and limitations is easier to use responsibly.

This also made the implementation easier to explain. Reviewers did not need to understand a complicated section-matching algorithm. They could inspect a report and see why a file was flagged.

## Polish became part of the design

Once the general direction became clearer, the work shifted from proving the idea to making it reviewable. That helped me understand the difference between a prototype and a contribution: a prototype can be messy if it proves an idea, but a contribution needs to be understandable by other people.

Mentor feedback helped identify more useful signals, improve the report format, and make the implementation easier to follow. Some feedback focused on the detection logic itself, but a lot of it focused on readability and maintainability: clearer command parameters, simpler output filenames, better report links, more consistent terminology, shorter names, and helper functions grouped around the analysis flow.

That changed how I thought about polish. At first, I treated it as normal code cleanup — shortening functions, renaming variables, reducing duplicate logic. Through the review process, I learned that polishing an open source tool also means reducing conceptual burden for future readers.

A lot of the improvement happened around terminology and presentation. We renamed status labels so the report sounded less definitive and more like reviewer-facing triage. Instead of presenting a page as simply "outdated," the report moved toward labels such as "likely outdated," "maybe outdated," and "no signal." That better matched what the script could responsibly claim. We also improved the report disclaimer, added clearer comments around thresholds and locale-specific adjustments, simplified command options, and reorganized helper functions around the analysis flow.

These changes did not necessarily change the core detection idea, but they made the script much easier to understand — important because the tool was no longer a private experiment. It was becoming something other maintainers might read, run, question, and possibly maintain later. In that sense, polish was not just cosmetic. It was part of making the tool acceptable as a community contribution.

## A report is also part of the product

Another lesson was that the output format matters almost as much as the detection logic.

If the script only prints raw numbers, it may be useful to the person who wrote it, but not very useful to maintainers.

A reviewer needs to know what was checked, why a file was flagged, and what kind of follow-up is expected.

So the final direction focused on Markdown reports that could be read by localization teams.

The report explains that it is a triage aid, not a final review. It groups results into categories such as likely outdated, maybe outdated, orphaned, and no signal. It also gives concrete indicators so reviewers can understand why a file appears in the report.

This was a small design choice, but it reflects the larger goal of the project.

For community adoption, a tool needs to be transparent. People should be able to disagree with its output, inspect its reasoning, and decide whether the signal is useful for their team.

That is especially important for localization because every team has its own history, conventions, reviewer availability, and maintenance rhythm.

A useful shared tool should not erase those differences. It should give teams better visibility while leaving final judgment to humans.

## Finding real issues, not just passing a benchmark

One encouraging moment was when the tool helped surface real structural inconsistencies in localized pages. While testing outdatedness detection, I noticed that some localized pages did not preserve heading levels from the English source: some English H2 headings appeared as H3 in localized content, and some H3 headings appeared as H2.

This was not the original goal of the script, but it showed that structural signals could reveal maintainability problems that are easy to miss during normal review.

It also led to a separate but related improvement. Earlier, the script had a `zh-cn`-specific heading guard to avoid over-triggering on one of these cases. After seeing the report output, it became clearer that the better fix was not to keep a hidden language-specific workaround inside the script, but to address the underlying content inconsistency. So I opened an issue for it, and after that was addressed, the script no longer needed the special `zh-cn` guard.

A useful lesson: sometimes the right way to simplify a tool is not to add smarter logic, but to fix the underlying content issue that made the special case necessary. It also made the tool more general: less tied to one language's temporary edge case, and easier to explain as a shared localization triage helper.

At the same time, this reminded me to be careful. A signal is not the same as a conclusion. Some differences may be intentional or harmless. The tool should make review easier, not create automatic judgments.

## Preparing the final PR

By the end of this process, the work became a more focused PR: a script for reporting potentially outdated localized documents using structural signals.

Compared with the first prototype, the final PR had a clearer purpose. It did not try to solve all localization freshness problems, fully understand translation meaning, or replace existing reviewer workflows. Instead, it introduced a standalone triage helper that localization teams could run, inspect, and adapt.

The PR description also became part of the work. I tried to provide enough context for reviewers:

- what problem the script is solving
- how to try it locally
- what the report categories mean
- what the limitations are
- why this approach may be useful

This was another open source lesson for me. Code is not enough when the problem space is unfamiliar or cross-cutting. A good PR also needs to teach reviewers how to evaluate it. For this project, that meant being clear that the script checks structural and outdatedness indicators, not translation quality. It meant explaining that "no signal" does not guarantee a translation is current. It meant making the tool safe to use as a first-pass filter rather than an authority.

## What changed in how I think about solutions

At the beginning of the mentorship, I was more focused on technical accuracy: could I make the detector more precise, reduce false positives, find more outdated pages?

Those questions still matter. But they are not enough.

By the end, I cared more about whether the solution fit the community:

- Can maintainers understand the script quickly?
- Can localization teams trust the report enough to try it?
- Can the output help reviewers prioritize work instead of adding more burden?
- Can the implementation stay small enough that future contributors can maintain it?
- Can the tool be useful even when it is imperfect?

I stopped thinking of the project as building the best detector, and started thinking of it as building a practical review aid for a real community.

## The final lesson: usefulness is a design constraint

This mentorship taught me that open source engineering is not only about solving a technical problem. It is about solving the right-sized version of the problem in a way that other people can accept, review, maintain, and improve.

The first version of my detector tried to be more precise by doing more. The final direction became better by doing less, but doing it more clearly.

That does not mean the simpler approach is perfect. There are still many possible follow-ups: better calibration across languages, more validation from localization teams, improved reporting, and maybe future experiments with LLM-assisted review guidance. Those should be built carefully on top of a workflow people can already understand.

For me, the second half of the LFX Mentorship was about moving from an idea to a contribution. Not just "I built a script," but:

- I learned what the script should *not* claim.
- I learned how mentor feedback can reduce complexity.
- I learned that output wording affects trust.
- I learned that a useful tool needs to fit existing human review practices.

Looking back across the whole arc, from the zh-tw coordination thread, through the first ambiguous weeks of the mentorship, to a small but specific PR, the through-line is the same. In a large open source community, making something easier to understand is not a secondary task. It is part of the engineering work itself.
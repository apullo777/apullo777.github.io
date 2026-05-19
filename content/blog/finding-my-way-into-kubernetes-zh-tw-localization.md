---
title: "Finding My Way into Kubernetes zh-tw Localization"
date: 2026-05-16
tags: ["kubernetes", "sig-docs", "localization", "open-source", "lfx"]
---

## Starting from a coordination gap

My entry into Kubernetes localization did not start from a carefully planned roadmap. It started from curiosity.

At the time, I was looking into the LFX Mentorship program and trying to understand how SIG Docs and localization work in Kubernetes. While watching a SIG Docs Localization Subproject meeting recording, I noticed that Seokho, one of the localization subproject leads, mentioned an issue about initiating Traditional Chinese (zh-tw) localization for the Kubernetes website.

That immediately caught my attention. I grew up in Taiwan, read and write Chinese, and have always felt that technical documentation becomes much more approachable when it speaks in the language and terminology people actually use. So I opened the issue to understand the situation.

What I found was not simply "a translation task waiting for contributors." It was a coordination problem.

The original request asked to enable `content/zh-tw` in the Kubernetes website repository, add language configuration, and begin Traditional Chinese localization. Early comments also made clear that SIG Docs expected a sustainable localization team, not just a one-time translation drop. Kubernetes documentation is large, constantly changing, and needs long-term review and maintenance.

There had also been an earlier attempt to add Traditional Chinese localization through a very large PR. The PR added the `content/zh-tw/` directory, updated the language configuration, and included a large amount of translated content at once. Reviewers quickly pointed out that this was difficult to review: the PR was huge, some links and phrases still reflected zh-cn assumptions, and some wording did not sound natural for Traditional Chinese users in Taiwan.

The technical concern soon became a social one. Reviewers asked for the work to be split into smaller PRs and emphasized that every page should be reviewable before being merged. The PR author, however, felt the pushback was becoming discouraging. From one side, reviewers worried about quality and long-term maintenance; from the other, the contributor felt that their work was being criticized without enough help to move it forward.

That tension later spilled back into the original issue. The disagreement was no longer only about one PR. It exposed a deeper question: how should a new localization start when there is enthusiasm, but not yet a stable team, shared terminology, or a clear review process?

From the outside, it looked like a conflict. But the more I read, the more I saw it as a signal. People were arguing because zh-tw localization mattered, and because everyone understood that a poor foundation would create long-term costs for future contributors and readers.

The immediate problem was not a lack of translated text. The harder problem was building a workflow that people could trust.

## Reading conflict as signal, not noise

The issue thread taught me something important about open source: conflict is not always a sign that a project is unhealthy.

In this case, the disagreement showed that people were trying to protect the quality and maintainability of the documentation. The concern was not "do not contribute." It was closer to: "please contribute in a way that future contributors can review, maintain, and build on."

Instead of treating the thread as a dead end, I left a comment offering to help from the coordination side: gathering contributors, tracking work, and moving the effort toward smaller, reviewable PRs.

Soon after that, @tico88612 contacted me on Slack. That was the real turning point.

## Getting connected through @tico88612

@tico88612’s help made the effort feel much more concrete. He had Kubernetes experience (`k-sigs/kubespray`), understood the expectations from SIG Docs, and also knew the Taiwanese open source context. Since he had been one of the reviewers pushing back on the earlier PR, talking with him directly helped me understand that the tension was not just criticism of someone’s work. It came from concrete concerns about reviewability, terminology, and long-term maintenance.

Through our Slack conversations, I learned that the key was not to rush into another massive content PR, but to first create a path that other contributors could follow.

We discussed the need for a dedicated tracking issue, more contributors, and a way to coordinate work in Slack. @tico88612 also helped connect me with **OpenSource4You**, also known as **源來適你** or **ALC Taipei**, a prominent Taiwan-based non-profit open-source community. Through that connection, we could reach more people who were interested in contributing to Kubernetes zh-tw localization.

That changed the effort from “one person trying to help on GitHub” into “a small group beginning to form.”

This part was especially meaningful to me because it showed how open source work often moves through informal trust networks. The public GitHub issue mattered, but so did the Slack conversations, introductions, and community outreach happening around it.

## Learning the Kubernetes SIG Docs workflow

As I got more involved, I realized that contributing to the Kubernetes website is not just about editing Markdown. The repository has ownership rules, labels, review expectations, localization conventions, and a publication process.

For zh-tw, that meant the work needed to be organized before it could scale: an umbrella issue for tracking, separate setup and content PRs, smaller reviewable batches, and shared documents for terminology and contribution guidance.

Eventually, the original issue was closed, and the work moved to the new umbrella issue. That felt like an important milestone: the effort had moved from scattered discussion into a more organized localization project.

## Current status of zh-tw localization

As of now, the Traditional Chinese localization work is being tracked through a dedicated umbrella issue:

- Umbrella issue: https://github.com/kubernetes/website/issues/54645

Setup and Tutorials are now tracked through dedicated issues:

- Setup tracking issue: https://github.com/kubernetes/website/issues/55006
- Tutorials tracking issue: https://github.com/kubernetes/website/issues/55046

The team is still small, but real. Around three to four regular contributors are helping with translation and review. We are also keeping live working documents in the Kubernetes Slack channel:

- contributing guide: https://kubernetes.slack.com/docs/T09NY5SBT/F0ANHDS1E48
- glossary: https://kubernetes.slack.com/docs/T09NY5SBT/F0ANM7SLH3J

The next step is to continue working through the minimum required content for publication on `main`.

There is still a lot to do, but the direction is clearer now. We are not trying to “translate everything at once.” We are trying to build a sustainable localization workflow.

## What I learned about stepping in

This experience changed how I think about open source contribution.

I used to assume that the best way to contribute was to find a clearly defined issue, write code or documentation, and submit a PR. That is still one valid path. But this experience taught me that coordination itself can be a contribution.

Sometimes the project does not need one heroic patch. It needs someone to read the context carefully, identify why progress is stuck, ask the right people for guidance, and create enough structure for others to join.

I also learned that it is okay to enter a project through uncertainty. When I first commented, I was not a Kubernetes organization member, I did not know the full SIG Docs process, and I was still learning the repository structure, review culture, and localization expectations.

But I did have one useful perspective: I understood the language, cared about the community, and was willing to help organize. That was enough to start.

## Notes for future contributors

If you want to contribute to Kubernetes localization, my advice is to spend time reading before opening a PR.

Read the existing issue threads. Look at how maintainers respond. Study how other localization teams track their progress. Join Slack and observe how people coordinate. Try to understand not only what needs to be done, but why previous attempts may have stalled.

A good first contribution is not always the biggest one. In localization, a small, reviewable PR is often much more valuable than a huge PR that no one can confidently review.

Most importantly, remember that localization is community work. The goal is not only to produce translated pages. The goal is to create documentation that people can trust, maintain, and improve together.

For me, the zh-tw localization journey started with a meeting recording, an issue thread, and a moment of discomfort while reading a conflict. But that conflict revealed a real need: Taiwan users deserved high-quality Kubernetes documentation, and contributors needed a sustainable way to collaborate.

That is how I entered the work — not by having all the answers, but by noticing a gap and deciding to step into it.

That step later became the bridge into the LFX 2026 Mentorship for Kubernetes SIG Docs, where the same questions about sustainability and workflow showed up at a larger scale: not for one language team, but across localization in an AI era. I'll write about that next.
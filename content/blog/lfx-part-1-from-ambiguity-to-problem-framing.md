---
title: "LFX: From Ambiguity to Problem Framing"
date: 2026-05-18
draft: true
tags: ["kubernetes", "sig-docs", "localization", "open-source", "lfx", "ai"]
---

## Entering a larger problem space

Before this mentorship, I had been helping start Traditional Chinese (zh-tw) localization for the Kubernetes website, coordinating a small group of contributors, working through SIG Docs review expectations, and trying to set up a sustainable workflow ([more on that here](/blog/finding-my-way-into-kubernetes-zh-tw-localization/)). The short version: localization in Kubernetes is not only about translated text. It is also about trust, reviewability, terminology, contributor coordination, and long-term maintenance.

That experience gave me useful context when I joined the LFX 2026 Mentorship for Kubernetes SIG Docs localization. It did not, however, give me all the answers.

At first glance, the mentorship sounded like an AI-assisted localization project. It touched on outdated document detection, review support, team visibility, prompt guidance, and possibly even a contribution assistant. These were all topics I cared about, especially because they connected directly to problems I had seen on the zh-tw side.

But the more I listened, the more I realized that "AI-assisted localization" was not quite the center of the project.

The real question was broader: how can a community explore localization workflow automation in an AI era without weakening the review practices that already keep the project sustainable?

It was not simply asking me to build one tool. It was asking me to understand a system: AI, localization quality, automation architecture, SIG Docs governance, maintainer workload, community adoption, and the reality that every localization team may work differently.

That was my first real impression of the mentorship: exciting, diverse, and honestly a little chaotic.

## Many mentors, many valid concerns

One reason the project felt complex was that there were several mentors, each looking at the work from a different angle.

Some discussions focused on AI governance: where AI should help, where it should not, and how to keep human review as the final authority. Some focused on localization quality: terminology consistency, review signals, and whether automation could actually help reviewers instead of creating more things for them to check. Some focused on implementation: whether a prototype could be simple, deterministic, maintainable, and realistic inside the Kubernetes website repository. Others focused on SIG Docs sustainability: whether a proposal would fit existing workflows, whether maintainers would accept it, and whether the work could be generalized beyond one language team.

At first, this felt like too many directions at once. Gradually, I realized that this was not a lack of direction. It was the shape of the problem.

In a project like Kubernetes, a technically interesting idea is not automatically a good contribution. It also needs to fit the project's review culture, maintenance capacity, governance model, and contributor expectations. A tool that looks useful in isolation can still fail if it creates too much triage burden, assumes a workflow that teams do not use, or tries to enforce a policy before the community has agreed on the problem.

## The pressure to show progress

Because the LFX timeline was short, I initially felt pressure to make visible progress quickly.

In open source, visible progress often means opening an issue, submitting a PR, writing a design, or showing a prototype. That instinct is understandable. Public artifacts show that something is happening.

But I also started to see the other side.

Every issue asks maintainers to read, label, triage, and decide whether the direction makes sense. Every PR asks someone to review code, check assumptions, and think about long-term maintenance. Even a well-intentioned proposal can become noise if it is opened before the problem is clear enough.

This was an uncomfortable lesson for me.

I wanted to prove that I was keeping up with the mentorship. But I also did not want to create more work for the community just to make my own progress visible.

That tension became one of the most important lessons of the mentorship: contributing is not only about producing artifacts. It is also about understanding when an artifact is useful to others.

## From broad ideas to smaller questions

The project started with many possible directions. Three of them stood out, and working through each one helped me see how to narrow a broad idea into a question the community could actually answer.

**Content-based outdated detection.** Localization freshness is often estimated through signals such as timestamps or commit history. These are easy to compute, but they can be misleading: a formatting-only English change might make a localized page look outdated when no translation update is needed, while a small localized edit might hide the fact that the English source changed meaningfully earlier.

So the practical question became: how can we help maintainers identify pages that are *likely* to need review, without pretending that automation can fully understand translation correctness? That led to a more careful direction — compare document structure, detect meaningful content changes, and output signals that help maintainers prioritize. Experimental, lightweight, for triage, not enforcement.

**Gen AI guidance.** It is tempting to frame this as: can we build a system for AI-assisted translation? But that framing is too narrow, and maybe even misleading. LLMs and advanced translation tools already exist. Contributors may use them, teams may have different comfort levels with them, and reviewers are still responsible for the final quality regardless of how a translation was produced.

So the question shifted: less about whether Kubernetes localization should "adopt AI," more about how localization teams can make their expectations explicit. Could a lightweight template help teams describe their own AI usage rules, review expectations, terminology requirements, and human-checking practices? The goal here is not abrupt adoption of the newest tools, but helping teams explain where automation may help, where human review must remain authoritative, and how contributors can use tools responsibly without increasing reviewer burden.

**Localization team status visibility.** The broad version of this was huge: how do we know the status of every localization team? A more useful first step was smaller: can we provide a simple report that helps maintainers see which files may deserve attention first? That framing avoids claiming too much. It does not replace existing scripts, enforce synchronization rules, or define one workflow for every team. It simply gives maintainers a better planning signal.

These smaller questions were less flashy than the original big ideas, but they were more useful, and easier for the community to review.

## The real work: identifying the problem

Before this mentorship, I often thought of open source contribution as a path from issue to implementation: find a problem, build a solution, submit a PR.

But in this project, the hardest part came before implementation — identifying the problem clearly enough that a solution would not create more problems. That meant reading existing workflows, listening to mentors, understanding maintainer constraints, and separating what was technically possible from what was socially adoptable.

The important question was not simply how to use AI. It was how to understand the current workflow clearly enough that any automation, AI-based or not, could support it instead of weakening it.

That led me to a set of more practical questions:

- How do we detect outdated content without creating false alarms?
- How do we help reviewers focus on meaningful changes?
- How do teams guide AI-assisted work?
- How do we preserve human review as the final authority?
- How do we make team status visible without unfairly ranking teams?
- How do we build prototypes that are small enough to review and easy enough to remove?

Once I started thinking this way, the project felt less chaotic. Each concern pointed to a constraint. Each constraint helped narrow the problem. Each narrowed problem made the next contribution more realistic.

## What I would tell future contributors

If you are new to open source, it is natural to want to show progress quickly. But progress is not always a PR.

Sometimes progress is reading enough context to avoid repeating an old mistake. Sometimes it is asking a narrower question, writing down non-goals, or turning a broad idea into a small experiment that maintainers can safely review.

This is especially true in localization work, where technical decisions are also community decisions. A useful tool is not just one that works. It is one that fits the way people review, discuss, maintain, and trust each other's work.

I entered the project thinking mainly about what could be built. The first lesson was not about choosing the right implementation: it was about learning how to ask a question the community could actually answer together. Before trying to make a workflow faster, I first need to understand why the workflow exists. Before proposing automation, I need to understand what kind of judgment it might affect. And before opening a PR, I need to ask whether the artifact will reduce uncertainty for maintainers or simply move it onto them.

For me, that became the real starting point of the LFX mentorship. In the next post, I'll write about what happened when the problem framing started turning into something more concrete: prototypes, smaller issues, mentor feedback, and the work of shaping a contribution that fits the community.
---
name: journeys
description: Research how comparable production software handles a feature that no reference covers (the user journeys, conventions people expect, platform guidelines, common complaints, and implementation patterns) and write a sourced brief the owner decides scope from. Use only when the owner wants the research. Not scope decisions or specifications.
---

# Research a feature's journeys and conventions

Where a reference covers a feature, the reference defines it and this research does not run unless the owner asks. For a feature with no reference, the research establishes what people who use comparable tools already expect, so the feature is not built to its thinnest reading. The brief is evidence; the owner decides scope. This research can fill a conversation, so it suits a subagent: give it this skill, the feature, and a link to the mission.

## Choose references

Pick four to six production products this product's users are likely to know, in the same category and on the same platforms, including the leading tool and at least one open-source or smaller one. When the feature touches the desktop, such as files, dialogs, shortcuts, the clipboard, or accessibility, add the platform's own guidelines, such as the GNOME Human Interface Guidelines and freedesktop.org specifications on Linux.

## Research

For each reference: how a person carries out the feature from start to finish, what the app shows at each step, and how it recovers from mistakes. Prefer official documentation. Read issue trackers, forums, and reviews for what users complain about; they reveal behavior documentation takes for granted. For implementation, find how production software and established libraries do it on the agreed stack.

Stop when the references agree, normally within about twenty pages. Cite each claim with its link and the date accessed. A source you could not reach makes its claim unverified; never fill the gap from memory.

## Classify and write

Sort each behavior: **table stakes** (nearly everyone has it, or its absence draws complaints), **common**, **differentiators**, and **excluded by the product** (conflicts with the mission or pitch, naming the boundary). Classification is a finding, not a scope decision.

Adapt [the template](templates/RESEARCH.md) and save `.project/documents/development/<version>/research/YYYY-MM-DDTHH-mm-ssZ-<feature>.md`, never overwriting an earlier brief. Aim for 600 to 1,200 words of findings and sources only.

Bring the owner, separately from the brief: the recommended scope, suggestions the research surfaced, and the decisions only they can make, each with options and a recommendation.

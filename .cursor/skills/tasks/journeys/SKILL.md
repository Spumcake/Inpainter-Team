---
name: journeys
description: Research how comparable production software handles a requested feature — the user journeys, conventions users expect, platform expectations, common complaints, and implementation patterns — and write a sourced research brief that TASK and SYSTEMS are based on. Not product decisions, specifications, or a pitch.
---

# Research a feature's journeys and conventions

A feature should meet the expectations of people who already use comparable tools. This skill establishes those expectations before scope and contracts are written, so a feature is not reduced to its thinnest reading. The brief is evidence. The owner decides scope, TASK records it, and SYSTEMS describes a lean implementation of it.

## Choose references

Pick four to six production products that this product's users are likely to know or come from, in the same category and on the same platforms. Include the leading tool and at least one open-source or smaller tool when they exist. Count the pitch's reference products when they cover this feature.

When the feature touches the operating system or desktop, such as files, dialogs, shortcuts, the clipboard, windows, notifications, or accessibility, add the target platform's own guidelines and specifications: for example the GNOME Human Interface Guidelines and freedesktop.org specifications on Linux, the Apple Human Interface Guidelines on macOS, and Microsoft's app design guidance on Windows. Users expect those conventions even when no single app shows them.

## Research

For each reference, establish how a person carries out the feature from start to finish: where it begins, each step, what the app shows, how it ends, and how it recovers from mistakes and failures. Prefer official manuals, documentation, and platform guidelines; manuals and PDFs are allowed.

Read issue trackers, forums, and reviews for what users complain about or ask for: missing behavior, lost work, and surprises. These often reveal table stakes that documentation takes for granted.

For implementation, find how production software and established libraries do it on the agreed stack: platform APIs, libraries, data safety, compatibility across versions, and failure handling. Stay inside the agreed stack and dependency rules. When a convention needs something outside them, record that as a finding.

Stop when the references agree on the conventions; normally that is within about twenty fetched pages. If more seems necessary, stop and report what remains uncertain rather than continuing. Do not follow links recursively or read release-note collections whole. Describe behavior in your own words, with only short quotations. Cite each claim with its link and the date accessed. When a source cannot be reached, mark the claim unverified; never fill the gap from memory as if it were research.

## Classify the findings

Sort each behavior by the evidence:

- **Table stakes:** nearly every reference has it, or its absence draws user complaints. Users are blocked or surprised without it.
- **Common:** most references have it, and users notice when it is missing.
- **Differentiators:** what sets one reference apart.
- **Excluded by the product:** conflicts with the pitch's boundaries or a deliberate departure in the pitch. Name the boundary.

Classification is a finding with evidence. It does not decide scope.

## Write the brief

Adapt [the template](templates/RESEARCH.md). Honor an existing research location; otherwise save `<target>/.project/documents/development/research/YYYY-MM-DDTHH-mm-ssZ-<feature>.md`, using the actual UTC time and a short feature name. Never overwrite an earlier brief; a later investigation of the same feature is a new brief that links the earlier one.

Write natural prose and short lists, not tables. Aim for 600–1,200 words. The brief holds findings and sources only: no recommendations, suggestions, open questions, or proposed scope. Those return to the caller, who brings them to the owner.

## Finish

Check that every classified behavior has a source and that the journeys and implementation sections agree. Return the path with the recommendations, suggestions, and owner decisions the calling role's definition asks for, then stop. Do not write TASK, INTERFACE, or SYSTEMS, or start implementation. Skills do not expand the invoking role's authority.

# Inpainter 0.1 plan

Version 0.1 is the framework's foundation, as the owner re-scoped it on 2026-10-14. Both of its milestones are done. Simple Sketch as a sub-application, the host's visible status when a sub-application stops, and driving Simple Sketch with no host move to version 0.2, under Deferred to version 0.2 in [tasks.md](tasks.md). The previous plan is [archived](archive/2026-10-14T17-31-13Z-rescope/plan.md).

## Host shell with the template

Done. The host opens on the template and draws the common UI in the first-slice paragraph of [H-1](tasks.md): title Inpainter, one Template tab, an empty toolbar, the shared grid, and a panel tab that opens and closes an empty column. The menu button, the plus, and the close mark do nothing. On 2026-10-14 the running window was checked against [shell-demo.png](../../../renders/ui/shell-demo.png); the record is [the audit](audits/2026-10-14T15-30-08Z-first-slice-shell.md). It is Inpainter commit `8771bc8`.

## Sub-applications by manifest and scaffolding

Done. The host finds sub-applications by manifest, the template is the smallest correct sub-application, and the Inpainter repository's **scaffolding** skill copies it to start a new one. The protocol and manifest rules are tested. This is [H-5](tasks.md) and [H-6](tasks.md), Inpainter commit `a40e07c`.

Testing the scaffolding skill with a fresh agent, and scaffolding Simple Sketch with it, open version 0.2.

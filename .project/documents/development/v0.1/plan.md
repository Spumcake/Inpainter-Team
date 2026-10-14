# Inpainter 0.1 plan

Version 0.1 separates the host from the sub-application, as the owner set on 2026-10-14. The owner agreed what the first slice shows on 2026-10-14, in [H-1](tasks.md), and on that day instructed that slice to be built.

## Host shell with the template

This slice is built. The host opens on the template and draws the common UI in the first-slice paragraph of [H-1](tasks.md): title Inpainter, one Template tab, an empty toolbar, the shared grid, and a panel tab that opens and closes an empty column. The menu button, the plus, and the close mark do nothing. On 2026-10-14 the running window was checked against [shell-demo.png](../../../renders/ui/shell-demo.png). The record is [the audit](../../history/v0.1/audits/2026-10-14T15-30-08Z-first-slice-shell.md). The moves are the first-slice section of [reuse.md](reuse.md). The visible status when a sub-application has stopped is [H-2](tasks.md), in the next milestone.

## Scaffolding sub-applications

The template is the smallest correct sub-application, and the host finds sub-applications by manifest. The Inpainter repository's **scaffolding** skill copies the template to start a new one. It is complete when a fresh agent can scaffold a sub-application with it alone, and Simple Sketch then starts as a scaffold.

## Simple Sketch inside the host

This depends on the shell. Simple Sketch 1.0.1 runs as the sub-application, and the person launches only the host. [H-4](tasks.md) is the sketching journey they could already do, now inside that window. Drawing, erasing, shapes, text, and ordering stay in the host. The sub-application owns arrangement, save, and undo. Declarations reach the host through the protocol. [H-2](tasks.md), the host staying up when that program stops, is met in this milestone, because there is then a real sub-application to stop. [H-3](tasks.md) holds for the whole version: separate programs, enforced by the crates.

## Driving Simple Sketch with no host

This depends on the sub-application speaking the protocol. [H-5](tasks.md) is one test that drives it while the host is not running. That test is written when the owner asks to commit.

The first slice is done. The next milestone is not started.

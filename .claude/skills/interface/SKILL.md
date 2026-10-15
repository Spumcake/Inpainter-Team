---
name: interface
description: Document views and user flows in INTERFACE.md from visual references, or prepare prompts and generate a series of UX images with an installed image-generation skill, or revise one named view. Generation needs the owner's request. Not application implementation.
---

# Define and visualize the interface

Two outcomes: interface documentation, or images showing how someone uses the product. A request for documentation does not authorize generating images. A request for images includes the prompts needed to produce them; do not stop at prompts.

Read the mission, the pitch if there is one, INTERFACE.md, and the references the owner supplied. Every view keeps the lasting structure the mission names, such as the regions present on every screen.

## Documenting views

INTERFACE.md, at the team root, describes only views that have a visual reference: an image the owner supplied or approved, or the running reference app. Describe what the reference shows and cite it for each element. Add no control, menu, dialog, panel, color, label, or state it does not show. When the scope needs a view or state no reference shows, ask the owner for one, or offer to generate one for approval.

Keep a short sitemap: where the person starts, which action opens each view, and where it leads. Give each view a short, stable, purpose-based name and use it for its heading, prompt file, and image folder. Adapt [the template](templates/INTERFACE.md) only where needed. Plain prose and lists, no tables.

## Generating images

Find an image-generation skill in `.claude/skills/` and read its instructions before creating anything. If there is none, or it cannot run because a key or tool is missing, say so and stop. Do not write a new API integration, and do not make paid calls just to test setup.

**Start from the closest real picture.** Base each new view on the closest existing picture of the screen it changes, built only from what exists:

- a screenshot of the running product, or of the reference app the mission names;
- approved references, such as the owner's mockups and the reference's renders.

When the screen does not run yet because it combines parts that exist separately, such as one program's chrome around another's content, compose it from those references: name what each supplies and where it goes, and add nothing they do not show. When the new view adds an element of a kind that already exists, such as a dialog or a notice, include an existing one so the new element inherits its look. Each prompt describes only the change and says everything else stays as in the references. Never generate a screen from a description alone when references for it exist.

With no references at all, generate an overview first, inspect it, and use it as the base for the rest of the series.

Save one prompt per image in the format the generator needs, under `.project/models/<provider>/<model>/`. Use fictional sample data. Quality is always high; if the generator cannot do high, report it rather than downgrade. Generate the base first, then the dependent views one at a time, passing reference images explicitly, since separate requests share no context. Inspect each image for layout, legibility, and consistency, and report defects rather than looping on regeneration.

Generated images are proposals. They become references only when the owner approves them, and INTERFACE is written from them only then, taking only the changed element.

## Revising a view

Find the view's prompt and image through INTERFACE.md. Edit the prompt for the requested change and generate a new version under the same view name, keeping earlier images. Do not regenerate other views; if the revised image is a base for others, name the ones affected.

## Finish

Give the owner the image paths in viewing order, what each shows, and what is generated, inspected, or approved. Images do not prove the application works.

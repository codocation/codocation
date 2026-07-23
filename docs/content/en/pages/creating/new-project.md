---
title: New Project
---

Use the New Project wizard when the documentation is a project of its own: a standalone
docs site, a product manual, a blog.

## Create through the wizard

1. Go to `File → New → Project…` and pick "Codocation" in the generator list.
2. Choose the documentation type:
   - "Docs" - a structured documentation site with a navigation tree.
   - "Blog" - chronological posts with a listing page.
3. Fill in the project name and location.
4. Fill in the documentation title (the site name readers will see), the site id, and
   optionally a description.
5. Create the project.

## Locales in a new project

Free New Project has one editable locale and creates a valid one-locale project. When Pro is
unavailable, `Add Predefined Locale` and `Add Custom Locale` remain visible, collapsed, disabled,
and marked `Pro`. Pro enables additional locales and the contextual
`Codocation tool window → Navigation gear → Manage Locales...` workflow. Import and export are
available inside this Pro locale-management workflow, not as top-level `Tools` menu actions.

## Choosing a site id

The site id is a short technical identifier: lowercase letters, digits, and dashes. It names
the navigation file (`content/<locale>/<site id>.tree.yml`) and distinguishes sites when a project
hosts more than one. Pick something short and stable; the human-facing name is the title,
which you can change at any time. The site id itself can also be changed later, from "Edit
Documentation..." in the "Codocation" tool window; see [Navigation](../writing/navigation.md).

## Description

The description is optional and can be filled in now or later; it is a one-line summary
shown in the site footer. See [Titles, URLs, and Branding](../site/branding.md).

## What you get

The wizard scaffolds a working site and opens it: a home page, a starter page, the navigation
tree, and `codocation.yml` with sensible defaults. See
[project structure](project-structure.md) for the full inventory.

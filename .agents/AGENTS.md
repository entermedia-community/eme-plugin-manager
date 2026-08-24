# AGENTS.md

## Manager Plugin Guide

### Purpose

`plugins/manager` is the multi-site/multi-database admin console. It is only needed when a single
EME-LIB install manages more than one Website — normal single-site operation does not require it.
It is a pure HTML/xconf plugin: no `code/` (Java) folder and no `plugin.xml` bean wiring.

### Folder Map

- `html/views/sites/` Create, snapshot, restore, and remove Websites; site translation tools
- `html/views/catalogs/` Manage catalog/database instances attached to a site
- `html/views/applications/` Manage installed "apps" (bundled feature packages) per site
- `html/views/plugins/` List, view details, and add new plugins available to a site
- `html/views/data/` Generic data browser (search/list/edit) for a site's tables/fields/views
- `html/views/settings/` Cross-cutting settings: status, lists, metadata, translation, mounts, logs
- `html/views/update/` Version/update tooling
- `html/tools/` Standalone admin utilities (`snapshotall.html`, `stats.html`)
- `html/theme/` Manager's own theme/layout, separate from the main site theme
- `html/components/` Shared JS used across manager views

### What This Plugin Owns

- Site lifecycle: add/remove a Website, snapshot/restore, upload an app bundle
- Cross-site settings screens (mounts, translation, metadata, logs)
- Nothing about the content or data of an individual site's Website itself — see the relevant
  plugin (catalog for schema, finder for the app UI, etc.)

### Editing Rules

- Every page under `html/views/**` is plain HTML + a sibling `.xconf`; there is no Java to
  rebuild, so changes are visible after page cache clear/reload.
- Localized UI strings live directly in the page's `.xconf` under `<property name='text.X'>` with
  one `<value>` per locale — add new strings the same way rather than hardcoding text in the HTML.
- Keep new admin screens inside the existing section folders (`sites`, `catalogs`, `applications`,
  `plugins`, `data`, `settings`, `update`) rather than inventing new top-level folders.
- `_site.xconf` in a section folder sets defaults (title, shared properties) for every page below
  it — add there instead of repeating properties per page.

### Validation Checklist

1. Clear the page cache (or restart) after adding/removing an `.html` or `.xconf` file.
2. Load the page and confirm both the default and translated locale (`?locale=es`) render.
3. If the page performs an action (add site, restore snapshot, upgrade plugin), confirm the
   underlying operation actually completes, not just that the form renders.

### Notes For Agents

- This plugin is UI-only. If a request needs new server-side logic (not just calling existing
  path-actions/services), the Java likely belongs in `plugins/system` or `plugins/finder`, wired
  through their `plugin.xml`, not here.
- Manager is optional — don't assume it's deployed on every EME site.

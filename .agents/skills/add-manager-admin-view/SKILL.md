---
name: add-manager-admin-view
description: Use this skill when the user wants to add a new admin screen to the multi-site manager console — requests like "add a manager page to list X", "add a settings screen for Y under manager", or "add a new tab to the sites/plugins/catalogs admin". Covers creating the HTML + .xconf pair and adding localized text. Consult this before hand-adding files under plugins/manager/html/views, since the localization and section-folder conventions are specific to this plugin.
---

# Add a Manager Admin View

Adds a new page to the manager plugin's admin console. Manager is pure HTML/xconf — there is no
Java to wire.

## Step 1: Pick the section

Put the new page under the existing section it belongs to: `sites`, `catalogs`, `applications`,
`plugins`, `data`, `settings`, or `update`. Only invent a new top-level section if the feature
genuinely doesn't fit any of these (rare).

## Step 2: Create the HTML page

Add `html/views/<section>/<pagename>.html`. Follow the layout already used by sibling pages in
that section (it inherits manager's theme via `html/theme`).

## Step 3: Add the `.xconf` with localized text

Add `html/views/<section>/<pagename>.xconf`:

```xml
<page>
	<property name='sectiontitle'>
		<value>My New Page</value>
		<value locale="es">Mi Nueva Página</value>
	</property>

	<property name='text.Some Label'>
		<value>Some Label</value>
		<value locale="es">Alguna Etiqueta</value>
	</property>
</page>
```

Reference each string in the HTML by its `text.*` property name rather than hardcoding text, so
translations stay centralized. If the section's `_site.xconf` already defines a property you need
(e.g. shared title), don't redefine it per-page.

## Step 4: Validate

1. Clear the page cache (or restart) — this plugin has no Java build step.
2. Load the page in the default locale and with `?locale=es` (or another configured locale) to
   confirm both render.
3. If the page triggers an action against a site/catalog/plugin, confirm it actually completes end
   to end, not just that the form displays.

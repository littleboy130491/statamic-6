# AGENTS.md — LLM Coding Agent Instructions

## Project Overview

This is a **Statamic CMS** project (Laravel-based flat-file CMS). Before implementing any task, you must follow the conventions and patterns documented here.

**Documentation Location:** `/statamic-conventions/`

---

## ⚠️ CRITICAL FIRST STEP: Multisite Detection

**Before ANY Statamic task, detect if the project is single-site or multisite.**

### How to Detect

Check these files in order:

1. [`resources/sites.yaml`](resources/sites.yaml) (flat-file configuration — check first)
2. [`config/statamic/sites.php`](config/statamic/sites.php) (PHP configuration — fallback)

### Single Site (Default)

```yaml
# resources/sites.yaml
default:
  name: Site Name
  locale: en_US
  url: /
```

- Only ONE site key defined
- No additional site handles

### Multisite

```yaml
# resources/sites.yaml
english:
  name: English
  locale: en_US
  url: /
indonesian:
  name: Indonesian
  locale: id_ID
  url: /id/
```

- TWO OR MORE site keys defined
- Multiple site handles (e.g., `english`, `indonesian`)

### Alternative Detection Methods

**Check content folder structure:**

```bash
# Multisite — site subfolders exist:
content/collections/pages/english/
content/collections/pages/indonesian/

# Single site — entries directly in collection folder:
content/collections/pages/home.md
content/collections/pages/about.md
```

### Path Differences Summary

| Content Type | Single Site Path | Multisite Path |
|--------------|------------------|----------------|
| Entries | `content/collections/{collection}/{entry}.md` | `content/collections/{collection}/{site}/{entry}.md` |
| Collection Trees | `content/trees/collections/{collection}.yaml` | `content/trees/collections/{collection}/{site}.yaml` |
| Taxonomy Terms | `content/taxonomies/{taxonomy}/{term}.yaml` | `content/taxonomies/{taxonomy}/{site}/{term}.yaml` |
| Navigation Trees | `content/trees/navigation/{nav}.yaml` | `content/trees/navigation/{site}/{nav}.yaml` |
| Global Data | `content/globals/{global}.yaml` (with `data:` key) | `content/globals/{site}/{global}.yaml` (no `data:` key) |

**Paths that DON'T change (shared across sites):**

- `resources/blueprints/*` — All blueprints
- `resources/fieldsets/*` — All fieldsets
- `resources/forms/*` — Form configurations
- `content/collections/{collection}.yaml` — Collection config
- `content/taxonomies/{taxonomy}.yaml` — Taxonomy config
- `content/navigation/{nav}.yaml` — Navigation config
- `content/globals/{global}.yaml` — Global config (multisite: config only)

---

## Available Documentation

| Document | File | Covers |
|----------|------|--------|
| File Structure | [`statamic-conventions/file_structure.md`](statamic-conventions/file_structure.md) | File paths, folder organization, naming conventions |
| Static Pages | [`statamic-conventions/static_pages.md`](statamic-conventions/static_pages.md) | Pages collection, page templates, page hierarchy |
| Collections | [`statamic-conventions/collections.md`](statamic-conventions/collections.md) | Content collections (blog, products, etc.), routing, mounting |
| Taxonomies | [`statamic-conventions/taxonomies.md`](statamic-conventions/taxonomies.md) | Categories, tags, terms, taxonomy relationships |
| Globals | [`statamic-conventions/globals.md`](statamic-conventions/globals.md) | Site-wide settings, reusable content blocks |
| Blueprints & Fields | [`statamic-conventions/blueprints_fields.md`](statamic-conventions/blueprints_fields.md) | Fieldtypes, validation, conditional fields, fieldsets |
| Navigation | [`statamic-conventions/navigations.md`](statamic-conventions/navigations.md) | Menus, nav trees, breadcrumbs, active states |
| Forms | [`statamic-conventions/forms.md`](statamic-conventions/forms.md) | Contact forms, submissions, email notifications |
| HTML to Antlers | [`statamic-conventions/html_to_antlers_instruction.md`](statamic-conventions/html_to_antlers_instruction.md) | Converting static HTML to dynamic Antlers templates |
| Static Pages CMS Structure | [`statamic-conventions/static_pages_cms_fields_structure.md`](statamic-conventions/static_pages_cms_fields_structure.md) | Page-specific blueprints vs flexible page builder approaches |

---

## Decision Tree

```
START: Any Statamic Task

┌─ STEP 0: Detect site type (ALWAYS DO THIS FIRST)
│  └─ Check resources/sites.yaml (or config/statamic/sites.php)
│     ├─ One site defined → SINGLE SITE (use direct paths)
│     └─ Multiple sites → MULTISITE (use {site}/ subfolders)
│
└─ STEP 1: What are you building?

   ├─ A page (single, standalone content)
   │  └─ Check: static_pages.md → static_pages_cms_fields_structure.md → blueprints_fields.md
   │
   ├─ Converting HTML to Antlers
   │  └─ Check: html_to_antlers_instruction.md → static_pages_cms_fields_structure.md → blueprints_fields.md
   │
   ├─ Multiple similar items (posts, products, team)
   │  └─ Check: collections.md → blueprints_fields.md
   │  │
   │  └─ Need to categorize them?
   │     └─ Check: taxonomies.md
   │
   ├─ Site-wide settings or reusable blocks
   │  └─ Check: globals.md → blueprints_fields.md
   │
   ├─ Menu or navigation
   │  └─ Check: navigations.md
   │
   ├─ Contact form or user input
   │  └─ Check: forms.md → blueprints_fields.md
   │
   ├─ Unsure about file location
   │  └─ Check: file_structure.md FIRST
   │
   └─ Field configuration for anything
      └─ Check: blueprints_fields.md
```

---

## Task-to-Documentation Mapping

### Static Pages

**Consult when:**
- Creating or editing pages
- Setting up page hierarchy (parent/child)
- Configuring page templates
- Working with the pages collection

**Trigger phrases:** "create a page", "page template", "homepage", "about page", "page hierarchy", "parent page", "child page"

**Often combined with:**
- `static_pages_cms_fields_structure.md` — for choosing blueprint approach
- `html_to_antlers_instruction.md` — for converting HTML to templates
- `blueprints_fields.md` — for page field configuration

### Static Pages CMS Structure

**Consult when:**
- Deciding between page-specific blueprints vs flexible page builder
- Creating unique page layouts (About, Contact, Services)
- Setting up Replicator-based page builders
- Planning CMS field architecture for static pages

**Trigger phrases:** "page blueprint", "page builder", "flexible layout", "page-specific fields", "replicator sections", "unique page layout"

**Often combined with:**
- `static_pages.md` — for page structure
- `blueprints_fields.md` — for field configuration details
- `html_to_antlers_instruction.md` — for template conversion

### HTML to Antlers

**Consult when:**
- Converting static HTML templates to dynamic Antlers
- Extracting content from HTML files into CMS entries
- Wrapping HTML with Statamic variables
- Creating Antlers templates from design files

**Trigger phrases:** "convert HTML", "HTML to Antlers", "template conversion", "static to dynamic", "extract content", "make template dynamic"

**Often combined with:**
- `static_pages.md` — for page template structure
- `static_pages_cms_fields_structure.md` — for field planning
- `blueprints_fields.md` — for field types and configuration

### Collections

**Consult when:**
- Creating blog, news, or article sections
- Setting up products, services, or portfolio items
- Configuring collection routing
- Mounting collections to pages

**Trigger phrases:** "create a collection", "blog posts", "articles", "products", "portfolio", "collection route", "mount collection"

### Taxonomies

**Consult when:**
- Creating categories or tags
- Organizing content by type/topic
- Setting up filtering or faceted navigation

**Trigger phrases:** "categories", "tags", "taxonomy", "filter by", "group by"

### Globals

**Consult when:**
- Creating site-wide settings
- Setting up footer content
- Managing social media links
- Creating reusable content blocks

**Trigger phrases:** "site settings", "global", "footer content", "social links", "site-wide", "reusable content"

### Blueprints & Fields

**Consult when:**
- Adding fields to any content type
- Choosing the right fieldtype
- Setting up validation rules
- Creating conditional fields
- Building reusable fieldsets

**Trigger phrases:** "add a field", "fieldtype", "blueprint", "validation", "required field", "conditional field", "fieldset"

### Navigation

**Consult when:**
- Creating header or footer menus
- Setting up sidebar navigation
- Implementing breadcrumbs
- Working with dropdown menus

**Trigger phrases:** "navigation", "menu", "nav", "header menu", "footer links", "breadcrumbs", "dropdown"

### Forms

**Consult when:**
- Creating contact forms
- Setting up email notifications
- Handling form submissions
- Implementing spam protection

**Trigger phrases:** "form", "contact form", "email notification", "form submission", "form validation"

---

## Multi-Document Tasks

### Creating a Blog

1. **Detect multisite** — Check `resources/sites.yaml`
2. `collections.md` — Set up blog collection
3. `taxonomies.md` — Create categories and tags
4. `blueprints_fields.md` — Define post fields
5. `static_pages.md` — Create blog index page (mount point)
6. `navigations.md` — Add blog to navigation

### Building a Services Section

1. **Detect multisite** — Check `resources/sites.yaml`
2. `collections.md` — Set up services collection
3. `blueprints_fields.md` — Define service fields (Replicator for flexible content)
4. `static_pages.md` — Create services landing page
5. `globals.md` — CTA block for service pages

### Setting Up a Contact Page

1. **Detect multisite** — Check `resources/sites.yaml`
2. `static_pages.md` — Create contact page
3. `forms.md` — Build contact form
4. `globals.md` — Company contact details
5. `blueprints_fields.md` — Form field configuration

### Converting HTML Design to Statamic

1. **Detect multisite** — Check `resources/sites.yaml`
2. `html_to_antlers_instruction.md` — Conversion workflow
3. `static_pages_cms_fields_structure.md` — Choose blueprint approach
4. `blueprints_fields.md` — Define fields
5. `static_pages.md` — Create pages and templates
6. Update entry `.md` files with extracted content
7. Create `.antlers.html` templates with wrapped variables

---

## Important Conventions

1. **ALWAYS detect multisite first** — Check `resources/sites.yaml` before doing anything; paths differ significantly
2. **Blueprint handles must match content handles** — Form blueprint handle = form handle, nav blueprint handle = nav handle
3. **Partials use underscore prefix** — Filename: `_name.antlers.html`, reference: `{{ partial:name }}`
4. **Multisite changes content paths** — Content goes in `{site}/` subdirectories, config files stay shared
5. **Globals single vs multisite differ** — Single site has `data:` wrapper, multisite site files don't
6. **Collections can be mounted** — Creating URL hierarchy by mounting to pages
7. **Taxonomies attach to collections** — Configure in collection yaml, not taxonomy yaml

---

## Before Implementation Checklist

Before writing any Statamic code or configuration:

- [ ] **FIRST: Detected single-site or multisite** (check `resources/sites.yaml` or `config/statamic/sites.php`)
- [ ] If multisite: noted site handles for correct paths
- [ ] Identified the correct document(s) to consult
- [ ] Confirmed file paths in `file_structure.md` (using correct single/multisite paths)
- [ ] Checked naming conventions (handles, filenames)
- [ ] Reviewed blueprint/field requirements in `blueprints_fields.md`
- [ ] Noted any related documents for complete implementation

---

## Quick Reference by File Type

| Creating This | Check These Docs |
|---------------|------------------|
| `.yaml` in `content/collections/` | collections.md, file_structure.md |
| `.md` entry file | collections.md, static_pages.md |
| `.yaml` in `resources/blueprints/` | blueprints_fields.md, static_pages_cms_fields_structure.md |
| `.yaml` in `content/taxonomies/` | taxonomies.md |
| `.yaml` in `content/globals/` | globals.md |
| `.yaml` in `content/navigation/` | navigations.md |
| `.yaml` in `content/trees/` | navigations.md, static_pages.md |
| `.yaml` in `resources/forms/` | forms.md |
| `.antlers.html` template | html_to_antlers_instruction.md, relevant content doc + file_structure.md |
| Email template | forms.md |
| Converting HTML files | html_to_antlers_instruction.md, static_pages_cms_fields_structure.md |

---

## When Documentation Conflicts with Memory

If you have prior knowledge that conflicts with these conventions:

1. **Trust the documentation** — It reflects project-specific standards
2. **Check file_structure.md first** — Paths may differ from Statamic defaults
3. **Follow the patterns shown** — Consistency matters more than alternatives

# Post Collection Plan - WordPress-like Fields

## Project Context
- **Multisite Project**: `default` (English) and `indonesian` (Indonesian)
- **Collection Handle**: `post`
- **Purpose**: Blog posts with WordPress-like functionality
- **SEO**: Handled by SEO Pro addon (already installed)

---

## WordPress-like Fields for Post Blueprint

### Main Content Tab
| Field | Type | Description | WordPress Equivalent |
|--------|------|-------------|-------------------|
| `title` | text | Post title (built-in) | Post Title |
| `content` | bard | Rich text content with formatting | Post Content (Gutenberg/Classic) |
| `excerpt` | textarea | Short summary for listings | Post Excerpt |
| `featured_image` | assets | Main post image | Featured Image |

### Sidebar Tab
| Field | Type | Description | WordPress Equivalent |
|--------|------|-------------|-------------------|
| `author` | users | Post author | Post Author |
| `categories` | terms | Category selection | Categories |
| `tags` | terms | Tag selection | Tags |
| `featured` | toggle | Mark as featured post | Sticky Post |
| `status` | select | Draft/Published/Archived | Post Status |

### SEO
- **Note**: SEO fields are handled by SEO Pro addon (already installed)
- No custom SEO fields needed in blueprint

---

## File Structure

### Collection Configuration
```
content/collections/post.yaml
```

### Post Blueprint
```
resources/blueprints/collections/post/post.yaml
```

### Taxonomies
```
content/taxonomies/categories.yaml
content/taxonomies/tags.yaml
resources/blueprints/taxonomies/categories/categories.yaml
resources/blueprints/taxonomies/tags/tags.yaml
```

### Templates
```
resources/views/post/
├── layout.antlers.html
├── show.antlers.html
├── index.antlers.html
└── partials/
    ├── _card.antlers.html
    └── _meta.antlers.html
```

### Content (Multisite)
```
content/collections/post/
├── default/
│   ├── 2024-01-15.hello-world.md
│   └── 2024-01-20.second-post.md
└── indonesian/
    ├── 2024-01-15.halo-dunia.md
    └── 2024-01-20.postingan-kedua.md
```

### Taxonomy Terms (Multisite)
```
content/taxonomies/categories/
├── default/
│   ├── news.yaml
│   └── tutorials.yaml
└── indonesian/
    ├── berita.yaml
    └── tutorial.yaml

content/taxonomies/tags/
├── default/
│   ├── wordpress.yaml
│   └── statamic.yaml
└── indonesian/
    ├── wordpress.yaml
    └── statamic.yaml
```

---

## Collection Configuration Details

### content/collections/post.yaml
```yaml
title: Posts
route: 'blog/{slug}'
template: '@blueprint'
layout: 'post/layout'
date: true
date_behavior:
  past: public
  future: private
taxonomies:
  - categories
  - tags
sites:
  - default
  - indonesian
sort_by: date
sort_dir: desc
```

---

## Blueprint Structure

### resources/blueprints/collections/post/post.yaml

**Main Tab:**
- Title (built-in)
- Content (Bard field with formatting buttons)
- Excerpt (Textarea, 300 char limit)
- Featured Image (Assets, max 1 file)

**Sidebar Tab:**
- Author (Users field, max 1)
- Categories (Terms field, multi-select)
- Tags (Terms field, multi-select)
- Featured (Toggle field)
- Status (Select: draft, published, archived)

**SEO:**
- Handled by SEO Pro addon - no custom fields needed

---

## Taxonomy Configuration

### Categories (Hierarchical)
- Route: `categories/{slug}`
- Template: `categories/show.antlers.html`
- Fields: title, slug (built-in), description (textarea), image (assets)

### Tags (Flat)
- Route: `tags/{slug}`
- Template: `tags/show.antlers.html`
- Fields: title, slug (built-in) - minimal fields

---

## Template Structure

### layout.antlers.html
- HTML shell with head, header, footer
- `{{ template_content }}` for content injection

### show.antlers.html
- Featured image
- Title
- Meta info (date, author, categories, tags)
- Content
- Related posts

### index.antlers.html
- Post listing with pagination
- Filter by category/tag
- Search functionality

### _card.antlers.html
- Post card for listings
- Featured image, title, excerpt, date, read more link

---

## Sample Content

### Sample Post (English)
- Title: "Hello World"
- Slug: "hello-world"
- Date: 2024-01-15
- Content: Sample blog post content
- Categories: News
- Tags: WordPress, Statamic

### Sample Post (Indonesian)
- Title: "Halo Dunia"
- Slug: "halo-dunia"
- Date: 2024-01-15
- Content: Konten postingan blog sampel
- Categories: Berita
- Tags: WordPress, Statamic

---

## Implementation Steps

1. Create post collection configuration
2. Create post blueprint with WordPress-like fields
3. Create categories taxonomy configuration
4. Create categories taxonomy blueprint
5. Create tags taxonomy configuration
6. Create tags taxonomy blueprint
7. Create post layout template
8. Create post show template
9. Create post index template
10. Create post card partial
11. Create sample post entries for default site
12. Create sample post entries for indonesian site
13. Create sample category terms for both sites
14. Create sample tag terms for both sites

---

## Notes

- All content files use multisite structure with site subfolders
- Taxonomies are attached to collection via collection config
- Templates use `@blueprint` convention for auto-mapping
- Date-based entries use `{date}.{slug}.md` filename format
- All fields marked as `localizable: true` for multisite support
- SEO Pro addon handles all SEO-related fields automatically

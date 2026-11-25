---
name: technical-seo
description: Technical SEO checklist for a static site or landing page - meta tags, social cards, structured data, canonical/robots/sitemap, accessibility basics and performance - with a control script that prints OK/KO per item. Use when preparing a site for release or auditing its technical SEO.
---

# Technical SEO checklist

## Tags and metadata
- `<title>` about 60 characters at most; meta description about 155 (common guidance: engines truncate by pixel width, not a hard limit).
- Open Graph and Twitter card tags; share image at the recommended ratio (1200x630 is the common size; check current platform docs).
- `lang` attribute on `<html>`, exactly one `h1`, favicon set and web manifest.

## Structured data
- JSON-LD matching the page's real subject, with fitting schema.org types (e.g. Person or Organization, a service/business subtype, WebSite, WebPage). Never mark up content that is not visible on the page.

## Crawling and URLs
- `canonical`, `robots.txt` and `sitemap.xml` generated from one `site_url` setting and skipped while it is unset, so no wrong URLs go live.
- Custom 404 page.

## Performance
- No blocking third-party script, sized images, fonts preloaded or system fonts.

## Control
- Ship a control script printing OK/KO per item; run it in the build and before release.
- Report anything not verified honestly; do not claim OK on an item you did not check.

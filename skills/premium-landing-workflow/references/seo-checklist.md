# Technical SEO checklist

- `<title>` at most 60 characters; meta description at most 155.
- Open Graph and Twitter card tags; share image 1200x630.
- JSON-LD matching the page: Person or Organization, ProfessionalService, WebSite, WebPage.
- `canonical`, `robots.txt` and `sitemap.xml` generated from one `site_url` setting and skipped while it is unset, so no wrong URLs go live.
- Custom 404 page, web manifest, favicon set, `lang` attribute, one `h1`.
- Performance: no blocking third-party script, sized images, fonts preloaded or system.
- Ship a control script printing OK/KO per item; run it in the build and before release.

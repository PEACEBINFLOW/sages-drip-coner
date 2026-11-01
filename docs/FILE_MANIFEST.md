# Frozen repository manifest — v1

Batch 1 defines the repository. A listed file is delivered only in its assigned batch. Do not deploy until Batch 7 is complete. Paths, capitalization and extensions are fixed.

L = backend layer; F = frontend layer; D0 = documentation. Configuration shared by both sides is L0/F0. Components use sublayers F3.1 through F3.6. Generated dist/ and node_modules/ are not committed. No photos or binary image files are committed.

| Path | Layer | Batch | Purpose |
| --- | --- | --- | --- |
| README.md | D0 | 1 | Beginner setup and deployment |
| docs/FILE_MANIFEST.md | D0 | 1 | Frozen file inventory |
| docs/DATA_MODEL.md | D0 | 1 | Tables, constraints and indexes |
| docs/API_CONTRACT.md | D0 | 1 | Requests, responses and errors |
| docs/ARCHITECTURE.md | D0 | 1 | Dependencies and resource budgets |
| docs/ADMIN_GUIDE.md | D0 | 1 | Daily publishing instructions |
| package.json | L0/F0 | 2 | Dependencies and scripts |
| .gitignore | L0/F0 | 2 | Ignore generated files and secrets |
| .nvmrc | L0/F0 | 2 | Node 22 |
| wrangler.jsonc | L0 | 2 | Worker, assets and storage bindings |
| vite.config.js | F0 | 2 | React production build |
| tailwind.config.js | F0 | 2 | Map semantic CSS tokens |
| postcss.config.js | F0 | 2 | Tailwind and Autoprefixer |
| index.html | F0 | 2 | HTML shell, fonts and default metadata |
| public/favicon.svg | F0 | 2 | Hand-written vector favicon |
| public/robots.txt | F0 | 2 | Crawler rules |
| public/_headers | F0 | 2 | Static response security headers |
| db/00_reset.sql | L1 | 2 | Optional empty-database reset |
| db/01_schema.sql | L1 | 2 | Create schema version 1 |
| db/02_seed_core.sql | L1 | 7 | Required slots, tags and house creator |
| db/03_seed_demo.sql | L1 | 7 | Optional clearly labeled demo content |
| worker/lib/http.js | L2 | 3 | JSON envelopes, headers and cache helpers |
| worker/lib/db.js | L2 | 3 | Execute prepared statements and batches |
| worker/lib/auth.js | L2 | 3 | Hashes, cookies, signatures and Origin checks |
| worker/lib/validate.js | L2 | 3 | Validate bounded input |
| worker/lib/slug.js | L2 | 3 | Slug normalization |
| worker/lib/pagination.js | L2 | 3 | Scoped cursor encoding and validation |
| worker/lib/mediaUrls.js | L2 | 3 | Final media URLs and variant widths |
| worker/lib/router.js | L2 | 3 | Method and path matching |
| worker/queries/site.js | L3 | 3 | Site slots and sitemap queries |
| worker/queries/creators.js | L3 | 3 | Creators, links and creator images |
| worker/queries/posts.js | L3 | 3 | Posts, tags and post images |
| worker/queries/items.js | L3 | 3 | Items, tags and item images |
| worker/queries/reviews.js | L3 | 3 | Reviews and review images |
| worker/queries/tags.js | L3 | 3 | Tag records |
| worker/queries/lookbooks.js | L3 | 3 | Lookbooks and ordered images |
| worker/queries/media.js | L3 | 3 | Media, usage, tags and cleanup queue |
| worker/queries/auth.js | L3 | 3 | Failed-login throttle |
| worker/routes/public/home.js | L4 | 3 | Home sections in one response |
| worker/routes/public/drip.js | L4 | 3 | Drip feed and post detail |
| worker/routes/public/shop.js | L4 | 3 | Shop feed and item detail |
| worker/routes/public/creators.js | L4 | 3 | Creators and their mini-blogs |
| worker/routes/public/reviews.js | L4 | 3 | Reviews and review-photo detail |
| worker/routes/public/lookbooks.js | L4 | 3 | Lookbook feed and detail |
| worker/routes/public/site.js | L4 | 3 | Brand and site images |
| worker/routes/public/tags.js | L4 | 3 | Public tag picker |
| worker/routes/admin/session.js | L4 | 3 | Login, session and logout |
| worker/routes/admin/dashboard.js | L4 | 3 | Counts and upcoming content |
| worker/routes/admin/creators.js | L4 | 3 | Creator and social-link editing |
| worker/routes/admin/posts.js | L4 | 3 | Post editing |
| worker/routes/admin/items.js | L4 | 3 | Item editing |
| worker/routes/admin/reviews.js | L4 | 3 | Review editing |
| worker/routes/admin/tags.js | L4 | 3 | Tag editing |
| worker/routes/admin/lookbooks.js | L4 | 3 | Lookbook editing |
| worker/routes/admin/media.js | L4 | 3 | Uploads, replacement and safe deletion |
| worker/routes/admin/slots.js | L4 | 3 | Named site image spaces |
| worker/routes/mediaServe.js | L4 | 3 | Stream R2 images |
| worker/routes/share.js | L4 | 3 | HTML share metadata |
| worker/routes/sitemap.js | L4 | 3 | Live-content XML sitemap |
| worker/index.js | L5 | 3 | Route registration, authentication and errors |
| src/config/brand.js | L0/F0 | 4 | Shared brand and contact constants |
| src/config/site.js | F0 | 4 | Routes, enums, limits and UI options |
| src/lib/api.js | F1 | 4 | Public HTTP requests |
| src/lib/adminApi.js | F1 | 4 | Admin requests and upload progress |
| src/lib/format.js | F1 | 4 | Pula and CAT date formatting |
| src/lib/seo.js | F1 | 4 | Browser page metadata |
| src/lib/imageHelpers.js | F1 | 4 | Browser resizing and colour sampling |
| src/hooks/useApi.js | F2 | 4 | Loading, errors, cancellation and refetch |
| src/hooks/useInView.js | F2 | 4 | Scroll visibility |
| src/hooks/useAdminSession.js | F2 | 4 | Session lifecycle |
| src/components/ui/Button.jsx | F3.1 | 4 | Accessible button or link |
| src/components/ui/Chip.jsx | F3.1 | 4 | Filter and status chip |
| src/components/ui/Spinner.jsx | F3.1 | 4 | Loading indicator |
| src/components/ui/EmptyState.jsx | F3.1 | 4 | Empty-content message |
| src/components/ui/ErrorNotice.jsx | F3.1 | 4 | Error and retry message |
| src/components/ui/Field.jsx | F3.1 | 4 | Labeled form field |
| src/components/ui/Tabs.jsx | F3.1 | 4 | Keyboard-accessible tabs |
| src/components/ui/Icon.jsx | F3.1 | 4 | Small inline SVG icon set |
| src/components/decor/GrainOverlay.jsx | F3.2 | 4 | Decorative grain layer |
| src/components/decor/DripDivider.jsx | F3.2 | 4 | Inline SVG divider |
| src/components/decor/Wordmark.jsx | F3.2 | 4 | Brand wordmark |
| src/components/decor/Marquee.jsx | F3.2 | 4 | Reduced-motion-aware strip |
| src/components/decor/SectionLabel.jsx | F3.2 | 4 | Editorial section number and title |
| src/components/media/ImageFrame.jsx | F3.3 | 4 | Universal responsive image renderer |
| src/components/media/Placeholder.jsx | F3.3 | 4 | Designed no-photo state |
| src/components/media/MasonryGrid.jsx | F3.3 | 4 | Responsive gallery arrangement |
| src/components/media/Gallery.jsx | F3.3 | 4 | Ordered image gallery |
| src/components/media/Lightbox.jsx | F3.3 | 4 | Focus-trapped photo viewer |
| src/components/cards/PostCard.jsx | F3.4 | 4 | Drip and article preview |
| src/components/cards/ItemCard.jsx | F3.4 | 4 | Seller item preview |
| src/components/cards/CreatorCard.jsx | F3.4 | 4 | Creator preview |
| src/components/cards/ReviewCard.jsx | F3.4 | 4 | Attributed review preview |
| src/components/cards/LookbookCard.jsx | F3.4 | 4 | Lookbook preview |
| src/components/layout/SiteLayout.jsx | F3.5 | 4 | Public page shell |
| src/components/layout/Header.jsx | F3.5 | 4 | Desktop navigation |
| src/components/layout/Footer.jsx | F3.5 | 4 | Contacts and doctrine |
| src/components/layout/MobileMenu.jsx | F3.5 | 4 | Mobile navigation |
| src/components/admin/AdminLayout.jsx | F3.6 | 6 | Authenticated admin shell |
| src/components/admin/MediaPicker.jsx | F3.6 | 6 | Shared library selection dialog |
| src/components/admin/ImageUploader.jsx | F3.6 | 6 | Queued image conversion and upload |
| src/components/admin/ImageSetEditor.jsx | F3.6 | 6 | Roles and ordered image selection |
| src/components/admin/SchedulePicker.jsx | F3.6 | 6 | Explicit CAT scheduling |
| src/components/admin/MarkdownPreview.jsx | F3.6 | 6 | Safe post preview |
| src/components/admin/EntityTable.jsx | F3.6 | 6 | Admin list and actions |
| src/components/admin/ConfirmDialog.jsx | F3.6 | 6 | Destructive-action confirmation |
| src/pages/public/Home.jsx | F4 | 5 | Magazine cover and highlights |
| src/pages/public/Drip.jsx | F4 | 5 | Filtered drip feed |
| src/pages/public/PostPage.jsx | F4 | 5 | Post, article and gallery |
| src/pages/public/Shop.jsx | F4 | 5 | Seller-linked catalogue |
| src/pages/public/ItemPage.jsx | F4 | 5 | Item, gallery and reviews |
| src/pages/public/Creators.jsx | F4 | 5 | Creator directory |
| src/pages/public/CreatorPage.jsx | F4 | 5 | Creator mini-blog |
| src/pages/public/Reviews.jsx | F4 | 5 | Social review collection |
| src/pages/public/Lookbooks.jsx | F4 | 5 | Lookbook directory |
| src/pages/public/LookbookPage.jsx | F4 | 5 | Lookbook gallery |
| src/pages/public/About.jsx | F4 | 5 | Story and Community Doctrine |
| src/pages/public/Contact.jsx | F4 | 5 | Call, WhatsApp and social links |
| src/pages/public/NotFound.jsx | F4 | 5 | Missing-page screen |
| src/pages/admin/AdminLogin.jsx | F4 | 6 | Shared-passphrase login |
| src/pages/admin/AdminDashboard.jsx | F4 | 6 | Publishing overview |
| src/pages/admin/AdminCreators.jsx | F4 | 6 | Creators and social links |
| src/pages/admin/AdminPosts.jsx | F4 | 6 | Posts and article editor |
| src/pages/admin/AdminItems.jsx | F4 | 6 | Seller items editor |
| src/pages/admin/AdminReviews.jsx | F4 | 6 | Social review editor |
| src/pages/admin/AdminTags.jsx | F4 | 6 | Tags editor |
| src/pages/admin/AdminLookbooks.jsx | F4 | 6 | Lookbook editor |
| src/pages/admin/AdminMediaLibrary.jsx | F4 | 6 | Central media workspace |
| src/pages/admin/AdminSiteSlots.jsx | F4 | 6 | Site image assignments |
| src/App.jsx | F5 | 4 | Public routes and lazy admin routes |
| src/main.jsx | F5 | 4 | React entry point |
| src/styles/index.css | F5 | 4 | Design tokens and global styles |

Documentation additions: this manifest. Reliability addition: media_cleanup is a table, not another file; existing media query and handler files own it. There are no unlisted image folders, generated source files, migrations, tests or lockfiles in this hand-copy release. Later dependency updates must be reviewed and rebuilt; caret ranges do not provide an immutable dependency lock.

# SAGE'S DRIP CONER

WHERE BOTSWANA GETS ITS DRIP.

An image-first fashion magazine from SageWorks Media, based in Maun, Botswana. Founder: Gift Thabiwa (@sage.sa4). Gift and his brother publish the content. Visitors browse outfits, creator mini-blogs, lookbooks, attributed social reviews and items that link to sellers.

There are no visitor accounts, comments, likes, carts or payments. Text is stored in Cloudflare D1; photos are uploaded through the admin screen into Cloudflare R2. Public views never write to the database.

## Read this before starting

This repository is delivered in seven batches. Batch 1 contains documentation only. Add each batch to GitHub, but connect Cloudflare Builds only after Batch 7. Otherwise missing files will cause the build to fail.

Assumption: both admins use one shared passphrase; Posting as starts with Gift and Brother. The name is spelled CONER exactly as supplied.

The target is operation within Cloudflare's free allowances. R2 requires activation of its subscription and can charge for usage beyond its included allowance. It is not a hard zero-cost cap. Choose Workers Free and R2 Standard; check usage and billing in your own dashboard. A custom domain is optional and may cost money. No paid upgrade is required by this build.

No photos go into GitHub. Do not upload images during repository assembly. The empty site uses designed placeholders. The only image-like repository asset is a hand-written SVG favicon; decorative grain and dividers are code.

## 1. Create your GitHub repository

1. Sign in at https://github.com.
2. Click the + menu, then New repository.
3. Name it sages-drip-coner. Private is fine; Cloudflare will request access to this repository.
4. Create the repository. Do not choose a template or generate another application.
5. In an empty repository, click creating a new file. After the first file, use Add file > Create new file.
6. Type the exact full path, for example docs/DATA_MODEL.md. GitHub creates folders from slashes; it cannot create empty folders.
7. Copy only the complete contents inside that file's outer code fence. Do not copy the heading or outer backticks.
8. Click Commit changes. Commit to main. Repeat for every file in the batch.
9. Complete all seven batches and compare your file list with docs/FILE_MANIFEST.md. Do not commit node_modules, dist, secrets or photos.

The four-backtick wrapper around Markdown files is only a delivery wrapper. Their inner code fences belong in the actual Markdown file.

## 2. Create your Cloudflare account

1. Open https://dash.cloudflare.com and sign up or sign in.
2. Verify your email if asked.
3. Keep the Workers plan on Free. You can use the supplied workers.dev address without buying a domain.

Cloudflare occasionally changes menu wording. Use the named product if a sidebar group has moved.

## 3. Create D1

1. Open Storage & databases > D1 SQL Database.
2. Click Create database.
3. Name it sages-drip-coner-db and create it.
4. Open its overview and copy the Database ID, a UUID. Copy the database ID, not the account ID.
5. In GitHub, open wrangler.jsonc and click the pencil to edit.
6. Replace only the clearly marked database_id placeholder with your real ID, keeping the quotation marks.
7. Keep binding DB and database_name sages-drip-coner-db unchanged. Commit.

## 4. Create the image bucket

1. In Cloudflare, open Storage & databases > R2 > Overview.
2. If asked, complete the R2 activation/checkout. Review its billing terms before enabling it; the free allowance does not block overage charges.
3. Click Create bucket. Name it sages-drip-coner-media.
4. Choose Standard storage. Keep public r2.dev access disabled. The Worker will serve photos through /media/ initially.
5. Create the bucket. Leave it empty; do not upload photos here by hand.

## 5. Initialize D1 in the console

Do this after every file, including the Batch 7 SQL files, has been added. Open your database > Console. Open each SQL file in GitHub, select Raw, copy its contents, paste into the D1 console and click Execute/Run.

If the console accepts one statement at a time, paste each complete semicolon-terminated statement in order. Do not paste Markdown fences. Stop on the first error instead of continuing into the next file.

| Order | File | Action and expected result |
| --- | --- | --- |
| Optional 00 | db/00_reset.sql | Skip during normal setup. Only for a confirmed empty/disposable database. It drops the schema and cannot recover content. A successful reset leaves no application tables. |
| 01 | db/01_schema.sql | Creates 21 application tables and their indexes. Expect successful queries, then schema_meta containing schema_version = 1. |
| 02 | db/02_seed_core.sql | Required. Expect six site slots, eight tags and one house creator named SageWorks Media. No media rows. |
| 03 | db/03_seed_demo.sql | Optional. Adds two fictional creators, three posts, three items, two reviews and one lookbook, all as drafts. No photos. |

The console's rows-written totals can include indexes and differ from logical record counts. The checks below are clearer. Run these in the D1 console after the applicable file:

```sql
SELECT value FROM schema_meta WHERE key = 'schema_version';
SELECT COUNT(*) AS slots FROM site_slots;
SELECT COUNT(*) AS tags FROM tags;
SELECT COUNT(*) AS house_creators FROM creators WHERE kind = 'house';
SELECT COUNT(*) AS photos FROM media_assets;
```

Expected after 02: 1, 6, 8, 1, 0 respectively. After optional 03, creators total 3, posts 3, items 3, reviews 2 and lookbooks 1. The public site does not display the fictional drafts. Seeds can be rerun without overwriting your edits. Never use reset to retry a seed or fix a live site.

## 6. Connect GitHub using Workers Builds

1. Open Workers & Pages > Create application.
2. Choose the Worker option for importing/connecting a Git repository. Do not create a Pages project.
3. Connect GitHub and allow access to sages-drip-coner only.
4. Select that repository and main branch.
5. Set Worker/project name to sages-drip-coner, matching wrangler.jsonc.
6. Set Root directory to the repository root; leave blank if blank means root.
7. Set Build command to npm run build.
8. Set Deploy command to npx wrangler deploy.
9. Keep the normal dependency installation step enabled. .nvmrc selects Node 22.
10. Start deployment and wait for a successful build and deploy. Build compiles React into dist; Wrangler deploys dist and worker/index.js together.

Those two commands are entered into Cloudflare's form. You do not run a terminal command yourself. An output-directory field from a Pages wizard is a sign you selected the wrong product; Worker static assets are configured as ./dist in wrangler.jsonc.

If initial deployment finishes before secrets are configured, public content can load but admin login intentionally reports unavailable. Add the secrets next.

## 7. Add secrets and check bindings

Open Workers & Pages > sages-drip-coner > Settings > Variables & Secrets. These must be runtime secrets, not Build variables.

1. Click Add. Choose Secret. Name it ADMIN_PASSPHRASE. Use a long, unique passphrase known only to Gift and his brother. Save.
2. Add a second Secret named SESSION_SECRET. Use your password manager to generate a separate random value of at least 32 random bytes, such as 64 random hexadecimal characters. Save it privately.
3. Use Cloudflare's Save/Deploy action if shown, so both secrets reach the active Worker version.
4. Open the Worker's Bindings section and verify the three connections below. Wrangler creates them from the repository configuration; do not invent different binding names.

| Name | Must connect to |
| --- | --- |
| DB | D1 database sages-drip-coner-db with your real database ID |
| MEDIA | R2 bucket sages-drip-coner-media |
| ASSETS | Static assets from dist |

Never paste either secret into GitHub, wrangler.jsonc, chat, browser code or a screenshot. To revoke all existing sessions later, change SESSION_SECRET and deploy. Changing only ADMIN_PASSPHRASE does not invalidate already-signed cookies.

## 8. Optional variables

These are ordinary configuration values, not secrets. Their source of truth is the vars section in wrangler.jsonc; commit edits there so later builds preserve them.

| Name | Initial value | When to change |
| --- | --- | --- |
| MEDIA_BASE_URL | Empty string | Later, an HTTPS R2 custom-domain origin with no path or trailing slash |
| SITE_URL | Empty string | Optional HTTPS canonical site origin with no path or trailing slash |

Empty MEDIA_BASE_URL serves photos through the Worker. Empty SITE_URL uses the current request origin for sitemap and preview URLs. Do not put the API URL into either value.

For a later R2 custom domain: attach a domain you control in the bucket's Settings > Custom Domains, wait until active, then set MEDIA_BASE_URL to its HTTPS origin and commit. Do not use r2.dev as the production shortcut. Existing database keys remain valid; newly returned image URLs use the domain. This reduces application Worker image requests; it does not eliminate R2 usage charges. Allow API caches to refresh.

## 9. Open the site and publish the first images

1. Open the HTTPS workers.dev address shown in the Worker overview. Save that exact address; your account determines its final hostname.
2. Confirm the home page loads with intentional placeholders.
3. Add /admin to the address and log in with ADMIN_PASSPHRASE.
4. Choose Posting as: Gift or Brother.
5. Open Media Library > Upload images. Choose a few photos from your phone/computer, enter descriptive alt text for each, and upload.
6. Wait until each photo says Done. The browser creates three smaller versions; the photos are stored in R2 automatically.
7. Open Site Image Slots, choose Home hero, click Choose images, select a photo and Save images.
8. Add a portrait to Home hero mobile when available. Add About poster and Default share image next.
9. Open Creators and create a real creator, or edit SageWorks Media. Set status Ready and save. Choose avatar/banner images using the shared Media Picker.
10. Open Posts > New post. Select the creator, enter title and alt-ready cover selection, choose Ready and Publish now, then Save post.
11. After both content and images save, open the public site. Allow about a minute, sometimes roughly 90 seconds, for caches to refresh.

Read docs/ADMIN_GUIDE.md for items, lookbooks, reviews, scheduling and image reuse. Upload only images intended for public publication. Draft and Archive are editorial states, not photo privacy controls.

## Everyday limits and troubleshooting

Public GET requests never write D1 rows. Failed admin login attempts write only their throttle record. All other changes come from authenticated admins. No visitor counters or analytics ledger exists.

Check D1 Metrics and Workers usage periodically. Images served through /media/ also invoke the Worker, even when an edge image cache avoids an R2 read. Browser image caching and lazy loading reduce repeat requests. R2 storage includes all three variants. Pending file cleanup is visible in Media Library > Retry cleanup.

| What you see | What to do |
| --- | --- |
| Build says a file/import is missing | Finish all batches; check exact spelling/capitalization against FILE_MANIFEST.md. |
| Build/deploy reports a database or bucket binding error | Check the real database ID, bucket name and Worker name in wrangler.jsonc. |
| Login says service unavailable | Check both runtime secrets, DB binding, and that 01_schema.sql ran. |
| Too many login attempts | Wait until the 15-minute failure window ends. Both admins may share the same IP throttle on one network. |
| Image upload fails | Keep the successful photos; retry the failed one. Use JPEG/PNG/WebP, check connection and alt text. |
| Image is still in use | Open Where used and remove every attachment before Delete permanently. Archive is available without removing attachments. |
| Public page looks old | Wait roughly 90 seconds and reload; caches are not globally purged on save. |
| WhatsApp preview looks old | Confirm the public URL is live; the social platform may cache previews longer than the site. |
| Quota/storage unavailable | Check Cloudflare usage and billing. Free D1 daily quota exhaustion lasts until the next reset; do not erase data to fix it. |

Cache API storage is local to each data centre. The app cannot promise instant global changes. An already-open page needs refresh; a scheduled post does not push itself into an old feed.

Edit branding, contacts, social links and ADMIN_NAMES in src/config/brand.js. Instagram starts empty and hidden. Change the wordmark spelling in its single name constant. Do not replace branding across unrelated files.

## Deferred features

The self-organising menu would later use a scheduled Worker handler and dedicated queries. No visitor event tracking is included now. AI captions/tagging are also deferred. wrangler.jsonc contains only commented cron and AI-binding stubs.

## Official platform references

- Workers Builds settings: https://developers.cloudflare.com/workers/ci-cd/builds/configuration/
- Static assets: https://developers.cloudflare.com/workers/static-assets/
- D1 limits: https://developers.cloudflare.com/d1/platform/limits/
- D1 pricing: https://developers.cloudflare.com/d1/platform/pricing/
- R2 activation: https://developers.cloudflare.com/r2/get-started/
- R2 pricing: https://developers.cloudflare.com/r2/pricing/
- Runtime secrets: https://developers.cloudflare.com/workers/configuration/secrets/
- Cache behavior: https://developers.cloudflare.com/workers/reference/how-the-cache-works/

Check the dashboard before launch. Cloudflare currently documents D1 batch() as one counted query/subrequest for the per-invocation limit; this project still caps actual statements conservatively and counts all underlying row work.

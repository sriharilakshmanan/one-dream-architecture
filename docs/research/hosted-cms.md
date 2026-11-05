# Research: hosted CMS options for non-technical staff

Ticket: #7 (map: #1). Researched 2026-11-05 from vendor docs and pricing pages. Prices in USD as the vendors list them. Pricing pages change often, so check them again before paying.

## Question

Which hosted CMS should Firm staff (with no developer on hand) use to edit About, Services, Projects (lots of photography), Testimonials and the Webinar calendar? We compared ease of editing, free or cheap tier limits, modelling a Webinar (date and time with timezone, INR price, seat limit, description), image handling, preview, rebuild webhooks and fit with common frontend stacks. One solo developer, launching 2026-11-16.

## Answer

**Recommendation: Prismic, on the Starter plan ($10/mo billed annually, 3 users).** Runner-up: **Sanity Free** ($0, 20 seats). Decap, Storyblok and Contentful are out for this project.

- **Prismic** is the most editor-friendly. Its visual Page Builder, previews, scheduling and releases come with every plan. Documents and assets are unlimited, and images go through imgix with automatic AVIF/compression. Webhooks have documented recipes for Vercel and Netlify deploy hooks. The catch is that **the Free plan allows 1 user**, so more than one staff editor means Starter.
- **Sanity Free** costs nothing and gives 20 seats, 10k documents and 100 GB of assets. Visual Editing/Presentation preview and 2 webhooks (enough for one deploy hook) are included. The Studio date field can show times in a fixed zone (for example `Asia/Kolkata`), which helps staff enter Webinar times. The costs: the developer writes the schema in code and deploys the Studio, scheduled publishing needs the paid plan, and the editing UI is more of a form tool than a page builder.

Pick Sanity instead if cost has to be zero with several editors, or if staff only fill in structured records and never lay out pages.

## Comparison

| | Prismic | Sanity | Storyblok | Contentful | Decap |
|---|---|---|---|---|---|
| Free tier users | **1** | 20 | 1 (up to 2) | 10 (search result, not verified on page) | Unlimited (Git accounts) |
| Next tier | Starter $10/mo annual, 3 users; Small $25, 7 users | Growth $15/seat/mo | Growth $99/mo (≈$90.75 annual), 5 seats | Lite $300/mo, 20 users (search result) | n/a (open source) |
| Free content/asset limits | Unlimited docs, types and assets; 4M API calls, 100 GB CDN/mo; project **locks** if bandwidth exceeded | 10k docs, 100 GB assets, 100 GB bandwidth, 1M CDN requests/mo, 2 public datasets | 20k stories, **2,000 assets** (500 MB each), 100k API requests, 100 GB traffic | 25 content types, 10k records, 100k API calls, 50 GB bandwidth, hard caps (search result) | Limited only by the Git repo/host |
| Visual editing / preview | Page Builder, previews and shareable previews on all plans | Live Preview, Visual Editing, Presentation on all plans | Visual editor on all plans | Live preview (not verified, page blocked) | Real-time preview pane |
| Scheduling on free | Yes | No (Growth) | No | Not verified | No (editorial workflow only) |
| Rebuild webhooks on free | Yes, publish/unpublish triggers; no count limit documented | 2 | 3 | Not verified | Every save is a Git commit, so the host rebuilds |
| Image handling | imgix, `auto=compress,format`, responsive crops per field | CDN with URL transforms, focal-point cropping | Asset CDN (transforms not checked) | Not verified | Files committed to the repo; only resize/convert in the browser before upload |
| Official framework guides | Next.js, Nuxt, SvelteKit (React/Vue/Svelte SDKs) | Not checked this round | Not checked | Not checked | Any static site generator |

## Modelling a Webinar

Every candidate stores date-times as **UTC instants**, and none stores the zone with the value. So:

- Store `startsAt` as a timestamp/datetime field. Add an IANA `timezone` text or select field (defaulting to `Asia/Kolkata`) if Webinars can be hosted in other zones. Otherwise hard-code IST in the frontend when formatting.
- `priceInr` and `seatLimit` are plain Number fields. Storyblok's Number field supports min/max/decimals. Store INR as whole rupees, or paise if fractions are ever needed.
- `description` is rich text.
- **Keep Registrants, payments and seats remaining out of the CMS.** The CMS holds only the published seat *limit*. Live counts belong in the booking/payment system (a separate ticket), because a CMS edit plus a rebuild is too slow and too easy to break as a source of truth.

How each CMS handles timezone on input:

- **Prismic:** Timestamp is "always UTC (+0 offset)", returned as `YYYY-MM-DDTHH:MM:SS+0000`. The docs do **not** say how an editor's local time is converted on input. Check this in a test repo on day 1, because staff in India entering "7 pm" must not end up with 7 pm UTC.
- **Sanity:** stored as an ISO UTC string. `displayTimeZone: 'Asia/Kolkata'` makes staff view and edit in IST whatever their browser zone is. This is the clearest story of the five.
- **Storyblok:** the UI uses the timezone set in the editor's account, and the API returns UTC.

## Why not the others

- **Storyblok:** a good visual editor, but the free tier has 1 seat and **2,000 assets**, which a photography-heavy Projects section could reach. The next step up is $99/mo, with no scheduling on free.
- **Contentful:** a capable product, but the free tier is hard-capped at 100k API calls and 50 GB/mo, with delivery blocked when the cap is hit. The first paid tier is $300/mo, and the editing UI is the least friendly for non-technical staff. These figures could not be confirmed on the live pricing page (see below).
- **Decap:** free and Git-based, but uploaded photos are committed into the repo (`media_folder`), so a large photo library bloats the repo. Editors need a Git-provider login, or Git Gateway with an identity service such as Netlify Identity or self-run GoTrue. There's no image CDN unless you add Cloudinary or Uploadcare. That is too much setup for 11 days and for staff without a developer.

## Not verified

- **Contentful's pricing and help pages** returned HTTP 429 or a Vercel bot checkpoint every time. Its numbers (10 users, 10k records, 100k API calls, 50 GB, Lite $300/mo with 20 users) come from search-result snippets of contentful.com pages, not a page load. One snippet said the pricing data was about 11 months old.
- **Prismic's monthly (non-annual) prices.** The page shows only annual rates.
- **How Prismic's editor converts a local time on input** (see above).
- **Whether Netlify Identity is still offered** for Decap. Decap's Git Gateway page shows no deprecation notice, but Netlify's own status wasn't checked.
- **Sanity, Storyblok and Contentful framework support pages** were not read this round. All three are widely used with Next.js and Astro, but that's background knowledge, not a cited fact.

## Sources

- Prismic pricing: https://prismic.io/pricing
- Prismic Timestamp field: https://prismic.io/docs/fields/timestamp
- Prismic Image field: https://prismic.io/docs/fields/image
- Prismic webhooks: https://prismic.io/docs/webhooks
- Prismic docs home (frameworks; "Slice Machine is deprecated. New projects use the Type Builder and the Prismic CLI."): https://prismic.io/docs
- Sanity pricing: https://www.sanity.io/pricing
- Sanity datetime type: https://www.sanity.io/docs/studio/datetime-type
- Sanity image URLs: https://www.sanity.io/docs/apis-and-sdks/image-urls
- Sanity Studio deployment (`*.sanity.studio`, browser-only): https://www.sanity.io/docs/studio/deployment
- Storyblok pricing: https://www.storyblok.com/pricing
- Storyblok fields: https://www.storyblok.com/docs/concepts/fields
- Contentful pricing (blocked; via search): https://www.contentful.com/pricing/
- Contentful usage limits (via search): https://www.contentful.com/help/admin/usage/usage-limit/
- Contentful Free plan FAQ (via search): https://www.contentful.com/help/admin/billing-subscription/contentful-free-plan/
- Decap intro: https://decapcms.org/docs/intro/
- Decap Git Gateway: https://decapcms.org/docs/git-gateway-backend/
- Decap configuration options: https://decapcms.org/docs/configuration-options/

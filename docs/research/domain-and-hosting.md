# Research: buying the domain, who controls DNS, and where to host

For [Task: buy the domain](https://github.com/sriharilakshmanan/one-dream-architecture/issues/8). Researched on 2026-11-15, and every price was checked that day. Prices are in USD per year unless stated otherwise.

## TL;DR

1. **Register `onedreamarchitecture.com` at Cloudflare Registrar.** It was unregistered on 2026-11-15 (`whois`: "No match"; [Vercel availability API](https://api.vercel.com/v1/registrar/domains/onedreamarchitecture.com/availability) returned `{"available":true}`). Cloudflare sells at cost, with no markup on renewal ([cloudflare.com/products/registrar](https://www.cloudflare.com/products/registrar/)), about **$10.46/yr** ([cfdomainpricing.com](https://cfdomainpricing.com/), a third-party tracker; the official price only appears at checkout). It also includes free WHOIS redaction, one-click DNSSEC and hardware-key 2FA.
2. **DNS lives at Cloudflare too.** You have no choice here: domains registered at Cloudflare must use Cloudflare nameservers ([Cloudflare Registrar FAQ](https://developers.cloudflare.com/registrar/faq/)). That's acceptable because Cloudflare DNS is free and good, and the host only needs a CNAME or A record. Keep **hosting separate** from the registrar and DNS unless the host is also Cloudflare.
3. **Ownership: the Firm's proprietor owns the Cloudflare account and is the registrant. The repo owner is added as a member.** Don't register it in the developer's personal account.
4. **If the Firm wants `.in`:** Cloudflare and Vercel can't sell it (Cloudflare's [TLD list](https://www.cloudflare.com/tld-policies/) doesn't include it, and the [Vercel API](https://api.vercel.com/v1/registrar/tlds/in/price) returns `tld_not_supported`). Use **Porkbun** at $7.83/yr with no promo trap ([porkbun.com/products/domains](https://porkbun.com/products/domains)). `onedreamarchitecture.in` was also available on 2026-11-15 (NIXI WHOIS). If the budget allows, buy `.in` as a redirect to `.com` to protect the name.
5. **Hosting depends on the stack, which isn't decided yet:**
   - **Next.js with Prismic previews and revalidation on publish:** use **Vercel Pro at $20/mo**. Hobby isn't allowed, because its terms say "personal or non-commercial use" only ([vercel.com/legal/terms](https://vercel.com/legal/terms)), and a site that sells paid Webinars is commercial. The cheaper alternative is Netlify or Cloudflare with an adapter (see Q4).
   - **Mostly static site that rebuilds on publish:** use a **static export (or Astro and similar) on Cloudflare Pages/Workers, free**. That puts the host, DNS and registrar in one account. Prismic triggers rebuilds through a deploy hook ([CF deploy hooks](https://developers.cloudflare.com/pages/configuration/deploy-hooks/)).
6. **Gateway activation and WhatsApp no longer block this domain in v1.** webinar.gg is the merchant ([Decide: payment gateway](https://github.com/sriharilakshmanan/one-dream-architecture/issues/13)), and the message channel is email sent by webinar.gg ([Decide: message channel](https://github.com/sriharilakshmanan/one-dream-architecture/issues/15)). The domain is still needed for the launch URL and the Firm's own mailbox. It would also be needed for webinar.gg sending from the Firm's domain, if webinar.gg supports that (open question).

## Q1. Which registrar?

### Comparison (prices checked 2026-11-15)

| Registrar | .com first yr / renewal | .in | WHOIS privacy | DNSSEC | Nameserver freedom | 2FA | Email forwarding | Notes |
|---|---|---|---|---|---|---|---|---|
| **Cloudflare** | ~$10.46 / ~$10.46 at cost ([third-party tracker](https://cfdomainpricing.com/); at-cost claim [official](https://www.cloudflare.com/products/registrar/)) | Not supported ([TLD list](https://www.cloudflare.com/tld-policies/)) | Free redaction ([product page](https://www.cloudflare.com/products/registrar/)) | Free, one click ([product page](https://www.cloudflare.com/products/registrar/)) | **No.** Cloudflare nameservers are mandatory ([FAQ](https://developers.cloudflare.com/registrar/faq/)) | TOTP, WebAuthn keys ([docs](https://developers.cloudflare.com/fundamentals/user-profiles/2fa/)) | Free Email Routing, receive and forward only ([docs](https://developers.cloudflare.com/email-routing/)) | Best price and security, but DNS is tied to the registrar |
| **Porkbun** | $11.81 / $11.81 ([pricing](https://porkbun.com/products/domains)) | **$7.83 / $7.83** ([pricing](https://porkbun.com/products/domains), [.in page](https://porkbun.com/tld/in)) | Free on most TLDs. **The Porkbun pages contradict each other on .in** (see the note under this table) | Not confirmed on the pages read | Yes | TOTP, WebAuthn ([KB 19](https://kb.porkbun.com/article/19-how-to-enable-two-factor-authentication), [KB 119](https://kb.porkbun.com/article/119-how-to-secure-your-account-with-a-physical-security-key-using-webauthn)) | Up to 20 forwards ([pricing](https://porkbun.com/products/domains)) | Best choice if you want .in, or freedom to point nameservers anywhere. Watch new gTLD promos: .studio is $11.84, then $41.71 on renewal |
| **Vercel** | $12.03 / $12.03 ([API](https://api.vercel.com/v1/registrar/tlds/com/price)) | Not supported ([API](https://api.vercel.com/v1/registrar/tlds/in/price)) | Not stated in the docs read | Not stated | Not stated | Via the Vercel account | No | **Lapse risk:** if payment fails, "Vercel may not register the domain or may allow the domain registration to expire" ([terms §18.1](https://vercel.com/legal/terms)). Promo trap on gTLDs: .studio is $21.99, then $45 on renewal |
| **AWS Route 53** | Couldn't verify: the price PDF ([link](https://d32ze2gidvkk54.cloudfront.net/Amazon_Route_53_Domain_Registration_Pricing_20140731.pdf)) uses an unreadable font encoding, and live prices are only in the console or `GetDomainPrices` | Supported ([TLD list](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar-tld-list.html)) | Not checked | Not checked | Yes | AWS IAM MFA | No | Adds $0.50 per month per hosted zone for DNS ([pricing](https://aws.amazon.com/route53/pricing/)). Too heavy for a non-technical Firm |
| **Squarespace Domains** | Not shown; the price only appears in a JS search ([domains.squarespace.com](https://domains.squarespace.com/)) | Not mentioned | Free on eligible domains | Included | Yes | Not checked | Yes | "The price you see is the price you pay" ([same page](https://domains.squarespace.com/)), but I couldn't verify the numbers |
| **Namecheap** | **Couldn't verify** (HTTP 403 to automated fetches) | Couldn't verify | n/a | n/a | Yes | n/a | n/a | Check by hand |
| **GoDaddy** | **Couldn't verify** (HTTP 403) | Couldn't verify | n/a | n/a | Yes | n/a | n/a | Known for low first-year promos with higher renewals. Unverified here, so read the renewal price before paying |

**Notes**
- **.in rules:** .in is run by NIXI, not ICANN's gTLD policy. Porkbun warns that NIXI "can suspend or delete your domain without warning" if the contact details can't be verified ([porkbun.com/tld/in](https://porkbun.com/tld/in)), so use the proprietor's real name and address. Porkbun's `.in` page says WHOIS privacy is included, but its [WHOIS privacy page](https://porkbun.com/products/whois_privacy) lists `.in` as excluded. Treat it as **not private** unless checkout says otherwise. NIXI publishes accredited registrars (the list is linked from the NIXI WHOIS response: `rdap.nixiregistry.in/registrars/`). Indian registrars on that list are an option if INR billing or a GST invoice matters. I didn't compare them.
- **Watch for promo traps on vanity TLDs** like .studio, .design and .archi: Porkbun sells .archi at $13.39 for the first year, then $92.17 ([pricing](https://porkbun.com/products/domains)). `.com` has no promo gap at Cloudflare, Porkbun or Vercel.

### Why Cloudflare beats Porkbun for this Firm
Cloudflare has the lowest .com renewal price and hardware-key 2FA, and gives DNS, email forwarding and possibly hosting in one account the proprietor owns. Its one real cost is that you can't change nameservers. Moving DNS later means a registrar transfer, which is blocked for the first 60 days. Pick **Porkbun** instead if you want .in as the main domain, or want DNS somewhere other than Cloudflare. Porkbun's DNS is also "Cloudflare-powered" ([pricing](https://porkbun.com/products/domains)).

## Q2. Domain, DNS and hosting from the same provider?

**Recommendation: keep the registrar (or registrar plus DNS) separate from the host.** Only combine them when the host is Cloudflare, and then because it's free and the Firm owns the account.

- **Suspension blast radius:** Vercel can "shut down and terminate projects or deployments using the Hobby plan without notice for any reason" ([terms §4](https://vercel.com/legal/terms)). After termination, content "may be permanently deleted" (§19.1). For domains registered there, an unpaid account means the registration may be allowed to expire (§18.1). If the domain is held elsewhere, a host problem costs you only a DNS record change.
- **Transfer lock:** under the current ICANN Transfer Policy, a registrar may refuse a transfer "within 60 days of the creation date" (§I.A.3.7.5) or within 60 days of a previous transfer (§I.A.3.7.6). A change of registrant triggers a mandatory 60-day lock unless you opt out (§II.C.2). ([ICANN Transfer Policy](https://www.icann.org/resources/pages/transfer-policy-2016-06-01-en)) A wrong registrar choice made today stays in place until mid-January 2027. The ICANN Board adopted a revised policy on 7 June 2026 that shortens both windows to 30 days, but I found **no effective date** ([Board resolutions](https://www.icann.org/en/board-activities-and-meetings/materials/approved-resolutions-regular-meeting-of-the-icann-board-07-06-2026-en)). .in follows NIXI's own 60-day post-creation rule, reported by registrars ([Openprovider](https://support.openprovider.eu/hc/en-us/articles/11313763536274--com-in)). I couldn't verify it from NIXI directly.
- **Auth code:** registrars must hand over the transfer code within 5 calendar days (§I.A.5.2, same ICANN page). Lock-in is friction, not a trap.
- **Convenience:** a single bill and automatic DNS. That's not worth the risk unless the provider is also the cheapest and most secure option, as Cloudflare is.

## Q3. DNS needed for email, WhatsApp and gateways, and who controls it

**Who controls DNS:** the Firm's Cloudflare account, owned by the proprietor, with 2FA on a hardware key or TOTP and a recovery code stored offline. The repo owner gets member access to edit records. Record this on the ticket.

**Records needed at launch:**
- **Site:** an apex (or `www`) record pointing at the host. The host gives you the exact value. Vercel, for example, shows it under Project → Domains ([Vercel domains](https://vercel.com/docs/domains)).
- **Inbound mail for the Firm (e.g. `hello@`):**
  - Free option: Cloudflare Email Routing forwards to an existing Gmail address. It handles incoming mail only ([docs](https://developers.cloudflare.com/email-routing/)).
  - Real mailbox: Google Workspace Business Starter at ₹270 per user per month on an annual plan ([pricing](https://workspace.google.com/intl/en_in/pricing.html)), or Zoho Mail. Zoho has a Forever Free plan with up to 5 users and one domain, but it's "available only in select data centers", and I couldn't confirm India ([pricing](https://www.zoho.com/mail/zohomail-pricing.html)).
- **Sending authentication:** Gmail requires **SPF or DKIM** for every sender, and **SPF, DKIM and DMARC** with From-domain alignment above 5,000 messages a day ([Google sender guidelines](https://support.google.com/a/answer/81126)). For Workspace, add the SPF TXT record `v=spf1 include:_spf.google.com ~all` ([Workspace SPF](https://knowledge.workspace.google.com/admin/security/set-up-spf)). Use **one SPF record** that combines every sender's `include:`, and the DKIM keys each sender provides. Add a starter DMARC record, `v=DMARC1; p=none; rua=mailto:…`, then tighten it later. Even though v1 is below the bulk threshold, add it anyway because it's free.
- **webinar.gg emails:** these go out under webinar.gg's identity unless webinar.gg supports a custom sending domain. **I couldn't verify** that: the webinar.gg site returned no readable feature text. Add it to the demo questions ([#21](https://github.com/sriharilakshmanan/one-dream-architecture/issues/21)). If it's supported, webinar.gg will provide the DKIM/SPF records to add.
- **Meta / WhatsApp (not in v1):** Meta domain verification works through a DNS TXT record, an HTML file or a meta tag ([Meta help](https://www.facebook.com/business/help/286768115176155), [dev docs](https://developers.facebook.com/docs/sharing/domain-verification/)). The Meta business-verification docs I could read say nothing about website requirements ([dev docs](https://developers.facebook.com/docs/development/release/business-verification/)). The help article needs JavaScript, so **this is unverified**. With DNS at Cloudflare, adding the TXT record later is quick.
- **Payment gateway "live site":** not needed in v1, because webinar.gg is the merchant ([#13](https://github.com/sriharilakshmanan/one-dream-architecture/issues/13)). The earlier research found that Razorpay wants live policy pages ([#2](https://github.com/sriharilakshmanan/one-dream-architecture/issues/2)). That only matters if the Firm opens its own gateway later.

## Q4. How much does the stack decide hosting?

**"Next.js means Vercel" is the easiest path, but it isn't automatic, and it costs $20/mo for this Firm.**

- **Vercel**
  - Hobby is free, but limited to "personal or non-commercial use" ([terms §4](https://vercel.com/legal/terms)). The terms don't define commercial. A business site that promotes paid Webinars should be treated as commercial.
  - Pro costs **$20/mo plus $20 per extra developer seat**, includes $20 of usage credit, and has free viewer seats ([pricing](https://vercel.com/pricing), [Hobby vs Pro](https://vercel.com/docs/plans/hobby)).
  - Hobby caps include 100 GB transfer and 5K image transformations a month ([Hobby docs](https://vercel.com/docs/plans/hobby)). That's plenty for traffic, but the terms rule it out regardless.
  - Paid Pro includes a free first-year domain ([Hobby docs](https://vercel.com/docs/plans/hobby)). Don't take it, per Q2.
- **Netlify**
  - Free plan with 300 credits; Pro is $20/mo with 3,000 credits ([pricing](https://www.netlify.com/pricing/)). Free-plan commercial terms are **unverified**.
  - Next.js 13.5+ runs through Netlify's OpenNext adapter. It fully supports ISR, on-demand revalidation, image optimization (Netlify Image CDN), middleware and draft mode, with minor ordering caveats ([Netlify Next.js docs](https://docs.netlify.com/frameworks/next-js/overview/)).
- **Cloudflare Workers/Pages**
  - Free plan: 100k Worker requests a day, and static asset requests are "free and unlimited". Paid is $5/mo ([pricing](https://developers.cloudflare.com/workers/platform/pricing/)).
  - Next.js runs through `@opennextjs/cloudflare`, which supports Next 15 (latest minor) and 16 with ISR, middleware/proxy, image optimization (needs Cloudflare Images) and PPR. Edge runtime isn't supported, and Workers are capped at **3 MiB gzipped on Free, 10 MiB on Paid** ([opennext.js.org/cloudflare](https://opennext.js.org/cloudflare)).
  - Cloudflare now points to `vinext` (beta) as the default ([CF Next.js guide](https://developers.cloudflare.com/workers/frameworks/framework-guides/nextjs/)).
  - Pages Free gives 500 builds a month ([limits](https://developers.cloudflare.com/pages/platform/limits/)). Free-plan commercial terms are unverified, though I found nothing in the docs I read that restricts commercial use.
- **AWS Amplify:** 12-month free tier, then $0.15/GB transfer and $0.30 per million SSR requests ([pricing](https://aws.amazon.com/amplify/pricing/)). Its Next.js integration isn't Next.js-verified ([Next.js deploying](https://nextjs.org/docs/app/getting-started/deploying)). It's overkill here.
- **Self-hosting:** a Node server or Docker supports "All" features ([Next.js deploying](https://nextjs.org/docs/app/getting-started/deploying)). Image optimization and Proxy work with no configuration under `next start`. ISR works out of the box on a single instance with persistent disk. Multiple instances need a shared cache handler, plus a shared encryption key and `deploymentId` ([self-hosting guide](https://nextjs.org/docs/app/guides/self-hosting)). That means running a server and patching it, which is wrong for a firm with no ops person.
- **Verified adapters (the Deployment Adapter API):** only Vercel and Bun are Next.js-verified today. Cloudflare and Netlify "are working on" verified adapters ([Next.js deploying](https://nextjs.org/docs/app/getting-started/deploying)). This is the honest measure of how far Vercel is ahead.
- **Static export runs anywhere** (S3, Nginx, Pages, GitHub Pages). It drops ISR, default image optimization, redirects and headers, Proxy, **Draft Mode**, Server Actions, and route handlers that rely on the request ([static exports](https://nextjs.org/docs/app/guides/static-exports)).
  - **What this means for Prismic:** Prismic's Next.js setup uses `/api/preview` and `/api/revalidate` route handlers driven by a Prismic webhook ([Prismic Next.js docs](https://prismic.io/docs/nextjs)). With a static export you lose full-site previews for staff and switch to rebuilding on publish through a deploy hook.

**Decision rule:** if the Firm's editors need to preview a draft page before publishing, run Next.js on a server runtime: Vercel Pro, or Netlify/Cloudflare to save the $20. If publishing and waiting a minute for a rebuild is fine, use a static export on Cloudflare at $0 next to the DNS.

## Open questions for the user

1. **Which name and TLD did the Firm pick?** I assumed `onedreamarchitecture.com`, which is available along with `.in` (2026-11-15). `onedreamarchitects.com` is taken (Hostinger, since 2022).
2. Buy `.in` too, as a redirect (about $7.83/yr at Porkbun)?
3. Will the proprietor create and own the Cloudflare account (yes is recommended), or is the repo owner holding it in trust?
4. Must staff preview drafts before publishing? This decides server runtime vs static export (Q4) and is an input to the stack/CMS decision.
5. Is $20/mo for Vercel Pro acceptable, or is the target $0 hosting?
6. Do you want a real mailbox on the domain (Workspace ₹270 per user per month vs Zoho), or just forwarding?
7. For the demo ([#21](https://github.com/sriharilakshmanan/one-dream-architecture/issues/21)): can webinar.gg send email from the Firm's domain (custom DKIM/SPF), and can the registration page sit on a subdomain (e.g. `webinars.<domain>`)?

## Unverified or blocked

- I couldn't read Namecheap or GoDaddy pricing (HTTP 403). Squarespace prices only load through JavaScript. The Route 53 price PDF couldn't be parsed.
- Cloudflare's .com price comes from a third-party tracker. Cloudflare only shows prices at checkout.
- Porkbun .in WHOIS privacy (its own pages contradict each other).
- Meta business-verification website requirements (the help page needs JavaScript).
- Netlify and Cloudflare free-plan commercial-use terms (I didn't read their ToS).
- NIXI's .in transfer rules come from registrars' docs, not NIXI itself.
- The effective date of ICANN's 30-day transfer-lock revision.

## Sources

All accessed 2026-11-15.

- Cloudflare: [Registrar product](https://www.cloudflare.com/products/registrar/), [Registrar FAQ](https://developers.cloudflare.com/registrar/faq/), [TLD policies](https://www.cloudflare.com/tld-policies/), [2FA](https://developers.cloudflare.com/fundamentals/user-profiles/2fa/), [Email Routing](https://developers.cloudflare.com/email-routing/), [Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/), [Pages limits](https://developers.cloudflare.com/pages/platform/limits/), [Pages deploy hooks](https://developers.cloudflare.com/pages/configuration/deploy-hooks/), [Next.js on Workers](https://developers.cloudflare.com/workers/frameworks/framework-guides/nextjs/), [OpenNext Cloudflare](https://opennext.js.org/cloudflare). Third party: [cfdomainpricing.com](https://cfdomainpricing.com/)
- Porkbun: [domain pricing](https://porkbun.com/products/domains), [.in](https://porkbun.com/tld/in), [WHOIS privacy](https://porkbun.com/products/whois_privacy), [KB 2FA](https://kb.porkbun.com/article/19-how-to-enable-two-factor-authentication), [KB WebAuthn](https://kb.porkbun.com/article/119-how-to-secure-your-account-with-a-physical-security-key-using-webauthn)
- Vercel: [terms](https://vercel.com/legal/terms), [pricing](https://vercel.com/pricing), [Hobby plan](https://vercel.com/docs/plans/hobby), [fair use](https://vercel.com/docs/limits/fair-use-guidelines), [domains](https://vercel.com/docs/domains), [registrar API: TLDs](https://api.vercel.com/v1/registrar/tlds/supported), [.com price](https://api.vercel.com/v1/registrar/tlds/com/price), [.in](https://api.vercel.com/v1/registrar/tlds/in/price)
- AWS: [Route 53 pricing](https://aws.amazon.com/route53/pricing/), [Route 53 TLDs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar-tld-list.html), [Amplify pricing](https://aws.amazon.com/amplify/pricing/)
- Squarespace: [domains](https://domains.squarespace.com/)
- ICANN: [Transfer Policy](https://www.icann.org/resources/pages/transfer-policy-2016-06-01-en), [Board resolutions 2026-06-07](https://www.icann.org/en/board-activities-and-meetings/materials/approved-resolutions-regular-meeting-of-the-icann-board-07-06-2026-en)
- .in: NIXI WHOIS (`whois.nixiregistry.in`), [Openprovider .in notes](https://support.openprovider.eu/hc/en-us/articles/11313763536274--com-in)
- Next.js: [deploying](https://nextjs.org/docs/app/getting-started/deploying), [self-hosting](https://nextjs.org/docs/app/guides/self-hosting), [static exports](https://nextjs.org/docs/app/guides/static-exports)
- Netlify: [pricing](https://www.netlify.com/pricing/), [Next.js on Netlify](https://docs.netlify.com/frameworks/next-js/overview/)
- Prismic: [Next.js guide](https://prismic.io/docs/nextjs)
- Google: [sender guidelines](https://support.google.com/a/answer/81126), [Workspace SPF](https://knowledge.workspace.google.com/admin/security/set-up-spf), [Workspace India pricing](https://workspace.google.com/intl/en_in/pricing.html)
- Zoho: [Mail pricing](https://www.zoho.com/mail/zohomail-pricing.html)
- Meta: [domain verification (dev)](https://developers.facebook.com/docs/sharing/domain-verification/), [domain verification (help)](https://www.facebook.com/business/help/286768115176155), [business verification (dev)](https://developers.facebook.com/docs/development/release/business-verification/)

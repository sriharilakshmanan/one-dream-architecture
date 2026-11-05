# WhatsApp Business messaging for a sole proprietor

Ticket: #6 (map: #1). Researched 5 November 2026 against Meta's WhatsApp Business Platform docs, Meta's official rate cards and providers' own pricing pages.

## Question

What does it take for a sole proprietorship in India, with GST registration still pending, to send transactional WhatsApp messages (booking confirmation, Webinar invite, reminders) to Registrants in India and abroad? Which provider should the Firm use, and can a working setup be ready by 16 November 2026?

## Answer

**Use Meta's WhatsApp Cloud API directly.** Meta charges no platform fee and no markup, and webhooks are built in. The Firm's server already handles payment success, so it can call one HTTPS endpoint per message.

**Launching on 16 November is realistic, as long as the Firm does not wait for business verification.** A business portfolio that Meta has not verified can already:

- register 2 phone numbers;
- send templates to 250 unique users in any rolling 24 hours;
- hold 250 templates.

That is enough for a launch-size Webinar. Templates are reviewed automatically, and Meta says review can take up to 24 hours. Business verification without GST is the slow, uncertain part. Run it in parallel and treat it as a post-launch upgrade.

**Costs are paise per Registrant.** Four utility messages (confirmation, invite, 2 reminders) cost about:

- India: **₹0.46**
- North America: **US$0.014**
- UK: **US$0.088**
- UAE: **US$0.063**

## Requirements and limits

### Accounts and onboarding (Meta Cloud API, direct)

- **Accounts needed:** a Meta developer account, a Meta business portfolio, a WhatsApp Business Account (Meta's docs call it a "Messaging account"), an app, and a system-user permanent token. Webhooks deliver message-status notifications. [Cloud API get started](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started)
- **Phone number:** it must be owned by the Firm, have a country and area code, and be able to receive SMS or voice calls. "Numbers already in use with WhatsApp cannot be registered unless they are deleted first." A new portfolio is "initially capped at two registered business phone numbers". The cap rises to 20 after verification, or once the portfolio reaches the 2,000 messaging limit. [Phone numbers](https://developers.facebook.com/docs/whatsapp/cloud-api/phone-numbers)
  - **Keeping the WhatsApp Business app on the same number ("Coexistence"):** this is possible, but only through a Solution Partner or Tech Provider's Embedded Signup, and it has limits, such as a fixed 20 messages per second and no catalogs. The docs do not say whether it is available in India. Simplest path: **get a fresh number (a new SIM) for the Firm.** [Coexistence](https://developers.facebook.com/docs/whatsapp/embedded-signup/custom-flows/onboarding-business-app-users)
- **Messaging limit without verification:** "Newly created business portfolios have a messaging limit of 250." This counts unique users messaged outside customer service windows in a rolling 24 hours. There are three ways to raise it to 2,000:
  1. verify the business;
  2. have a partner verify it;
  3. send 2,000 delivered messages to unique users within 30 days, using high-quality templates.

  After 2,000 the limit scales automatically. [Messaging limits](https://developers.facebook.com/docs/whatsapp/messaging-limits)
- **Template cap:** an unverified portfolio can hold 250 templates per Messaging account. [Business accounts](https://developers.facebook.com/docs/whatsapp/overview/business-accounts)

### Business verification without GST (UNVERIFIED)

- **What Meta's docs say:** Meta's developer docs say only that verification is done in Business Manager by an Admin. They point to the Help Center for the list of accepted documents. [Business verification](https://developers.facebook.com/docs/development/release/business-verification)
- **What could not be checked:** the Help Center article ([Official documents to upload](https://www.facebook.com/business/help/159334372093366)) loads its content with JavaScript, so its India list could not be read.
- **Secondary sources only:** third-party guides (not primary) consistently name **Udyam (MSME) registration** and the **Shop & Establishment licence** as GST alternatives for sole proprietors. Getting a Udyam certificate is free and online. The Firm should confirm this inside Business Manager's verification flow.
- **How long it takes:** Interakt's FAQ says verification "usually takes between 3 business days to 4 weeks". This is a provider's statement, not Meta's. [Interakt FAQ](https://www.interakt.shop/faqs/)
- **Why it does not block launch:** verification is **one of three** ways to raise the messaging limit, not a precondition for sending.
- **Display name:** the docs do not say how an unverified number's name appears to recipients before display-name review. The review "runs automatically once you reach a higher messaging limit". Expect Registrants to possibly see the phone number rather than "One Dream Architecture" at first. [Display names](https://developers.facebook.com/docs/whatsapp/display-names)

### Templates and categories

- **When templates are needed:** the Firm's messages go out first, without the Registrant writing in, so every one must be an approved **template**.
- **Review time:** "Templates are automatically reviewed upon creation or after editing," and review "can take up to 24 hours." [Template guidelines](https://developers.facebook.com/docs/whatsapp/message-templates/guidelines)
- **Utility vs marketing:** **Utility** templates must be "non-promotional" and specific to, or requested by, the user. Order and transaction confirmations are named as utility. Mixed or unclear content defaults to marketing. Since 9 April 2025, a template submitted as UTILITY that doesn't qualify is approved as MARKETING instead of being rejected. Approved templates can also be recategorized later, with 1 day's notice. [Template categorization](https://developers.facebook.com/docs/whatsapp/updates-to-pricing/new-template-guidelines)
  - The docs don't name event reminders explicitly. A reminder about the Registrant's own booking should qualify as utility. Any "invite a friend" or upsell wording makes it marketing, which costs roughly 7× more in India.
- **Receipt as a PDF header:** a template's header can be a `DOCUMENT` (PDF). The sample PDF is uploaded through the Resumable Upload API when the template is created. [Template components](https://developers.facebook.com/docs/whatsapp/business-management-api/message-templates/components)

### Sending a document (receipt)

- **Formats:** the Cloud API document message takes a media `id` or a public `link`, plus an optional `filename` and a caption of up to 1,024 characters. PDF is supported, up to 100 MB. [Document messages](https://developers.facebook.com/docs/whatsapp/cloud-api/messages/document-messages)
- **How to send it:** a free-form document only works inside a customer service window, which opens when the Registrant messages the Firm. Otherwise, use the booking-confirmation **utility template with a DOCUMENT header** and attach the receipt PDF at send time.
- **What the docs don't show:** the exact send-time syntax for a document header (link or id) was not visible in the pages fetched. The general template-send docs should cover it. Check with a test send.
- **GST caveat:** until GST registration is done, the PDF is a payment receipt, not a tax invoice. See the GST research ticket.

### Opt-in

- **Rule:** the Firm needs the Registrant's number and their opt-in permission. The opt-in "must clearly state that a person is opting in to receive communication from the business" and must name the business. A **website form is an accepted method.** [Getting opt-in](https://developers.facebook.com/docs/whatsapp/overview/getting-opt-in)
- **What to build:**
  - a checkbox on the registration or checkout form, for example "Send my booking confirmation and Webinar reminders from One Dream Architecture on WhatsApp";
  - a phone field with a country code (E.164 format);
  - a stored consent timestamp.
- **Fallback:** a Registrant who doesn't opt in, or whose number fails, gets email. Only one channel is used per Registrant.

### Pricing (Meta, effective 1 October 2026)

- **How Meta charges:**
  - Meta charges **per delivered message**.
  - The rate depends on the **recipient's country calling code**, not on where the Firm is.
  - Since **1 October 2026**, utility messages sent inside the customer service window are charged too. They had been free since July 2025.
  - Each number gets 1,000 free *service* (non-template) messages a month.

  [Pricing](https://developers.facebook.com/docs/whatsapp/pricing)
- **Billing currency:** Indian "Sold-To" businesses must move their Messaging accounts to **INR** billing by 31 December 2026. After that, non-INR accounts of eligible customers stop delivering. Choose INR at setup, because currency can't be changed later except through the migration API. (Meta's page doesn't say whether GST registration affects INR billing or invoices. This is unverified.)

Rates per message, from Meta's official rate-card files ("Cost per message … effective October 1, 2026"), linked from the pricing page:

| Market | Utility (INR) | Utility (USD) | Marketing (USD) | 4 utility msgs / Registrant (USD) |
|---|---|---|---|---|
| India | ₹0.115 | $0.0014 | $0.0118 | $0.0056 (₹0.46) |
| North America | ₹0.2491 | $0.0034 | $0.025 | $0.0136 |
| United Kingdom | ₹1.6122 | $0.022 | $0.0635 | $0.088 |
| United Arab Emirates | ₹1.1509 | $0.0157 | $0.0576 | $0.063 |
| Singapore | ₹1.1727 | $0.016 | $0.0732 | $0.064 |
| Germany | ₹4.0322 | $0.055 | $0.1365 | $0.22 |
| Rest of Western Europe | ₹1.2516 | $0.0171 | $0.0592 | $0.068 |
| Rest of Asia Pacific | ₹0.8282 | $0.0113 | $0.0842 | $0.045 |
| Rest of Middle East | ₹0.6665 | $0.0091 | $0.0392 | $0.036 |
| Other | ₹0.5641 | $0.0077 | $0.0604 | $0.031 |

### Provider comparison

| Option | Fee over Meta | Monthly | API / webhooks | Notes |
|---|---|---|---|---|
| **Meta Cloud API (direct)** | none | none | native REST and webhooks | Cheapest. You build the integration (a few HTTPS calls). |
| Interakt | India utility ₹0.160 / ₹0.150 / ₹0.140 (Starter / Growth / Advanced) vs Meta ₹0.115 | Starter n/a; Growth ₹2,799; Advanced ₹3,799 (+tax) | APIs and webhooks only on Growth and above, not Starter | [Pricing](https://www.interakt.shop/pricing/). International rates are on a separate page and were not checked. |
| AiSensy | India utility ₹0.145 | "Free Forever plan" exists. Plan prices weren't in the page text. | "Developer APIs" and "Webhook APIs" listed, but the plan isn't stated | [Pricing](https://aisensy.com/pricing). Its Indian virtual-number add-on "Requires GST/MSME for KYC". |
| Gupshup (self-serve) | US$0.001 per message | none stated | yes | [Pricing](https://www.gupshup.ai/channels/self-serve/whatsapp/pricing). Country rates not shown in the page text. |
| Twilio | US$0.005 per message (in or out), plus $0.001 per failed message | none | yes | [Pricing](https://www.twilio.com/en-us/whatsapp/pricing). That fee is about 3.5× Meta's India utility rate. |

Why the recommendation is direct Cloud API:

- The Firm only needs about 4 server-triggered templates per Registrant. It doesn't need a shared inbox, chatbots or broadcast tools, which is what the ₹2,800+/month plans pay for.
- Interakt Starter has no API at all.
- Twilio's fee is several times Meta's India rate.

If the Firm later wants a no-code inbox for replies, AiSensy or Interakt can be added. Gupshup is the cheapest API-first provider if Meta's direct onboarding hits a snag.

## Realistic plan to 16 November

1. **Day 1:** set up all accounts:
   - business portfolio, app and WhatsApp Business Account, billed in INR, with a payment card;
   - a new SIM, registered as the business number;
   - the system-user token and webhook endpoint.
2. **Day 1:** submit 4 utility templates in English. Write them plainly and with no promotion:
   - `booking_confirmation`, with a DOCUMENT header for the receipt;
   - `webinar_invite`, with the join link;
   - `reminder_24h`;
   - `reminder_1h`.
3. **Day 1:** start business verification in parallel, with Udyam or a Shop & Establishment licence plus PAN. Don't depend on it.
4. **Days 2–4:** build these on the server:
   - send-on-payment-webhook;
   - scheduled reminders;
   - email fallback on failure or no opt-in;
   - a status webhook that logs delivered or failed.
5. **Days 5–10:** send test messages to Indian and foreign numbers, and confirm the document header and category billing.

**Risks:**

- A template is recategorized as marketing, which raises cost but doesn't block sending.
- Display-name presentation before verification is unknown.
- Launch demand exceeds 250 *unique* Registrants in a 24-hour window. Mitigation: spread the reminder sends, or fall back to email for the overflow.

## Not verified

- Meta's official list of accepted India verification documents for sole proprietors without GST. The Help Center page needs JavaScript. All claims about Udyam and Shop & Establishment come from secondary sources.
- How long Meta's verification takes. The only figure, 3 days to 4 weeks, is Interakt's.
- How an unverified number's display name appears to recipients.
- Whether Coexistence is available in India.
- The send-time payload for a DOCUMENT template header.
- Whether GST status affects Meta's INR billing or invoices.
- International per-message rates for Interakt, AiSensy and Gupshup, and AiSensy's plan prices.
- Twilio's pricing page still says utility messages inside the service window cost nothing. That contradicts Meta's change on 1 October 2026, so its page may be stale.

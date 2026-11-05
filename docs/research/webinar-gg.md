# What webinar.gg can do

Ticket: #5. Researched 5 November 2026 against webinar.gg's own site, developer docs, public plans endpoint and the English strings shipped in its web app.

## Short answer

webinar.gg can run **the whole booking flow itself**: a public registration page per Webinar with a cover image, description and social links; paid tickets in INR or USD through a **Razorpay checkout it runs**; a "Ticket confirmed" email that works as the receipt; an invite with a calendar file; and **email reminders** (24 h, 1 h and at start by default, all editable). After each session it sends the host an attendee report listing who joined and who did not.

What it **cannot** do: **WhatsApp or SMS** to Registrants (email only), and a receipt or tax invoice issued by the Firm. webinar.gg collects the money as the merchant, adds a 2.5% processing fee for the buyer, and pays the Firm out to its bank account about 5 days later. For the Registrant, the seller on the checkout is webinar.gg, not the Firm.

So there are two possible designs:

- **A. webinar.gg owns booking.** Our calendar links each Webinar to its webinar.gg registration page. webinar.gg handles payment, the receipt email, the invite, the email reminders and attendance. We build only the calendar and Webinar pages, plus a webhook listener if we want WhatsApp.
- **B. We own booking.** We take payment on our own gateway, show our own thank-you page and receipt, and send WhatsApp ourselves. After payment we call the webinar.gg API to register the person (`POST /webinar/:id/invite`, which sends webinar.gg's invite email and reminders) or mint a join token at join time. Attendance comes from the attendee report email and `GET /webinar/:id/metrics`.

The "one channel only" rule settles it. If we want WhatsApp, webinar.gg's own invite and reminder emails would make a second channel. Choose A with email as the only channel, or B with our own WhatsApp messages and no `invite` call (join tokens only). Whether join tokens alone get reminders is **unverified**.

## Which product this is

webinar.gg is a browser-based live webinar platform (the "unlimited attendees, flat price" product sold on AppSumo and Product Hunt). Its API is at `https://webinar-api.webinar.gg/api/v1`. It lists Razorpay plan ids, charges in INR or USD and shows Asia/Kolkata in its examples, so it is clearly built for the Indian market. The site is a single-page app. Its docs come as Markdown at <https://webinar.gg/llms-full.txt>, which is the main source below.

## Findings by question

### Registration pages and forms

- Each Webinar can have a **public registration page** (an on/off toggle). The host can switch off "accept registrations" to show "Registration Closed". Options include guest registration (name and email only, with a magic link to sign in later), host approval with a waitlist, and capacity. [Source: app strings `webinar` locale, "Registration page" and "Tickets & registration" sections; blog: [How to Sell Paid Webinars](https://webinar.gg/blog/how-to-sell-paid-webinars), published 2026-10-19, updated 2026-10-27, fetched via `webinar-api.webinar.gg/api/blog/posts/how-to-sell-paid-webinars`]
- **Custom join-form fields** come in 11 types (text, number, email, phone, date, time, textarea, checkbox, radio, select, multi-select) and can be managed through the API. [Source: [docs/api-invites-registration](https://webinar.gg/docs/api-invites-registration)]
- The page shows a countdown, cover image, host name and event details. The buyer's path is: Sign in to Register → email OTP → mobile number → pay. [Source: blog above]
- After registration the attendee can add the event to Google Calendar or download an `.ics` file. [Source: app strings `addToCalendar`]
- A **Redirect URL** (https only) can be set per Webinar. The strings don't say whether it applies after registration or after the session ends: **unverified**. [Source: app strings `redirectUrlLabel`]

### Taking payments

- **Paid registration is built in.** You set a ticket price in INR and/or USD. Checkout is **Razorpay**: the site preconnects to `checkout.razorpay.com`, and refund webhooks carry a `razorpayRefundId`. Buyers can pay by UPI QR, card, net banking, wallet or pay-later, and EMI is offered. [Source: blog above; [docs/webhooks](https://webinar.gg/docs/webhooks) `registration.refunded`; homepage HTML]
- **The buyer pays a 2.5% processing fee on top of the ticket price.** The host is paid on the ticket price only. [Source: app bundle `webinar-registration-pricing` (`const c=.025`); app string "A {{percent}} processing fee is charged to attendees at checkout on top of the ticket price. Your payout is based on the ticket price only."]
- **webinar.gg is the merchant and pays the host out.** Before turning on paid registration, the host must save payout details: account holder, bank, account number, IFSC/SWIFT, country and phone. These are then verified, and supporting documents can be requested. "Settlements usually take 5 days." [Source: app strings `dashboard` locale "Payout details"; blog above]
- Refunds (full or partial) exist and fire webhooks. A full refund cancels the registration. An order still unpaid after 3 days is marked failed. [Source: [docs/webhooks](https://webinar.gg/docs/webhooks); [docs/changelog](https://webinar.gg/docs/changelog) 2026-10-27]
- **Unverified:** the payout fee or commission beyond the buyer-paid 2.5% (the docs say the payout equals the ticket price), whether webinar.gg issues a GST invoice and in whose name, and how the Firm should book this income. The terms page is rendered client-side and its text was not readable. Ask webinar.gg support, and cross-check with the GST research ticket.

### Receipt and thank-you

- After payment, webinar.gg emails **"Ticket confirmed: {title}"**. It contains a "Ticket receipt" block with ticket price, processing fee, total paid, payment ID, order ID and a **View invoice** link, plus the line "Save this email as your receipt." [Source: app strings `email` locale `registrationTicket`]
- The thank-you screen is a toast ("Payment successful") on webinar.gg's own page, not a page on our site. [Source: app strings]

### Invites and reminders (email, WhatsApp, SMS)

- **Email only.** The invite email ("Invitation: {title}") includes a join link, a passcode if one is set, and an attached calendar invite. Times are shown in the Webinar's timezone. [Source: `email` locale `invitation`; [docs/api-webinars](https://webinar.gg/docs/api-webinars) `timezone`]
- **Email reminders** are set per Webinar. The defaults are 24 hours before, 1 hour before and at start. The host can add, edit or remove them, with optional custom subject and body, in units of minutes, hours or days before. A manual **follow-up email** can be sent after the session. [Source: `webinar` locale "Email reminders" (`defaultHint`, `followUpTitle`)]
- **No WhatsApp or SMS messages to attendees.** I searched every shipped English string and the app bundle. WhatsApp appears only (a) as a social-card fetcher for shared links, and (b) as a phone field on webinar.gg's own *demo-booking* form for sales leads. SMS does not appear. [Source: [docs/overview](https://webinar.gg/docs/overview) "Search and sharing"; app bundle]
- Invite API limits: at most 100 emails per call and 5,000 invitees per Webinar. **Inviting always sends an email.** [Source: [docs/api-invites-registration](https://webinar.gg/docs/api-invites-registration); [docs/quickstart](https://webinar.gg/docs/quickstart)]

### Custom branding

- Per Webinar: a **cover image** (square, 1:1), an "About this event" description, and social links (website, Instagram, X, LinkedIn, YouTube, TikTok) on the registration and join pages. The waiting screen can have a custom title, message and **logo**. There is also an optional watermark (viewer email on the video). [Source: `webinar` locale `Branding`, `waitingScreen`; [docs/api-webinars](https://webinar.gg/docs/api-webinars) `watermarkEnabled`]
- **No evidence of a custom domain or white-labelling.** Emails are signed "The webinar.gg team", and pages live on `webinar.gg`. [Source: `email` locale `layout.signoff`]
- The room can be **embedded in an iframe** on our own site. Embedding needs a join token. [Source: [docs/iframe-embed](https://webinar.gg/docs/iframe-embed)]

### API and webhooks to create Registrants from our site after payment

- REST API with a bearer token from **Settings → Integrations**. It is a **premium-plan** feature (otherwise `403`). Rate limit: 120 requests per minute per account. `POST`s are not idempotent. [Source: [docs/authentication](https://webinar.gg/docs/authentication); [docs/errors-and-rate-limits](https://webinar.gg/docs/errors-and-rate-limits)]
- There are two ways to add a person after our own payment:
  - `POST /webinar/:id/invite` with `{emails: [...]}` creates the user and sends webinar.gg's invite email. There is no way to suppress that email. [Source: [docs/api-invites-registration](https://webinar.gg/docs/api-invites-registration)]
  - `POST /webinar/join-token` with name, email, **phone (required)**, passcode and optional custom responses mints a one-time token. It is valid for 10 minutes and up to 10 uses, and **sends no email**. Our join button calls it at click time and redirects to `…/webinar-page/:id?token=…`. If the Webinar has `authentication: true`, the email must also be on the invite list. [Source: same page]
- **Webhooks** (one callback URL per account): `webinar.created/updated/started/ended/deleted`, `webinar.user.invited/uninvited`, `webinar.recording.created`, `registration.created`, `registration.status.changed`, `registration.payment.completed`, `registration.payment.failed`, `registration.refunded`. Payloads include registrant email and name, and payment amounts and ids. Deliveries are **unsigned** (the docs say to verify ids against the API), may arrive more than once, and may arrive out of order. [Source: [docs/webhooks](https://webinar.gg/docs/webhooks)]
- There is no documented endpoint to *list registrations* or payments. Only invites can be listed (`GET /webinar/:id/invite`). [Source: [llms-full.txt](https://webinar.gg/llms-full.txt) endpoint index]

### Unique join links

- Yes. Each invite email carries a "personalized join link". Join tokens give single-person links that skip the form and OTP. [Source: [docs/api-invites-registration](https://webinar.gg/docs/api-invites-registration)]
- The open webinar link also works with a public join form (email OTP) unless the Webinar requires invite-only `authentication` or a passcode. A paid Webinar blocks unpaid joins ("Payment is required before you can join"). [Source: same; `webinar` locale `errors.payment_required`]

### Attendance and analytics (who actually joined)

- After each Webinar the host gets an **Attendee report** email listing Attendees and Absentees, with name, email, joined at, left at and total minutes. [Source: `email` locale `attendeeReport`; [docs/webhooks](https://webinar.gg/docs/webhooks)]
- The dashboard can download the invited-users list, chat transcript and Q&A transcript, and shows join-form responses. [Source: `webinar` locale]
- API: `GET /webinar/:id/metrics` returns aggregates only (`totalUsers`, `maxLiveUsers`, duration and a participant time series), **not a per-person list**. There is no documented API for per-Attendee attendance. [Source: [docs/api-webinars](https://webinar.gg/docs/api-webinars)]
- Browser analytics (GA4, Meta Pixel, Clarity, GTM) fire on registration and join pages, including `InitiateCheckout` and `Purchase`. [Source: [docs/analytics-events](https://webinar.gg/docs/analytics-events)]

### Recordings

- Auto-recording is included. Visibility can be public, invited users only, or host only. A `webinar.recording.created` webhook carries MP4 or HLS URLs. Recordings can be reused as pre-recorded or "just-in-time" sessions. Live sessions can also be pushed to YouTube, Twitch or custom RTMP on premium. [Source: [docs/webhooks](https://webinar.gg/docs/webhooks); [docs/api-stream-destinations](https://webinar.gg/docs/api-stream-destinations); `webinar` locale; plans endpoint `sharedFeatures`]

### Plan limits and pricing

- There is one paid tier ("starter", sold monthly or annually). Live prices from `GET https://webinar-api.webinar.gg/api/payment/get-plans` on 2026-11-05:

  | Plan | List price | Auto-applied coupon today |
  | --- | --- | --- |
  | INR monthly | ₹799 | DIWALI → ₹99 |
  | INR annual | ₹5,999 | ANNUAL70 → ₹1,799 |
  | USD monthly | $9.99 | none |
  | USD annual | $99.99 | ANNUAL70OFF → $29.99 |

- Included features: unlimited participants, unlimited duration, polls and quizzes, screen annotation, auto-recording, scheduling, one-click reports and "all integrations". **Premium is required to start a Webinar at all**, and for the API. [Source: plans endpoint; [docs/changelog](https://webinar.gg/docs/changelog) "Starting a webinar needs a premium owner"]
- Discrepancy: the blog says "$4.99/month (₹499 in India)", but the live plans endpoint says ₹799 / $9.99. Trust the endpoint, and check the Firm's current plan in its billing page.
- Hard limits found: 5,000 invitees per Webinar, 100 per invite call, 120 API requests per minute, 3 stream destinations per Webinar, and a 2 GB maximum for uploaded MP4s.

## Which steps webinar.gg could own outright

| Step | webinar.gg? | Notes |
| --- | --- | --- |
| Calendar of Webinars on our site | No | We build it. webinar.gg has no public multi-event listing page that we could find. |
| Webinar detail page | Partly | Its registration page is branded per Webinar, but on webinar.gg's domain. |
| Payment (Indian gateway) | **Yes** | Razorpay, with webinar.gg as merchant, a 2.5% buyer fee and payout in about 5 days. |
| Thank-you page and receipt | **Yes** (email) | A "Ticket confirmed" receipt email with an invoice link. The thank-you is on webinar.gg's page, not ours. |
| Invite | **Yes** (email) | Includes a calendar file. |
| Reminders | **Yes** (email only) | No WhatsApp or SMS. |
| Join link | **Yes** | Personalised links, or join tokens. |
| Attendance | **Yes** (email report) | Per-person report by email only. The API gives aggregates. |
| Recording | **Yes** | |

## Not verified

- Tax treatment: who invoices the Registrant, whether GST is charged, and in whose name. The terms page text could not be read.
- Whether webinar.gg takes any fee from the host's payout beyond the buyer-paid 2.5%.
- The exact audience for reminder emails (invitees, paid registrants, or both), and whether people who only use join tokens get them.
- What the Redirect URL applies to.
- Whether our own domain or branding can replace webinar.gg's in emails.
- The Firm's current plan and price. The account was not inspected.

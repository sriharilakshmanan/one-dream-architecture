# International cards: cost and effort of enabling vs not

Ticket: #3 (map #1). Researched 5 Nov 2026 for a 16 Nov 2026 launch.

Setting: the Firm is a sole proprietorship in India. GST registration is in progress. Webinars are priced in INR. Some Registrants are abroad.

## Short answer

- **Foreign cards can pay an INR price** on Razorpay and PhonePe PG (the "non-DCC" model): the Registrant is charged INR, and their own bank converts it to their currency, usually adding its own foreign-transaction fee. Cashfree's international gateway (IPG) is presented as "customer pays in their currency, you get INR" (DCC). The Firm still receives INR from every gateway.
- **Enabling it is a dashboard request plus KYC**, not a separate licence. Razorpay's own posts say approval takes about 1–3 business days. No gateway publishes a guaranteed timeline (SLA).
- **Cost: roughly 1 percentage point more per international transaction** (about 3% instead of about 2%), plus 18% GST on the fee. There is no setup fee. PayPal costs much more (4.4% + USD 0.30, plus a 3–4% currency-conversion spread).
- **If international cards are off**, a foreign-issued card is declined at checkout. A foreign Registrant then needs an Indian card or UPI, or has no way to pay.
- **Paperwork** is light, because the gateway is the RBI-regulated cross-border Payment Aggregator (PA-CB). The gateway takes a purpose code at onboarding and issues inward-remittance proofs (FIRC/FIRS). The Firm needs to keep invoices and talk to its CA about how GST applies to export of services (the LUT question).

**Recommendation for launch:** ask for international cards on whichever gateway is chosen as part of onboarding, the same day the account is opened. It costs nothing to request and about 1% more only on foreign transactions. If it isn't approved by 16 Nov, launch domestic-only and show a "foreign card? email us" fallback (manual PayPal invoice or bank transfer) rather than delaying launch. Don't build a separate PayPal checkout for launch.

## Comparison by gateway

| Gateway | Foreign card charged in INR? | How to enable | Intl card fee (public) | Domestic card fee | Notes |
|---|---|---|---|---|---|
| Razorpay | Yes. INR is a supported currency and settlement is INR [R1][R2] | Dashboard → Account & Settings → International payments → Activate. Needs PAN, address proof, video KYC (Aadhaar + PAN), and GSTIN only "if GST registered" [R1] | Up to 3% + GST [R3] | 2% + GST [R3] | Freelancers/individuals are directed to PayPal; *registered businesses with a valid website* can accept foreign cards [R1]. NGOs are excluded [R2]. Activation is "typically 1–2 business days" [R5] or "2–3 business days" [R6] |
| Cashfree | Its IPG markets local-currency payment ("customers pay in their local currency; you receive INR") [C2][C3]. INR-priced foreign-card support isn't stated | Settings → Payment Methods → enable international cards / PayPal [C2] | 2.99% Visa/MC, 2.95% Amex (2.69% promo) [C1][C2] | 1.95% + GST [C1] | Settlement T+2. Issues a FIRS per transaction [C3]. Activation timeline isn't published |
| PhonePe PG | Yes, non-DCC: "charging the equivalent INR value, allowing the issuing bank to apply the exchange rate" [P1] | Not on by default. Request through the merchant dashboard or account manager [P2] | Not published. A PhonePe guide estimates 3–4.5% [P3] | 1.99% promo [P1] | Only **Mastercard** is listed for international cards [P1]; another page says Visa/MC/Amex [P3]. Collects purpose codes at onboarding [P1] |
| PayU | INR settlement [U1] | Through a Key Account Manager. Documents vary by line of business; every transaction needs an invoice [U1][U2] | 3% + GST [U3] | 2% + GST [U3] | Account-manager process makes it the slowest route on a 10-day horizon |
| Stripe India | n/a | **Invite-only for new Indian businesses** [S1] | n/a | n/a | Not a realistic launch option |
| PayPal (foreign payers only) | No, the payer pays in their currency | Separate PayPal business account | 4.40% + USD 0.30 fixed [PP1] | **Not available.** "For India users, we only support international payments" [PP1] | Plus a 3.0–4.0% conversion spread over the base rate [PP1]. Usable only alongside an Indian gateway |

### Example cost on a ₹2,000 webinar

Uses the fees above. The Firm pays 18% GST on the gateway fee. Whether that GST can be reclaimed as input credit depends on registration.

- Domestic card at 2%: ₹40 fee + ₹7.20 GST = **₹47.20**
- International card at 3%: ₹60 + ₹10.80 = **₹70.80** (about ₹24 more)
- PayPal at 4.4% + USD 0.30 (about ₹25), plus a 3–4% conversion on withdrawal: roughly **₹190–210**. This is an estimate because the exchange rate varies.

## Effort and paperwork

- **No separate RBI or FEMA licence for the Firm.** Since 31 Oct 2023, cross-border collection through gateways is done by RBI-regulated Payment Aggregator–Cross Border entities (PA-CB) [RBI1]. The consolidated Master Direction on Payment Aggregators (15 Sep 2025) caps each PA-CB transaction at ₹25 lakh [RBI2]. That cap doesn't matter for webinar prices.
- **Purpose code:** the gateway collects it at onboarding [P1][R5]. For webinars, pick the code for educational or professional services with the gateway or CA. *Not verified: which exact code fits.*
- **Proof of inward remittance:** Razorpay offers eFIRC/FIRS downloads from the dashboard [R4][R5]. Cashfree issues a FIRS per transaction [C3]. Keep these documents for the CA.
- **Invoices:** PayU requires one per transaction [U2]. Razorpay's settlement explainer says services exports "generally require a services Export Declaration Form within 30 days after the end of the invoice month" [R4]. *Not verified* whether this applies to card collections through a PA-CB or only to bank-transfer flows. Ask the gateway or CA.
- **GST:** GST registration isn't mandatory below the ₹20 lakh threshold, but filing a Letter of Undertaking (LUT, which lets export of services go out without charging IGST) needs a GSTIN [R5][R6]. So the in-progress GST registration matters for how foreign sales are taxed, not for whether the gateway lets the Firm accept them. *This is a tax question for the CA, not a gateway question.*

## What a foreign Registrant experiences

- **International cards on (non-DCC, Razorpay or PhonePe):** they see the INR price and pay with their card. Their bank converts the amount and usually adds a 1–3% foreign-transaction fee on the bank's side. 3DS or OTP authentication follows their bank's own flow. Fake email addresses or phone numbers cause international payments to fail on Razorpay [R1], so collect a real email and phone number at booking.
- **International cards off:** the card is declined at checkout. UPI only works with an Indian bank account, apart from NRIs using NRE/NRO accounts. Netbanking needs an Indian bank. *Not verified:* the exact error message each gateway shows.
- **PayPal only for foreigners:** they pay in their own currency. The Firm loses about 7–8% after fees and conversion. Only worthwhile as a manual fallback.

## Couldn't verify

- Guaranteed activation timelines for any gateway. The 1–3 day figures come from Razorpay blog posts, not an SLA.
- Whether Cashfree IPG accepts foreign cards against an INR-priced order (non-DCC) or forces DCC.
- PhonePe's public international card fee, and whether it supports Visa as well as Mastercard.
- Whether each gateway requires GSTIN or a current account specifically for **international** activation for a sole proprietor. Razorpay's docs say GSTIN only "if registered". A Razorpay blog recommends a current account over a savings account [R5].
- The exact purpose code for paid webinars, and whether an EDF or SOFTEX-type filing applies to small card receipts.
- PhonePe [P2][P3] and PayU [U1][U2][U3] claims come from search-result summaries of those first-party pages. I did not read the pages in full.

## Sources

- [R1] Razorpay docs, International Payments: https://razorpay.com/docs/payments/international-payments/
- [R2] Razorpay docs, International Payments FAQs: https://razorpay.com/docs/payments/international-payments/faqs/
- [R3] Razorpay pricing: https://razorpay.com/pricing/
- [R4] Razorpay blog, International payment settlement explained: https://razorpay.com/blog/international-payment-settlement-explained
- [R5] Razorpay blog, Payment links for international clients: https://razorpay.com/blog/accept-international-payments-payment-links/
- [R6] Razorpay blog, Shopify international payments: https://razorpay.com/blog/razorpay-shopify-international-payments/
- [C1] Cashfree pricing: https://www.cashfree.com/payment-gateway-charges/
- [C2] Cashfree international payment gateway: https://www.cashfree.com/international-payment-gateway/
- [C3] Cashfree docs, IPG overview: https://www.cashfree.com/docs/payments/international-payments/ipg/overview
- [P1] PhonePe Business, International payments: https://business.phonepe.com/international-payments
- [P2] PhonePe Business, List of international payment gateways: https://business.phonepe.com/articles/list-of-international-payment-gateways
- [P3] PhonePe guide, What are international card payments: https://www.phonepe.com/guides/payment-gateway/what-are-international-card-payments/
- [U1] PayU docs, Cross-border FAQs: https://docs.payu.in/docs/faqs-for-cross-border-payments
- [U2] PayU, International payments: https://payu.in/international-payments/
- [U3] PayU pricing: https://payu.in/pricing/
- [S1] Stripe support, Stripe accounts are invite-only in India: https://support.stripe.com/questions/stripe-accounts-are-invite-only-in-india
- [PP1] PayPal India merchant fees: https://www.paypal.com/in/webapps/mpp/merchant-fees
- [RBI1] RBI, Regulation of Payment Aggregator – Cross Border (31 Oct 2023): https://rbi.org.in/Scripts/NotificationUser.aspx?Id=12561&Mode=0
- [RBI2] RBI, Master Direction on Regulation of Payment Aggregators: https://www.rbi.org.in/Scripts/BS_ViewMasDirections.aspx?id=12896

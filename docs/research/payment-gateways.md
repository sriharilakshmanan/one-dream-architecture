# Payment gateways for a sole proprietor without GST

Research for issue #2. Researched 5 November 2026 from official gateway docs, help centres and pricing pages. Gateways change their onboarding flows often, so check every claim again in the gateway dashboard when signing up.

## Answer

- **Shortlist: Razorpay (first choice) and Cashfree (backup).** Both state in their own docs that you can onboard without a GSTIN. Both have a hosted checkout, signed webhooks and a redirect to our own URL, with no setup or annual fee.
- **Ruled out:** **Stripe India** is invite-only for new accounts. **PayPal India** no longer takes domestic (INR, India-to-India) payments, so it can only be an add-on for foreign Registrants. **PayU** and **PhonePe PG** would probably work, but their docs give nothing better and say less about the no-GST path.
- **The catch is international cards.** Razorpay lets an *unregistered/individual* account onboard with only PAN, CKYC/Aadhaar and a bank account. That account cannot take international cards, though; it has to use PayPal instead. To take foreign cards directly, the Firm needs to onboard as a **registered proprietorship**, which needs **2 business proofs**. A **Udyam (MSME) certificate** is free, online, needs no documents and is issued when you finish the form, so it is the quickest first proof. The second can be a Shop & Establishment certificate, a postpaid mobile bill, or an IEC.
- **Receipts:** we should generate our own downloadable receipt on the thank-you page. The gateways' receipts are emailed, or downloadable from the merchant dashboard. None of the docs describe a receipt URL the Registrant can open after the redirect. See the receipts section below.
- **Is 16 November realistic?** Yes for domestic payments, if KYC is submitted by about 7–9 November. Cashfree says activation typically takes 24 working hours after full documents. Razorpay gives no SLA, except a paid "assisted onboarding" that promises activation within 8 business hours. International cards need a separate approval step with **no published timeline**, so they are the real risk for launch. Fallbacks: launch domestic-only, or link PayPal for foreign Registrants.

## Comparison

| | Razorpay | Cashfree | PayU | PhonePe PG | Stripe India | PayPal India |
|---|---|---|---|---|---|---|
| Onboards without GSTIN | Yes: "I don't have a GSTIN" option; unregistered/individual path needs no business documents [R1][R2] | Yes: GST is only one of several business proofs; without proofs the account becomes "Unregistered" [C1] | Probably: GSTIN is one of "any two" proofs [U1] | Probably: proprietor needs 2 of Udyam/S&E/ITR/utility bill [P1] | **New accounts invite-only** [S1] | **No domestic payments since 1 Apr 2021** [PP1][PP2] |
| Proprietor KYC | PAN, Aadhaar/voter ID/passport, bank account, **2 of** Udyam / GST / S&E / IEC / postpaid mobile bill [R2] | PAN, Aadhaar/CKYC, bank account in owner's name; 2 business proofs if automated checks fail (Udyam, S&E, municipal, tax returns, professional licence…) [C1] | 2 of GSTIN / MSME / ITR / S&E / trade licence / utility bill [U1] | Proprietor CDD + 2 business proofs [P1] | n/a | n/a |
| Activation time | No SLA; CKYC can take "minutes"; paid assisted onboarding "within 8 business hours" [R1] | "Typically 24 working hours" after all documents [C1] | Not verified | Not verified | n/a | n/a |
| Website before go-live | About, Contact, Pricing, T&C, Privacy, Refund pages + sample invoice; Razorpay can generate policy pages [R3] | Live site; business name must match KYC; contact details, privacy, refund, terms [C1][C4] | Not verified | Not verified | n/a | n/a |
| Hosted checkout | Standard Checkout (Orders API) or no-code Payment Pages [R4][R5] | Hosted Web Checkout or no-code Payment Forms [C2][C3] | Yes (not examined) | Yes (not examined) | n/a | n/a |
| Redirect back | `callback_url` gets a POST with `razorpay_payment_id`, `razorpay_order_id`, `razorpay_signature` [R5]; Payment Pages can "Redirect to your website" (docs don't say whether any parameters are appended) [R4] | `return_url` set per order; put the order id in it yourself [C2] | — | — | — | — |
| Webhooks | Yes: `payment.captured`, `order.paid`, `payment.failed`… [R6] | Yes: HMAC-SHA256 signed; `payment_form_order_webhook` for forms [C3] | — | — | — | — |
| Receipts | Payment Pages: automatic PDF receipt emailed to the customer; merchant can download it from the dashboard [R7] | Payment Forms have a "Download Invoice" success page, **but turning it on stops the auto-redirect** [C5] | — | — | — | — |
| Fees (+18% GST on the fee) | 2% domestic (incl. UPI); up to 3% international; ₹0 setup/AMC [R8] | 1.95% domestic; 2.99% international Visa/MC; ₹0 setup [C6] | 2% domestic; 3% international; ₹0 setup [U2] | Not verified | — | — |

## Details that matter for us

### International Registrants
- Razorpay: a registered business "with a valid website" can take international cards once approved. Individuals and freelancers must integrate **PayPal** instead [R9]. International cards need T&C, Privacy, Refund **and Shipping** policy pages [R3][R9]. For a webinar, a "Shipping/Delivery" page that says delivery is digital (by joining link) should satisfy this; that is an inference, not checked with Razorpay. Approval time is not published; the only stated time is video KYC (~3–4 min) [R9].
- Cashfree: international cards at 2.99%. PayPal can be added through Cashfree with no extra Cashfree charge [C6]. Eligibility details are marked "pending verification" in their own FAQ [C4].

### Receipts and the thank-you page
- Neither gateway's docs describe a receipt link that comes back in the redirect or webhook payload. Razorpay emails a PDF and the merchant downloads it from the dashboard [R7]. If Cashfree's invoice page is turned on, the Registrant stays on Cashfree's page and is not sent back to our site [C5].
- **So:** the thank-you page should make its own receipt from our booking record, once the payment is confirmed by webhook or a server-side status check. Both gateways warn against trusting the browser redirect alone [R5][C2]. A sole proprietor with no GST registration cannot issue a GST *tax invoice*, so the receipt must not show a GSTIN or GST breakdown until registration is done. (This is general GST knowledge, not taken from gateway docs. Confirm the wording with the Firm's accountant.)

### Udyam registration (fastest business proof)
- Official portal: "totally free", "paperless and based on self-declaration", Aadhaar-based, no documents to upload, and the certificate is issued online when registration is complete. PAN and GSTIN are needed only "as applicable under the CGST Act", so a proprietor below the GST threshold can register without a GSTIN [UD1].

## Recommended path to 16 November
1. Today: register on Udyam (about 1 hour). Get a second proof: a postpaid mobile bill in the proprietor's name is the quickest; a Shop & Establishment certificate depends on the state.
2. Publish the policy pages (About, Contact, Pricing, T&C, Privacy, Refund/Cancellation, Shipping/Delivery) on the live domain. The business name must match the KYC name.
3. Sign up on Razorpay as a registered **Proprietorship**, with "I don't have a GSTIN". If speed matters, pay for assisted onboarding. Apply for International Cards straight away.
4. At the same time, sign up on Cashfree as the backup. Signing up costs nothing (₹0 setup).
5. Build against test mode now: Orders API + Standard Checkout + `callback_url` to the thank-you page, plus an `order.paid` webhook.
6. If international cards aren't approved by about 13 November, launch domestic-only, or link PayPal for foreign Registrants.

## Not verified
- Real activation times for a no-GST proprietor (vendors publish none, apart from Cashfree's "typically 24 working hours").
- Whether Razorpay Payment Pages add payment parameters to the redirect URL.
- How long international card approval takes, at either gateway.
- PhonePe PG fees and integration details; PayU activation time and website rules.
- Whether Razorpay's 2% UPI fee is negotiable (UPI itself is zero-MDR; the 2% is called a platform fee) [R8].
- Pricing and onboarding of non-GST merchants versus GST merchants (no source says they differ).

## Sources
- [R1] Razorpay, Set up account: https://razorpay.com/docs/payments/set-up/
- [R2] Razorpay, Business types & KYC documents: https://razorpay.com/docs/payments/business-types-kyc-documents/
- [R3] Razorpay, Business website details: https://razorpay.com/docs/payments/dashboard/account-settings/business-website-details/
- [R4] Razorpay, Create a Payment Page: https://razorpay.com/docs/payments/payment-pages/create/
- [R5] Razorpay, Callback URL: https://razorpay.com/docs/payment-gateway/callback-url
- [R6] Razorpay, Webhooks: https://razorpay.com/docs/webhooks/ ; Payment Pages webhooks: https://razorpay.com/docs/payments/payment-pages/subscribe-to-webhooks/
- [R7] Razorpay, Payment Page receipts: https://razorpay.com/docs/payments/payment-pages/receipt/
- [R8] Razorpay, Pricing: https://razorpay.com/pricing/
- [R9] Razorpay, International payments: https://razorpay.com/docs/payments/international-payments/
- [C1] Cashfree, Activate your account: https://www.cashfree.com/docs/help/account/account-activation
- [C2] Cashfree, Hosted web checkout: https://www.cashfree.com/docs/payments/online/web/redirect
- [C3] Cashfree, Payment Form webhooks: https://www.cashfree.com/docs/payments/no-code/payment-forms/webhooks
- [C4] Cashfree, Onboarding FAQs: https://www.cashfree.com/docs/help/onboarding-related/onboarding-faqs ; Payments FAQs: https://www.cashfree.com/docs/payments/general-faqs
- [C5] Cashfree, Payment Forms FAQs: https://www.cashfree.com/docs/help/payment-forms/faqs/faqs
- [C6] Cashfree, Payment gateway charges: https://www.cashfree.com/payment-gateway-charges/
- [U1] PayU, Documents checklist: https://docs.payu.in/docs/documents-checklist-for-account-activation
- [U2] PayU, Pricing: https://payu.in/pricing/
- [P1] PhonePe, Merchant profiling guidelines (PDF): https://www.phonepe.com/apollo/pdf/Merchant_Profiling_Guidelines.pdf
- [S1] Stripe, Accounts are invite-only in India: https://support.stripe.com/questions/stripe-accounts-are-invite-only-in-india
- [PP1] PayPal, India integration guidance: https://developer.paypal.com/docs/regional/in/
- [PP2] PayPal, Receiving payments from Indian customers: https://www.paypal.com/in/cshelp/article/can-i-use-paypal-to-receive-payments-from-indian-customers-help1049
- [UD1] Udyam Registration portal (Ministry of MSME): https://udyamregistration.gov.in/

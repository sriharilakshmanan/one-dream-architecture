# GST obligations for paid Webinars before (and after) registration

Researched 5 November 2026 for issue #4 (map #1).

> **This is research, not legal or tax advice.** It reads the statute, rules and CBIC notifications/circulars as published. Points marked **[CA]** need a Chartered Accountant's confirmation before launch on 16 November 2026.

## Short answer

1. **Yes, the Firm can sell before its GSTIN is granted**, as long as its *aggregate turnover* for FY 2026-27 (all supplies on its PAN, including architecture fees and exports) stays within ₹20 lakh (₹10 lakh if registered in Manipur, Mizoram, Nagaland or Tripura). Selling online across states or abroad does **not** by itself force registration for a service provider.
2. **Before registration the Firm must not charge or show GST.** The receipt is a plain payment receipt with no GST line, no GSTIN and no "tax invoice" title.
3. **After registration, every paid Webinar needs a GST tax invoice** (Rule 46 fields, listed below). India sales: 18% GST (CGST+SGST or IGST by place of supply). Foreign sales: zero-rated export under an LUT, with the export endorsement on the invoice. **An LUT needs a GSTIN, so it is unavailable until registration.**
4. If turnover already crossed ₹20 lakh, registration was **already compulsory**. Apply within 30 days of crossing so the registration takes effect from that date. Then issue revised tax invoices for sales made between that date and the date the certificate is granted.

## 1. Can the Firm sell before registration completes?

### Threshold
- CGST Act **s.22(1)**: registration is required once aggregate turnover in a financial year exceeds ₹20 lakh (₹10 lakh in special category States). For a service-only supplier, this is the threshold that applies. The ₹40 lakh limit in Notification 10/2019-CT is for goods only.
- **Aggregate turnover** (CGST s.2(6)) is computed PAN-wide. It **includes exports and exempt supplies** and excludes taxes. Webinar income from abroad therefore counts towards the ₹20 lakh, alongside the Firm's architecture fees. **[CA]** Confirm the Firm's FY 2026-27 turnover to date.

### Compulsory registration (s.24) — none applies on the stated facts
| s.24 clause | Applies? | Why |
|---|---|---|
| (i) inter-State taxable supply | **No** | IGST **Notification 10/2017-Integrated Tax** (13 Oct 2017, under s.23(2)) exempts persons making inter-State supplies of *services* from registration while aggregate turnover is within the threshold. An export of services is an inter-State supply (IGST s.7(5)(a)), so this exemption covers foreign Registrants too. |
| (ix) supplies through an e-commerce operator (ECO) that collects TCS | **No, if sold on the Firm's own site** | A payment gateway only processes payments; it is not an ECO collecting TCS under s.52. Service suppliers selling through an ECO are in any case exempt below the threshold under **Notification 65/2017-CT**. **[CA]** If tickets are sold through a marketplace such as an event-ticketing platform, confirm whether that platform is an ECO. |
| (x)/(xi) OIDAR supplied from **outside** India | **No** | These clauses target non-resident suppliers. The Firm is located in India. |

**Is a Webinar an OIDAR service?** IGST s.2(17) defines OIDAR as services "essentially automated and involving minimal human intervention". A live, presenter-led Webinar is unlikely to qualify. A pre-recorded, on-demand product might. This barely matters for an Indian supplier below the threshold, but **[CA]** should confirm it if recordings are sold separately.

### Voluntary registration / effective date
- If the Firm was **not** liable (turnover ≤ ₹20 lakh), the application is voluntary under s.25(3). Registration then takes effect from the **date of grant** (CGST Rule 10). Sales before that date are outside GST for the Firm. It collects nothing, files nothing and cannot claim input tax credit on them.
- If the Firm **was** liable, s.25(1) and Rule 10(2) make registration effective from the date liability arose, provided it applied within 30 days of that date. Otherwise it takes effect from the date of grant, and the period in between is a default.

## 2. What the receipt must say before registration

- CGST **s.32(1)**: "A person who is not a registered person shall not collect in respect of any supply … any amount by way of tax." So: **no GST amount, no GST rate and no "Tax Invoice" heading**.
- No GST rule prescribes a format for an unregistered person's receipt. A bill of supply (Rule 49) is a registered-person document. A plain receipt with a serial number, date, the Firm's name and address, Registrant name, the Webinar, the amount in INR and the payment reference is enough.
- Wording that is safe to add: "Firm not registered under GST; no GST charged." Avoid "GST included" or "inclusive of taxes".
- **Revised invoices after grant:** CGST **s.31(3)(a)** and **Rule 53(2)** allow revised tax invoices, issued within **one month of the certificate date**, for supplies made between the *effective date of registration* and the *date of the certificate*. This matters only if registration is backdated (the mandatory case). **Implication for the product:** the receipt system should allow the same payment to be re-issued later as a tax invoice. **[CA]**

## 3. After registration: tax invoice

### Rate and classification
- Commercial training/coaching: SAC heading **9992** (e.g. 999293), **18% GST** (Notification 11/2017-CT(Rate), as amended). **[CA]** Confirm the SAC and rate, including after the 2025 rate rationalisation.
- **Domestic, intra-State** (Registrant in the Firm's State): CGST 9% + SGST 9%. **Inter-State:** IGST 18%.
- **Place of supply for an online Webinar [CA]:** IGST s.12(2) (default: the recipient's location; for unregistered recipients, their address on record) vs s.12(6) (admission to an educational event: "where the event is actually held"). For an online event this is unsettled. The practical choice is to collect the Registrant's State at checkout and treat it as the place of supply.
- **Foreign Registrants: export of services** if all five conditions of **IGST s.2(6)** hold: supplier in India; recipient outside India; place of supply outside India (s.13(2) default = recipient's location); payment received in **convertible foreign exchange or in INR where permitted by RBI**; supplier and recipient not establishments of the same person.

### Exports without paying IGST (LUT)
- IGST **s.16** zero-rates exports. They can be made **without paying IGST under a Letter of Undertaking** (LUT, form **GST RFD-11**, renewed each FY; CGST Rule 96A; Notification 37/2017-CT, Circular 8/8/2017-GST). Only a *registered* person can file an LUT.
- Without an LUT, the Firm must charge IGST at 18% and then claim a refund.
- **Payment condition — INR via an Indian gateway [CA, important]:** the Firm prices in INR and settles through an Indian payment gateway. s.2(6)(iv) needs the money to arrive as convertible foreign exchange or as INR that RBI permits. CBIC **Circular 202/14/2023-GST** (Special Rupee Vostro) and, per secondary reports, **Circular 263/09/2026-GST (26 Oct 2026)** test whether the money came through a channel that can be repatriated (e.g. SNRR or INR Nostro accounts). Neither circular, as far as could be verified, addresses card payments through a domestic gateway. **The official CBIC PDF of Circular 263 could not be fetched (certificate error), so its content is unverified.** Ask the CA and the gateway whether the gateway's cross-border (PA-CB) settlement plus its FIRC/e-FIRA counts as foreign-exchange receipt. If it does not, foreign sales would be taxable at 18% IGST rather than zero-rated exports.

### Mandatory invoice fields (CGST s.31 + Rule 46)
1. Supplier name, address, **GSTIN**
2. Consecutive serial number, max 16 characters, unique for the financial year (`-` and `/` allowed)
3. Date of issue (within **30 days** of supply of service, Rule 47)
4. Recipient name and address; **for unregistered recipients the State name and code are required when the value is ≥ ₹50,000**. Cheaper Webinars do not need the full address.
5. SAC code. Optional on B2C invoices for turnover ≤ ₹5 crore (Notification 78/2020-CT). 4 digits are required on B2B invoices.
6. Description of service
7. Taxable value (after discounts)
8. Tax rate and amount, split into CGST/SGST or IGST
9. **Place of supply with State name** (inter-State)
10. Whether tax is payable on reverse charge ("No")
11. Signature or digital signature of the supplier or authorised representative (Rule 46(q)). **[CA]** Confirm whether a system-generated PDF needs a DSC.
- **Export invoice extras** (Rule 46 proviso): the endorsement **"SUPPLY MEANT FOR EXPORT UNDER BOND OR LETTER OF UNDERTAKING WITHOUT PAYMENT OF INTEGRATED TAX"** (or "…ON PAYMENT OF INTEGRATED TAX"), plus the recipient's name, address and **country**.
- **Not needed at this size:** e-invoicing/IRN (aggregate turnover > ₹5 crore) and the B2C dynamic QR code (> ₹500 crore).

## Implications for the build (16 Nov launch)
- Checkout should collect the Registrant's **name, email, country and Indian State** (for place of supply and the invoice).
- The receipt template should carry a flag `gst_registered`. Before the GSTIN is granted, it shows the plain receipt with no tax. After, it shows the Rule 46 tax invoice, with the export endorsement for foreign Registrants.
- Use one serial number series per financial year from day one. It carries straight into tax invoices.
- Keep the gateway's payment ID and, for foreign payments, the FIRC/e-FIRA reference for each receipt.

## Questions for the CA
1. FY 2026-27 aggregate turnover to date. Is registration voluntary or already compulsory, and what is its effective date?
2. Does INR card settlement through the chosen Indian gateway satisfy IGST s.2(6)(iv) (with Circular 263/09/2026 checked)? If not, foreign sales are taxable.
3. Place of supply for an online Webinar to an unregistered Indian Registrant: s.12(2) or s.12(6)?
4. SAC and rate (999293 @ 18%?). Is a recorded-Webinar product OIDAR?
5. Is the chosen ticketing or payment setup an e-commerce operator?
6. Can the invoice be system-generated without a DSC?

## Sources (primary)
- CGST Act 2017: s.2(6), s.22, s.23, s.24, s.25, s.31, s.32, s.52 — https://cbic-gst.gov.in/gst-acts.html
- IGST Act 2017: s.2(6), s.2(17), s.7(5), s.12, s.13, s.16 — https://cbic-gst.gov.in/gst-acts.html
- CGST Rules 2017: Rules 10, 46, 47, 49, 53, 96A — https://cbic-gst.gov.in/central-tax-rules.html
- Notification 10/2017-Integrated Tax (13 Oct 2017), exemption from registration for inter-State service suppliers below the threshold — https://cbic-gst.gov.in/integrated-tax-notifications.html
- Notification 65/2017-Central Tax, exemption for service suppliers selling through an ECO — https://cbic-gst.gov.in/central-tax-notifications.html
- Notification 37/2017-Central Tax and Circular 8/8/2017-GST, LUT for all exporters — https://cbic-gst.gov.in/central-tax-notifications.html
- Notification 78/2020-Central Tax, HSN/SAC digits on invoices
- Notification 11/2017-Central Tax (Rate), service rates
- Circular 78/52/2018-GST, export of services — https://cbic-gst.gov.in/pdf/Circular_78-52-2018_Export_Services.pdf
- Circular 202/14/2023-GST, s.2(6)(iv) and Special Rupee Vostro — https://gstcouncil.gov.in/sites/default/files/2024-06/circular-no-202-14-2023.pdf
- Circular 263/09/2026-GST (26 Oct 2026), s.2(6)(iv) repatriable INR accounts — https://taxinformation.cbic.gov.in/view-pdf/1003354/ENG/Circulars (**not fetched; content taken from a secondary summary**: https://www.corplawupdates.in/updates/cbic-circular-263-gst-export-payment-realisation-clarified)

Caveat on method: the section and rule numbers above come from the published Acts and Rules. Some notification wording was checked only through secondary reproductions, because the CBIC PDFs were unreadable or unreachable from this environment. Check each against the CBIC text before relying on it.

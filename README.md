# Sun Life (sun-life)

Sun Life Financial Inc. is a Canadian multinational life and health insurer and asset manager headquartered in Toronto, one of the "life trio" that dominates Canada's insurance market alongside Manulife and Great-West Lifeco. Sun Life writes individual life insurance, individual and group health and dental coverage, group benefits, disability and absence management, annuities and retirement products, and runs a large wealth business through Sun Life Global Investments, SLC Management and MFS Investment Management, with operations across Canada, the United States, the United Kingdom, Ireland and Asia. In the United States it is a leading group benefits, stop-loss and dental carrier and owns DentaQuest.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/sun-life/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/sun-life/refs/heads/main/apis.yml)

## Tags

- Insurance
- Canada
- Life Insurance
- Health Insurance
- Employee Benefits
- Group Benefits
- Dental Insurance
- Disability
- Wealth Management
- Financial Services
- Carrier

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

Three, all from **DentaQuest**, the Sun Life U.S. dental company — not from the Sun Life brand itself. DentaQuest runs a public Azure API Management developer portal at [developers.dentaquest.com](https://developers.dentaquest.com/) publishing OpenAPI 3.0.1 for three FHIR R4 APIs released under the CMS Interoperability and Patient Access Final Rule (CMS-9115-F). All three specs are harvested to `openapi/`.

| API | Base URL | Auth | Paths |
| --- | --- | --- | --- |
| FHIR Patient Access | `https://api.dentaquest.com/FhirPatientAccess/v1` | SMART on FHIR (OAuth 2.0 code + PKCE) via Okta CIAM | 31 |
| FHIR Provider Directory | `https://api.dentaquest.com/FhirProviderDirectory` | Azure APIM subscription key | 36 |
| FHIR Metadata | `https://api.dentaquest.com/fhirmetadata` | anonymous | 1 |

Patient Access instantiates US Core 3.1.1 / 6.1.0 / 7.0.0-ballot and publishes a live [SMART configuration](https://api.dentaquest.com/FhirPatientAccess/v1/.well-known/smart-configuration) with real scopes (`patient/*.read`, `launch/patient`, `offline_access`, `openid`, `fhirUser`). Provider Directory conforms to Da Vinci PDex Plan-Net 1.1.0. Both specs list `api.deltadentalma.com` as a second production server — the same backend serves the Delta Dental of Massachusetts brand. Credentials are not self-serve: a [developer questionnaire](https://www.dentaquest.com/en/interoperability-api) is reviewed and production credentials are emailed.

Under the **Sun Life brand there is still no public API**. Probed on 2026-07-25: `developer.sunlife.com`, `developers.sunlife.com`, `docs.sunlife.com`, `api.sunlife.com`, `apis.sunlife.com`, `gateway.sunlife.com` and `partners.sunlife.com` do not resolve in DNS, and neither do the equivalents under `sunlife.ca`. The paths `/developers`, `/developer`, `/partners` and `/integrations` on `www.sunlife.com` all return HTTP 404, as do `/openapi.json`, `/swagger.json` and `/api-docs`. The `www.sunlife.com/api` vanity path redirects to the Sun Life U.S. [Digital capabilities](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/) marketing page.

## Sun Life Link — the real integration surface

Sun Life's only named API surface is **Sun Life Link**, described on its own product page as "our API solution" delivering "real-time data exchanges with the HR platforms you use every day." It is a partner-gated U.S. group-benefits connectivity program, not a developer program: connections are negotiated per client and per platform, and the company's own RFP guidance tells buyers to ask carriers for "a breakdown of pricing per API" and whether APIs are "offered to all clients, or only a subset."

Named platform connections and live capabilities, as published:

| Platform | Capabilities |
| --- | --- |
| Workday | EOI, Billing, Absence (leave create/update, intermittent leave, return-to-work, leave consolidation); Plan Setup and Enrollment via Workday Wellness listed as Q4 |
| ADP Workforce Now | EOI, Plan Setup, self-bill; Enrollment in early adopter |
| ADP Vantage HCM / Health & Welfare Service Engine / ADP Lyric HCM | EOI, Plan Setup; Enrollment a future enhancement |
| UKG Pro Benefits Administration | EOI |
| PlanSource | EOI |
| Employee Navigator | Plan Setup; EOI a future enhancement |
| bswift | EOI |

Sun Life also states "general connectivity with 150 + other HR platforms."

Of the four insurance API verbs, **quote**, **bind** and **FNOL** are not exposed publicly at all. **Issue** is present only in partner-gated form as Plan Setup, Enrollment and Evidence of Insurability exchanges. Claims are submitted and tracked through the login-gated Sun Life Connect employer portal and the member account.

## ACORD posture

**No ACORD reference found.** No mention of ACORD, AL3, ACORD XML, NGDS, IVANS, agency download, Applied Epic or Vertafore AMS360 appears on any Sun Life page or in either public API whitepaper. As a life/health and group-benefits carrier rather than a P&C carrier distributing through independent agencies, Sun Life sits off the ACORD/IVANS rail entirely. Its connectivity vocabulary is generic — the Connectivity 101 whitepaper frames the choice as EDI batch files versus real-time APIs, and names no standards body, not even LIMRA/LDEx.

## Market context

Canada has the most fragmented insurance supervision of the major markets: OSFI supervises federally-regulated insurers prudentially while the provinces regulate market conduct through FSRA in Ontario, the AMF in Quebec and their counterparts elsewhere. There is no open-insurance mandate, and Consumer-Driven Banking — Canada's open-banking framework — excludes insurance entirely. Nothing compels a Canadian carrier to publish an API, and Sun Life does not. What pull exists comes from the U.S. group-benefits BenTech ecosystem, and Sun Life answers it with negotiated platform partnerships instead of an open developer program.

## Links

- [Website](https://www.sunlife.com/)
- [About Us](https://www.sunlife.com/en/about-us/)
- [Sun Life Canada](https://www.sunlife.ca/)
- [Sun Life U.S.](https://www.sunlife.com/us/en/)
- [Digital capabilities](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/)
- [Sun Life Link](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/sun-life-link/)
- [Sun Life Connect](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/sun-life-connect/)
- [Sun Life Onboard](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/sun-life-onboard/)
- [Connectivity education materials](https://www.sunlife.com/us/en/employers/products-and-services/digital-capabilities/sun-life-link/navigating-connectivity-in-benefits-administration/)
- [Sign in](https://login.sunlifeconnect.com/commonlogin/)
- [Canadian advisor community](https://connect.sunlife.ca/s/)
- [Investor relations](https://www.sunlife.com/en/investors/)
- [Newsroom](https://www.sunlife.com/en/newsroom/)
- [Careers](https://www.sunlife.com/en/careers/)
- [LinkedIn](https://www.linkedin.com/company/sun-life-financial)

## Maintainers

- Kin Lane — kin@apievangelist.com

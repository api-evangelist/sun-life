---
name: DentaQuest Patient Access — SMART on FHIR standalone launch
description: Authorize a member with SMART App Launch, then read their dental coverage and demographics from the DentaQuest (Sun Life U.S.) Patient Access API.
api: openapi/sun-life-dentaquest-fhir-patient-access-openapi.json
generated: '2026-07-25'
method: generated
operations:
  - well-known-smart-configuration
  - authorize
  - token
  - get-metadata
  - get-patient
  - get-patient-id
  - get-coverage
  - get-coverage-id
---

# DentaQuest Patient Access — SMART on FHIR standalone launch

Read-only FHIR R4 access to one member's dental data, released under the CMS
Interoperability and Patient Access Final Rule (CMS-9115-F). Base URL:
`https://api.dentaquest.com/FhirPatientAccess/v1` (the same API is served for
Delta Dental of Massachusetts at `https://api.deltadentalma.com/FhirPatientAccess/v1`).

## Before you start

You cannot self-register. Request a `client_id` and `client_secret` through the
DentaQuest developer questionnaire form linked from
<https://www.dentaquest.com/en/interoperability-api>; credentials arrive by
secure email after review.

## Steps

1. **Discover the SMART endpoints.** `GET` `well-known-smart-configuration`
   (`/.well-known/smart-configuration`). Use the returned
   `authorization_endpoint` and `token_endpoint` rather than hard-coding them.
   Note: the published document's `issuer` and `jwks_uri` contain a typo
   (`/oath2/`); the working Okta paths use `/oauth2/`.
2. **Check server capability.** `GET` `get-metadata` (`/metadata`) and confirm
   `fhirVersion` is `4.0.1` and the SMART OAuth URIs extension is present.
3. **Authorize the member.** Send the user to `authorize` (`/authorize`) with
   `response_type=code`, your `client_id`, `redirect_uri`, `state`, a PKCE
   `code_challenge` using `S256`, and
   `scope=launch/patient openid fhirUser patient/*.read offline_access`.
   `launch/patient` binds the session to a single member — every subsequent read
   is scoped to that compartment.
4. **Exchange the code.** `POST` `token` (`/token`) with
   `grant_type=authorization_code`, the `code`, `redirect_uri` and
   `code_verifier`. Authenticate the client with `client_secret_basic` or
   `private_key_jwt`. Keep the `refresh_token` returned under `offline_access`.
5. **Read the member.** `GET` `get-patient` (`/Patient`) to resolve the in-context
   member, then `get-patient-id` (`/Patient/{id}`) for the full record.
6. **Read coverage.** `GET` `get-coverage` (`/Coverage`) for the member's dental
   benefit plans and `get-coverage-id` (`/Coverage/{id}`) for one plan.
   Follow `Coverage.payor` to `get-organization` (`/Organization`) when you need
   the payer, and `get-relatedperson` (`/RelatedPerson`) for a personal
   representative.

## Rules

- Every data operation is a **GET**. There is no write surface and no
  idempotency-key contract (`conventions/sun-life-conventions.yml`).
- Send `Accept: application/fhir+json`. Responses are FHIR resources; searches
  return a `Bundle` of type `searchset` — page with `Bundle.link[relation=next]`,
  never by guessing offsets.
- Errors carry a FHIR `OperationOutcome`; read `issue[].severity`,
  `issue[].code` and `issue[].diagnostics`
  (`errors/sun-life-problem-types.yml`). A `403` means the member has not
  granted your application access — re-run consent, do not retry blindly.
- No rate limit is published. Back off on `429`/`5xx` rather than assuming a quota.
- Claims data is released only "with the approval and at the direction of an
  applicable member or the member's personal representative" — treat the token
  as the consent record.

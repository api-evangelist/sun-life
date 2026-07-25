---
name: DentaQuest Provider Directory — find an in-network dentist
description: Search the DentaQuest (Sun Life U.S.) Da Vinci PDex Plan-Net provider directory for dentists, locations, services and dental plans using an APIM subscription key.
api: openapi/sun-life-dentaquest-fhir-provider-directory-openapi.json
generated: '2026-07-25'
method: generated
operations:
  - get-metadata
  - get-practitioner
  - get-practitionerrole
  - get-practitionerrole-id
  - get-location
  - get-organization
  - get-healthcareservice
  - get-insuranceplan
  - get-endpoint
---

# DentaQuest Provider Directory — find an in-network dentist

Public-facing FHIR R4 provider directory conforming to the HL7 Da Vinci PDex
Plan-Net Implementation Guide 1.1.0, published under CMS-9115-F. Base URL:
`https://api.dentaquest.com/FhirProviderDirectory`
(also `https://api.deltadentalma.com/FhirProviderDirectory`).

## Before you start

Request an API key through the DentaQuest developer questionnaire form linked
from <https://www.dentaquest.com/en/interoperability-api>, selecting
"Provider Directory". Send it on every call as the
`Ocp-Apim-Subscription-Key` header (or the `subscription-key` query parameter).
Without it the gateway returns `401 {"statusCode":401,"message":"Access denied
due to missing subscription key..."}` before the request reaches FHIR.

## Steps

1. **Confirm capability.** `GET` `get-metadata` (`/metadata`) to see which search
   parameters the server supports for each resource — the exported OpenAPI does
   not enumerate them.
2. **Start from the plan.** `GET` `get-insuranceplan` (`/InsurancePlan`) to
   identify the dental product/network the member is enrolled in.
3. **Search the network join.** `GET` `get-practitionerrole`
   (`/PractitionerRole`) — in Plan-Net this is the entity that ties a dentist to
   an organization, a location, a healthcare service and a network. Fetch one
   with `get-practitionerrole-id` (`/PractitionerRole/{id}`).
4. **Resolve the references.** Follow `PractitionerRole.practitioner` to
   `get-practitioner` (`/Practitioner`), `PractitionerRole.location` to
   `get-location` (`/Location`), `PractitionerRole.organization` to
   `get-organization` (`/Organization`) and
   `PractitionerRole.healthcareService` to `get-healthcareservice`
   (`/HealthcareService`).
5. **Optional technical contacts.** `GET` `get-endpoint` (`/Endpoint`) for the
   electronic endpoints attached to a directory entry.

## Rules

- Read-only: every operation is a GET. All resources also expose `_history` and
  `/{id}/_history/{vid}` for version reads.
- Page with `Bundle.link[relation=next]`; the directory is large.
- Directory accuracy is regulated — report errors through
  <https://www.dentaquest.com/en/contact-us/report-provider-directory-error>
  rather than assuming a stale record is an API bug.
- Errors are FHIR `OperationOutcome` bodies once past the gateway; the gateway's
  own errors use `{statusCode, message}` (`errors/sun-life-problem-types.yml`).

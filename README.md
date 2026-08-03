# Amazon IoT Device Management (amazon-iot-device-management)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AWS IoT Device Management makes it easy to securely onboard, organize, monitor, and remotely manage your IoT devices at scale. You can register your connected devices individually or in bulk, and easily manage permissions so your devices remain secure throughout their lifecycle.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amazon-iot-device-management/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Device Management, Fleet Management, IoT, OTA Updates

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### AWS IoT Device Management API
The AWS IoT Device Management API provides access to thing groups, jobs, bulk registration, fleet indexing, and remote device management capabilities.

**Human URL:** [https://aws.amazon.com/iot-device-management/](https://aws.amazon.com/iot-device-management/)

#### Tags:

 - Device Management, Fleet Management, IoT

#### Properties

- [Documentation](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)
- [OpenAPI](openapi/amazon-iot-device-management-openapi-original.yml)
- [GettingStarted](https://docs.aws.amazon.com/iot/latest/developerguide/iot-thing-management.html)
- [Pricing](https://aws.amazon.com/iot-device-management/pricing/)
- [FAQ](https://aws.amazon.com/iot-device-management/faqs/)

## Common Properties

- [Portal](https://aws.amazon.com/iot-device-management/)
- [Website](https://aws.amazon.com/iot-device-management/)
- [Documentation](https://docs.aws.amazon.com/iot/latest/developerguide/iot-thing-management.html)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/iot/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/iot/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [Login](https://signin.aws.amazon.com/)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)

## Features

| Name | Description |
|------|-------------|
| Bulk Device Registration | Register thousands of devices simultaneously using bulk registration templates. |
| Fleet Indexing | Search and query your entire device fleet based on attributes and shadow state. |
| Remote Jobs | Deploy firmware updates, configuration changes, and software remotely at scale. |
| Thing Groups | Organize devices into hierarchical groups for policies and bulk operations. |
| Tunnel Secure | Create secure bidirectional tunnels to devices behind firewalls for remote troubleshooting. |

## Use Cases

| Name | Description |
|------|-------------|
| OTA Firmware Updates | Deploy firmware updates to thousands of devices simultaneously with rollback. |
| Device Onboarding | Automate device provisioning and certificate management at manufacturing. |
| Fleet Monitoring | Monitor device connectivity, metadata, and shadow state across the entire fleet. |

## Integrations

| Name | Description |
|------|-------------|
| AWS IoT Core | All device management operations integrate with IoT Core connectivity. |
| AWS Lambda | Trigger automation workflows based on job status changes. |
| Amazon S3 | Store firmware and configuration files for remote deployment. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [AWS IoT Device Management API](openapi/amazon-iot-device-management-openapi-original.yml)

### JSON Schema

200 schema files covering key resources and operations.

### JSON Structure

200 JSON Structure files converted from JSON Schema.

### JSON-LD

- [Amazon IoT Device Management Context](json-ld/amazon-iot-device-management-context.jsonld)

### Examples

200 example JSON files generated from schemas.

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [AWS IoT Device Management API](capabilities/shared/iot-device-management.yaml) — operations for amazon iot device management management

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Iot Fleet Management](capabilities/iot-fleet-management.yaml) | Amazon IoT Device Management | 8 | IoT Engineer, Operations Engineer |

## Vocabulary

- [Amazon IoT Device Management Vocabulary](vocabulary/amazon-iot-device-management-vocabulary.yaml) — Unified taxonomy mapping resources, actions, workflows, and personas

## Rules

- [Amazon IoT Device Management Spectral Rules](rules/amazon-iot-device-management-spectral-rules.yml) — 14 rules across 6 categories enforcing Amazon IoT Device Management API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com

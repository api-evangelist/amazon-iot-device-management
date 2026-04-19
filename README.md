# Amazon IoT Device Management (amazon-iot-device-management)
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

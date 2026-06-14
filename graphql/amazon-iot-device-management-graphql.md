# AWS IoT Device Management GraphQL Schema

## Overview

This GraphQL schema models the AWS IoT Device Management fleet management REST API. It covers device registration, fleet indexing and search, job orchestration for remote management, thing groups, billing groups, certificates, and policies.

**Provider:** Amazon Web Services  
**API Reference:** https://docs.aws.amazon.com/iot/latest/apireference/  
**Base REST Endpoint:** https://iot.amazonaws.com  
**Schema File:** [amazon-iot-device-management-schema.graphql](amazon-iot-device-management-schema.graphql)

---

## Schema Source

The schema was derived from the [AWS IoT API Reference](https://docs.aws.amazon.com/iot/latest/apireference/) covering the following resource domains:

- Things and Thing Types
- Thing Groups and Dynamic Thing Groups
- Billing Groups
- Jobs, Job Executions, and scheduling
- Fleet Indexing and Index Search
- Fleet Metrics and Aggregation
- Certificates and Key Pairs
- Policies and Policy Versions

---

## Type Summary

| Domain | Key Types |
|---|---|
| Things | `Thing`, `ThingDetails`, `ThingList`, `ThingAttribute`, `ThingConnectivity`, `ConnectivitySummary`, `AttributePayload` |
| Thing Types | `ThingType`, `ThingTypeDetails`, `ThingTypeName`, `ThingTypeProperties`, `ThingTypeMetadata` |
| Thing Groups | `ThingGroup`, `ThingGroupDetails`, `ThingGroupMetadata`, `ThingGroupProperties` |
| Dynamic Thing Groups | `DynamicThingGroup` |
| Billing Groups | `BillingGroup`, `BillingGroupDetails`, `BillingGroupProperties`, `BillingGroupMetadata` |
| Jobs | `Job`, `JobDetails`, `JobDocument`, `JobTarget`, `JobSummary`, `JobList`, `JobType` |
| Job Executions | `JobExecution`, `ExecutionDetails`, `ExecutionStatus`, `JobExecutionSummary` |
| Rollout Config | `RolloutConfig`, `ExponentialRolloutRate`, `RateIncreaseCriteria` |
| Retry Config | `RetryConfig`, `RetryCriteria` |
| Timeout Config | `TimeoutConfig`, `InProgressTimeoutInMinutes` |
| Abort Config | `AbortConfig`, `AbortCriteria` |
| Presigned URL | `PresignedUrlConfig`, `PresignedUrl` |
| Fleet | `Fleet`, `FleetDetails`, `FleetIndex`, `FleetIndexing`, `FleetIndexingConfiguration` |
| Fleet Metrics | `FleetMetric`, `FleetMetricDetails`, `FleetMetricList` |
| Index Search | `IndexSearch`, `SearchQuery`, `SearchResults`, `ThingDocument`, `ThingGroupDocument` |
| Aggregation | `AggregationType`, `Bucket`, `BucketAggregation`, `CardinalityAggregation`, `StatisticsAggregation` |
| Certificates | `Certificate`, `CertificateDetails`, `KeyPair`, `KeysAndCertificate` |
| Policies | `Policy`, `PolicyDetails`, `PolicyVersion`, `PolicyVersionList` |
| Auth | `APIKey`, `Token` |

---

## Queries

### Things

- `thing(thingName)` — Fetch a single thing by name
- `listThings(...)` — List things with optional attribute and type filters
- `thingType(thingTypeName)` — Get a thing type definition
- `listThingTypes(...)` — List all registered thing types

### Thing Groups

- `thingGroup(thingGroupName)` — Get group details including metadata
- `listThingGroups(...)` — List groups with optional parent filter
- `listThingsInThingGroup(...)` — List members of a group (recursive supported)
- `listGroupsForThing(...)` — List all groups a thing belongs to

### Billing Groups

- `billingGroup(billingGroupName)` — Get billing group details
- `listBillingGroups(...)` — List billing groups
- `listThingsInBillingGroup(...)` — List things in a billing group

### Jobs

- `job(jobId)` — Full job details including rollout, abort, and retry configuration
- `listJobs(...)` — List jobs with status and group filters
- `jobExecution(jobId, thingName)` — Get a specific execution
- `listJobExecutionsForJob(...)` — All executions for a job
- `listJobExecutionsForThing(...)` — All job executions for a thing
- `jobDocument(jobId)` — Retrieve the raw job document

### Fleet Indexing

- `indexingConfiguration` — Current fleet indexing settings
- `thingIndex(indexName)` — Get a specific index
- `listIndices(...)` — List available indices
- `searchIndex(input)` — Full-text and attribute search across the fleet

### Fleet Metrics

- `fleetMetric(metricName)` — Metric definition and configuration
- `listFleetMetrics(...)` — List configured fleet metrics

### Certificates and Policies

- `certificate(certificateId)` — Certificate details
- `listCertificates(...)` / `listCertificatesByCA(...)` — Browse certificates
- `policy(policyName)` — Policy document and metadata
- `listPolicies(...)` / `listPolicyVersions(...)` — Browse policies

---

## Mutations

### Things and Groups

- `createThing` / `updateThing` / `deleteThing`
- `createThingType` / `deprecateThingType` / `deleteThingType`
- `createThingGroup` / `updateThingGroup` / `deleteThingGroup`
- `createDynamicThingGroup` / `updateDynamicThingGroup` / `deleteDynamicThingGroup`
- `createBillingGroup` / `updateBillingGroup` / `deleteBillingGroup`
- Group membership mutations: `addThingToThingGroup`, `removeThingFromThingGroup`, `addThingToBillingGroup`, etc.

### Jobs

- `createJob` — Full job creation with rollout, abort, retry, scheduling, and presigned URL config
- `updateJob` — Modify rollout and timeout settings on an in-progress job
- `cancelJob` — Cancel with optional reason code
- `deleteJob` — Permanent deletion (force option for in-progress jobs)
- `cancelJobExecution` / `deleteJobExecution` — Per-device execution control

### Fleet Indexing

- `updateIndexingConfiguration` — Enable or modify thing and group indexing modes

### Fleet Metrics

- `createFleetMetric` / `updateFleetMetric` / `deleteFleetMetric`

### Certificates and Policies

- `createKeysAndCertificate` — Generate key pair and certificate
- `deleteCertificate` / `updateCertificate`
- `createPolicy` / `deletePolicy` / `createPolicyVersion` / `setDefaultPolicyVersion`
- `attachPolicy` / `detachPolicy` — Attach policies to things, groups, or certificates

---

## Notable Design Decisions

1. **Job configuration types are fully decomposed** — `RolloutConfig`, `AbortConfig`, `RetryConfig`, `TimeoutConfig`, and `PresignedUrlConfig` are separate reusable types mirroring the AWS API structure.

2. **Fleet indexing is represented at two levels** — `Fleet`/`FleetDetails` for the index resource and `FleetIndexingConfiguration`/`ThingIndexingConfiguration` for the indexing mode settings.

3. **Search results are polymorphic** — `SearchResults` returns both `ThingDocument` and `ThingGroupDocument` lists, reflecting that AWS fleet index search can query across both resource types.

4. **Aggregation types cover all three AWS aggregation modes** — `StatisticsAggregation` (count/mean/stddev), `BucketAggregation` (terms), and `CardinalityAggregation` (distinct count) map to the three `AggregationTypeName` enum values.

5. **APIKey and Token types** are included to represent authentication artifacts that may be issued alongside certificate-based auth flows.

---

## References

- [AWS IoT API Reference](https://docs.aws.amazon.com/iot/latest/apireference/)
- [AWS IoT Developer Guide — Thing Management](https://docs.aws.amazon.com/iot/latest/developerguide/iot-thing-management.html)
- [AWS IoT Developer Guide — Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)
- [AWS IoT Fleet Indexing](https://docs.aws.amazon.com/iot/latest/developerguide/iot-indexing.html)
- [AWS IoT Pricing](https://aws.amazon.com/iot-device-management/pricing/)

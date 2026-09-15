# EASN Cloud — Codex Handoff

Last updated: 2026-09-15

## Purpose of this document

This document transfers the durable project context from the original Codex
task on João's macOS host to a new Codex task running on the Ubuntu machine that
will host the EASN cloud pilot.

The receiving task must read this document before proposing requirements,
architecture, infrastructure, or implementation changes. It must continue from
the current decisions instead of restarting the project from an empty brief.

## Project objective

Design, specify, implement, and operate a pilot cloud platform for the
Ecoacoustic Sensing Node (EASN). The first phase is requirements and system
specification. Implementation follows only after the relevant requirements and
interfaces are sufficiently defined.

The pilot will run on a repurposed Dell laptop with an Intel i7 and a 256 GB
NVMe SSD. Ubuntu was reinstalled from scratch after the previous encrypted
installation was deliberately erased. The laptop is intended to become the
local laboratory server.

## Terminology and system boundary

The following names must remain distinct:

- **EASNFW-SENSOR**: firmware running on the nRF5340 and responsible for sensor
  acquisition, processing, local storage, and record assembly.
- **EASNFW-CLOUD**: firmware running on the nRF9151 and responsible for LTE-M
  connectivity and communication with the server.
- **EASN Cloud Backend**: the Linux server software that receives, validates,
  persists, processes, and exposes EASN data.
- **EASN Cloud UI**: the researcher-facing web interface backed by the EASN
  Cloud Backend.

The word "cloud" must not be used ambiguously in interfaces or documentation
when it could refer to both the nRF9151 image and the Linux backend.

## Existing related project

The firmware repository is `hslourenc/easnfw`. Its documentation is built with
Sphinx and published with GitHub Pages. The documentation is organized around:

- Introduction;
- Requirements;
- Verification;
- Architecture;
- Design; and
- Cloud Backend context.

The EASN Cloud specification should follow the same documentation philosophy:
version-controlled source, numbered and testable requirements, explicit
architecture and design chapters, verification traceability, diagrams, and an
automatically published documentation site.

Relevant firmware documentation currently includes these cross-system
requirements:

- globally unique `record_id` per acquisition;
- canonical record assembly on EASNFW-SENSOR;
- versioned formats and processing-algorithm identifiers;
- fragmented transfer with integrity checking between the nRF5340 and nRF9151;
- idempotent delivery to the backend;
- cloud acknowledgement only after durable storage;
- local retention/removal only after durable cloud acknowledgement;
- bounded buffers and explicit backpressure or loss policies; and
- explicit timestamp validity and time-source metadata.

The cloud requirements must be compatible with these contracts. Any conflict
must be recorded and resolved across both specifications rather than silently
worked around in the backend.

## Confirmed product scope

The cloud pilot needs at least three researcher-facing working areas.

### Capture view

Provide visibility into acquired records and node activity, including record
identity, node identity, acquisition time and time validity, environmental
metadata, upload/delivery state, validation state, processing state, and access
to associated audio or derived artifacts.

### Real-time processing view

Run and display a simple, transparent baseline pipeline over incoming data.
The initial example is the computation of conventional acoustic indices in
near-real time. The precise indices, windowing, latency target, and meaning of
"real time" remain to be specified.

### Machine-learning testbench

Allow authorized researchers to select captured datasets and either:

- train an experimental model; or
- run a previously trained model over selected data.

The testbench must preserve dataset provenance, configuration, code/model
version, parameters, outputs, and execution status. It is initially a research
facility, not an unrestricted multi-tenant model-hosting platform.

## Initial logical architecture

The current baseline is:

```text
EASN nodes
    |
    | HTTPS over LTE-M
    v
Public ingress boundary
    |
    v
Ingestion API
    |-- schema/authentication/integrity validation
    |-- idempotent record commit
    |-- durable acknowledgement
    |
    +--> relational metadata database
    +--> binary/object storage
    +--> background processing queue/workers
                         |
                         +--> acoustic indices
                         +--> validation and quality checks
                         +--> model inference/training jobs
                                      |
                                      v
                              researcher API and UI
```

The minimum logical services identified so far are:

- HTTPS ingestion API;
- relational metadata database;
- storage for audio, spectrograms, datasets, models, and derived artifacts;
- background workers;
- researcher API and dashboard;
- monitoring;
- backup and restore; and
- administrative access.

This is a logical decomposition, not yet a commitment to one process or
container per item.

## Data and delivery decisions

The current candidate protocol uses CBOR over HTTPS for structured node uploads
and binary objects for audio or large artifacts. JSON may be used for human-facing
diagnostic and UI APIs. The final schemas and endpoints are not yet defined.

Delivery semantics must satisfy all of the following:

1. Every upload identifies its `record_id`, node, schema version, and integrity
   information.
2. Repeating an upload for an already committed `record_id` must not duplicate
   the record or its artifacts.
3. Receiving a fragment or returning a generic HTTP success is not sufficient
   for node-side deletion.
4. A durable commit acknowledgement is returned only after the configured
   durability boundary has been reached.
5. Incomplete, corrupt, oversized, unauthenticated, or unsupported payloads are
   rejected without being treated as committed records.
6. Processing failures do not invalidate a successfully preserved raw record;
   ingestion and scientific processing have separate states.

Structured metadata and large binary artifacts should remain separate. The
current baseline is PostgreSQL for metadata and either S3-compatible object
storage or a managed filesystem with equivalent integrity and lifecycle
controls for large objects. TimescaleDB and MinIO are candidates, not confirmed
requirements.

## Deployment and access decisions

The pilot backend will be hosted on the Ubuntu laptop. Services should be
reproducibly deployed in containers, while persistent data must live on
explicitly managed host volumes outside disposable containers.

Administrative access and device ingestion are different trust boundaries:

- João and Henrique should access administration and SSH through a private
  network such as Tailscale or WireGuard.
- Each researcher must have an individual account or identity.
- SSH private keys and passwords must not be shared.
- Password-based SSH and direct public exposure of port 22 should be disabled
  after key/VPN access is verified.
- EASN devices still require an Internet-reachable HTTPS ingestion endpoint.

The laboratory network may be behind NAT or carrier-grade NAT. Therefore, the
public ingress design remains open. A public relay or authenticated reverse
tunnel may be required. If a relay acknowledges uploads while the laboratory
server is offline, its durable queue becomes part of the formal durability
boundary and must preserve `record_id` idempotency.

No credentials, private keys, access tokens, or production secrets may be
committed to Git.

## Storage, reliability, and operations

The laptop has limited local storage and is a single point of failure. The
pilot therefore needs explicit capacity, retention, backup, and restoration
requirements before continuous acquisition is enabled.

At minimum, the design must account for:

- primary server storage;
- a scheduled backup on a separate device;
- a periodic off-site copy of scientific data and database backups;
- integrity checks for stored objects;
- a documented and tested restoration procedure;
- monitoring of disk capacity and database health;
- monitoring of backup age and failed jobs;
- monitoring of failed ingestion and certificate expiration; and
- last-contact and delivery status for every deployed node.

RAID or mirrored storage, if later added, improves availability but is not a
backup.

## Current status

- Ubuntu installation: complete.
- Previous encrypted disk contents: intentionally erased.
- Firmware repository and Sphinx documentation: existing upstream project.
- Cloud repository directory: `easncloud/`.
- Cloud requirements: not yet formally numbered.
- Cloud implementation: not started.
- Public ingress mechanism: undecided.
- Node authentication mechanism: undecided.
- Command/configuration/FOTA channel: undecided.
- Raw audio and derived-data retention: undecided.
- Backup target and off-site destination: undecided.
- Exact machine resources, thermal behavior, and sustained workload capacity:
  still to be characterized.

## Recommended specification structure

The documentation site should initially contain:

```text
EASN Cloud
|-- Introduction
|   |-- Purpose
|   |-- Scope
|   |-- Users and roles
|   `-- Terminology
|-- Requirements
|   |-- Ingestion and delivery
|   |-- Data model and storage
|   |-- Capture view
|   |-- Real-time processing
|   |-- ML testbench
|   |-- Security and access
|   `-- Reliability and operations
|-- Verification
|-- Architecture
|-- Data and API contracts
|-- Design
`-- Deployment and operations
```

## Open decisions to resolve incrementally

Do not resolve all of these by assumption. Address them part by part with João:

1. Exact scope and actors for the first pilot.
2. What an EASN record contains and how fragments are assembled.
3. The durability boundary for `CLOUD_COMMIT_ACK`.
4. Node identity, provisioning, authentication, and revocation.
5. Public ingress topology for the actual laboratory network.
6. Capture-view filters, detail fields, playback, and download behavior.
7. Which acoustic indices form the vanilla baseline.
8. Latency and scheduling expectations for near-real-time processing.
9. Dataset selection, annotation, versioning, and train/test split semantics.
10. Model registry, supported execution types, provenance, and reproducibility.
11. Storage capacity, quotas, retention, and deletion policy.
12. Backup destinations, frequency, restoration targets, and acceptable loss.
13. Researcher authorization roles and audit requirements.
14. Observability, alerts, and degraded-operation behavior.
15. Hardware capacity and whether training must later move to another machine.

## Instructions for the receiving Codex task

1. Read this file and the relevant `easnfw` Sphinx source before editing cloud
   requirements.
2. Preserve the terminology and confirmed decisions above.
3. Work with João incrementally. Start with scope and actors, then write small
   groups of numbered, testable requirements.
4. Separate requirements from architecture and implementation choices.
5. Give every requirement a stable identifier and a corresponding verification
   method or placeholder.
6. Record unknowns as open decisions; do not present candidates as decisions.
7. Keep the documentation buildable and version controlled after each part.
8. Do not implement the backend until João explicitly moves the project from
   specification to implementation.

Suggested first prompt on the Ubuntu host:

> Read `docs/handoff.md` completely and inspect the referenced EASNFW Sphinx
> documentation. Continue the EASN Cloud specification from the first open
> step: define the pilot scope, users, roles, and system boundary. Do not start
> backend implementation yet, and do not silently resolve open decisions.

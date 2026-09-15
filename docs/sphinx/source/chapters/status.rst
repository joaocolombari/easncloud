Continuation status and decisions
=========================================

Increment 1 — 2026-09-15
--------------------------------

Resumed from the first open handoff step: pilot scope, users, roles, and system
boundary. No project restart or backend implementation was performed.

**D-001 (confirmed by user in this task):** Initial actors are João and Henrique
as administrators plus individually authorized researchers using capture,
acoustic processing, and ML workflows. Additional actors may be added later.
Detailed permission assignments are not implied by this confirmation.

The introduction records this scope and boundary; six draft requirements
formalize inherited capabilities with corresponding verification plans.
The original handoff is preserved unchanged as the historical baseline.
Its statement that requirements are not yet numbered is superseded by this
increment, not by an edit to the historical file.

Source inventory
------------------------

* Cloud source: ``docs/handoff.md`` at initial cloud commit ``dc20982``.
  The requested name ``docs/codex_handoff.md`` was absent; the existing file
  has the title "EASN Cloud — Codex Handoff" and supplies the continuation step.
* Firmware source: local ``easnfw`` commit
  ``d59b65b7d98c9182a35e7b3c159a8c9b6328a61f``.
* Reviewed firmware ``README.md``; all six Sphinx chapters (introduction,
  requirements, verification, architecture, design, cloud_backend);
  ``diagrams/sys_arch.puml``; Sphinx index/configuration; documentation
  requirements; and ``.github/workflows/docs.yml``.
* Firmware requirements and verification cases are context, not claims of
  implemented or verified firmware behavior. No firmware files were edited.

Open decision register
------------------------------

IDs below retain the order of the original handoff's fifteen open decisions.
All remain open except the actor portion of OD-01, resolved by D-001.
No candidate technology has been promoted to a decision.

.. list-table:: Decisions to resolve incrementally with João
   :header-rows: 1
   :widths: 12 48 40

   * - ID
     - Topic
     - Remaining decision
   * - OD-01
     - Exact first-pilot scope and actors
     - Actors confirmed; scale, acceptance scenario, and firmware-maintenance coverage remain open.
   * - OD-02
     - Canonical record and fragments
     - Uploaded content, required artifacts, message classes, assembly, and timestamp validity.
   * - OD-03
     - Durable acknowledgement
     - Durability boundary for CLOUD_COMMIT_ACK, including any relay storage.
   * - OD-04
     - Node identity and trust
     - Provisioning, authentication, and revocation; command/configuration/FOTA trust remains unresolved.
   * - OD-05
     - Public ingress
     - Actual laboratory network topology, relay/tunnel choice, and endpoint exposure.
   * - OD-06
     - Capture view
     - Filters, detail fields, playback, and download behavior.
   * - OD-07
     - Acoustic baseline
     - Index selection, input compatibility, algorithm versions, and windowing.
   * - OD-08
     - Near-real-time processing
     - Latency definition, target, scheduling, and load assumptions.
   * - OD-09
     - Datasets
     - Selection, annotation, versioning, and train/test split semantics.
   * - OD-10
     - ML execution
     - Registry, execution types, provenance, and reproducibility.
   * - OD-11
     - Storage lifecycle
     - Capacity, quotas, raw/derived retention, and deletion policy.
   * - OD-12
     - Backup and restore
     - Destinations, frequency, restoration targets, and acceptable loss.
   * - OD-13
     - Researcher authorization
     - Detailed permissions and audit requirements for the confirmed actors.
   * - OD-14
     - Operations visibility
     - Observability, alerts, and degraded-operation behavior.
   * - OD-15
     - Hardware capacity
     - Measured resources, thermal/sustained load limits, and training placement.

Next specification step
-------------------------------

Continue with OD-02: establish the record contents and uploaded artifacts,
including whether raw audio is delivered or remains only on the node. Use
CI-01 through CI-04 in :doc:`contracts` to preserve firmware dependencies.
Then specify OD-03's durable acknowledgement with a small group of numbered
ingestion requirements and matching failure/retry verification cases.
Do not select these contracts by assumption or start backend implementation.

Publication status
--------------------------

Sphinx source and a build/Pages workflow are supplied. Publication requires
pushing the changes and configuring GitHub Pages for Actions; neither has
been performed in this task.

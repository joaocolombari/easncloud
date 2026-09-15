Introduction
====================

Purpose and scope
-------------------------

EASN Cloud receives, preserves, processes, and exposes ecoacoustic data for a
laboratory research pilot. The confirmed product scope includes a capture
view, near-real-time baseline acoustic processing, and an ML testbench for
training experimental models and running existing models on selected data.
The first specification increment defines responsibility boundaries and
formalizes only a small group of capabilities already stated in the handoff.

The initial actors were confirmed by the user on 2026-09-15: João and Henrique
as administrators, plus individually authorized researchers using capture,
acoustic processing, and ML workflows. Additional actors can be added later.
Deployment scale, acceptance dataset, and detailed role permissions remain
open under OD-01 and OD-13. No node count, user count,
latency target, or capacity guarantee is assumed.

Users and actors
------------------------

These confirmed actors have responsibility descriptions, not a detailed
authorization matrix.
One person may carry multiple responsibilities; permissions remain undecided.

.. list-table:: Initial actor inventory
   :header-rows: 1
   :widths: 23 47 30

   * - Actor
     - Responsibility in the existing scope
     - Decision status
   * - Authorized researcher
     - Inspect capture data and run authorized baseline, training, and inference workflows.
     - Confirmed actor; exact permissions open (OD-13).
   * - Administrator
     - Administer the pilot server through individual private-network/SSH access.
     - João and Henrique confirmed; detailed permission mapping open.
   * - EASNFW-CLOUD
     - Initiate HTTPS uploads over LTE-M and relay backend delivery outcomes to EASNFW-SENSOR.
     - External machine actor; node authentication open (OD-04).
   * - EASNFW-SENSOR
     - Acquire data, assemble and persist canonical records, and enforce local retention.
     - External upstream component, reached through EASNFW-CLOUD.

A separate field-operator, annotator, read-only user, or model-maintainer role
has not been approved. Dataset annotation itself remains open (OD-09).

Terminology and system boundary
---------------------------------------

**EASNFW-SENSOR** is the nRF5340 firmware. **EASNFW-CLOUD** is the nRF9151
firmware. **EASN Cloud Backend** is the Linux server software. **EASN Cloud
UI** is the researcher-facing web interface. These names are not interchangeable.

The product boundary contains ingestion, validation, persistent metadata and
binary storage, processing workers, researcher API/UI, monitoring, backup,
and restore functions. A potential public relay is a deployment decision;
if it issues durable acknowledgements, its storage enters the durability
boundary. That boundary is not yet selected.

Sensor acquisition, canonical on-node record assembly, inter-component
fragment transfer, local node retention, and firmware installation/rollback
belong to EASNFW. The backend must honor their cross-system contracts.
Defining backend support for firmware distribution remains open; firmware
REQ-019 and REQ-020 already require update and rollback behavior.

An unrestricted multi-tenant model-hosting platform is outside the initial
research-facility scope. Implementation begins only when João explicitly
moves the project beyond specification.

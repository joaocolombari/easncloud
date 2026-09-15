Requirements
====================

Status and identifiers
------------------------------

The requirements below are **draft formalizations of confirmed handoff scope**.
They introduce no new role assignments or technology selections. Stable IDs
use the EASNC-REQ prefix to avoid collision with firmware REQ identifiers.
Verification plans are in :doc:`verification`; no runtime tests have run.
Detailed ingestion, storage, security, reliability, and scientific processing
requirements will be added incrementally after their open decisions are resolved.

.. _easnc-req-001:

EASNC-REQ-001: Capture visibility
-----------------------------------------

The EASN Cloud UI shall allow an authorized researcher to inspect captured
records with record identity, node identity, acquisition time and time validity,
environmental metadata, delivery state, validation state, processing state,
and access to associated audio or derived artifacts when present.

Source: handoff, Capture view. Verification: EASNC-VER-001.
Filters, playback/download behavior, and unavailable-data presentation remain OD-06.

.. _easnc-req-002:

EASNC-REQ-002: Baseline processing
------------------------------------------

EASN Cloud shall run a baseline acoustic-index pipeline over incoming data
and display its results to authorized researchers.

Source: handoff, Real-time processing view. Verification: EASNC-VER-002.
Indices, eligible input representation, windowing, and latency acceptance
criteria remain OD-02, OD-07, and OD-08; this requirement does not define a
real-time service-level target.

.. _easnc-req-003:

EASNC-REQ-003: Experimental training
--------------------------------------------

The EASN Cloud testbench shall allow an authorized researcher to select a
captured dataset and execute experimental model training on that dataset.

Source: handoff, Machine-learning testbench. Verification: EASNC-VER-003.
Supported execution types and resource limits remain OD-09, OD-10, and OD-15.

.. _easnc-req-004:

EASNC-REQ-004: Model inference
--------------------------------------

The EASN Cloud testbench shall allow an authorized researcher to select a
captured dataset and run a previously trained model over that dataset.

Source: handoff, Machine-learning testbench. Verification: EASNC-VER-004.
Model compatibility and dataset selection semantics remain OD-09 and OD-10.

.. _easnc-req-005:

EASNC-REQ-005: Experiment provenance
--------------------------------------------

For each training or inference execution, EASN Cloud shall preserve dataset
provenance, configuration, code/model version, parameters, outputs, and
execution status.

Source: handoff, Machine-learning testbench. Verification: EASNC-VER-005.
Provenance schema, versioning semantics, and retention remain OD-09 to OD-11.

.. _easnc-req-006:

EASNC-REQ-006: Individual researcher identity
-----------------------------------------------------

EASN Cloud shall associate each researcher's access with an individual account
or identity.

Source: handoff, Deployment and access decisions. Verification: EASNC-VER-006.
Identity provider, permissions, and audit requirements remain OD-13.

Data and API contracts
==============================

Inherited constraints
-----------------------------

These constraints are carried forward for the next specification increment;
no endpoint, wire schema, or error code is selected here.

* EASNFW-SENSOR owns canonical record assembly and globally unique record IDs
  (firmware REQ-014). EASNFW-CLOUD adds a transport envelope.
* Record/schema, acquisition-configuration, and processing-algorithm versions
  preserve interpretation across firmware changes.
* Inter-component fragments are versioned and integrity-checked (REQ-015).
  Their framing must not be assumed to define HTTPS upload fragmentation.
* Retransmission is idempotent by ``record_id`` (REQ-016, TC-045).
* Link receipt or generic HTTP success does not authorize local deletion.
  ``CLOUD_COMMIT_ACK`` requires the selected durability boundary (REQ-010,
  TC-034). Its exact representation remains open.
* Incomplete, corrupt, oversized, unauthenticated, or unsupported data is not
  committed. Scientific processing failure does not invalidate preserved input.
* Buffer bounds and explicit backpressure/loss behavior must be respected.
* Timestamp validity and source must remain visible; see compatibility issue
  CI-02 before finalizing the schema.

CBOR over HTTPS is a candidate for structured uploads; JSON is a candidate for
diagnostic/UI APIs. Metadata and large binaries remain separate.

Compatibility issues to resolve jointly
-----------------------------------------------

**CI-01 — Firmware maintenance scope.** Firmware REQ-019/020 and TC-049 through
TC-054 require update discovery, compatible image delivery, signature checks,
and rollback. The handoff leaves the command/configuration/FOTA channel open.
This is an unresolved backend dependency, not permission to remove firmware
requirements or silently add a release service. Resolve under OD-01 and OD-04/05; model execution under OD-10 remains a
separate concern.

**CI-02 — Unsynchronized timestamps.** Firmware REQ-018 requires synchronized
ISO 8601 timestamps and a synchronization source, while TC-048 also expects
unsynchronized acquisition with monotonic timing and an invalid/unsynchronized
flag. The handoff explicitly retains time-validity metadata. Agree the
unsynchronized-record acceptance contract under OD-02; do not silently reject
these records or invent valid wall-clock times.

**CI-03 — Scientific input availability.** The canonical firmware record
contains processed audio and may reference locally stored raw audio. This does
not guarantee that raw audio reaches the backend. Baseline indices, playback,
and ML input compatibility depend on the agreed uploaded artifacts (OD-02,
OD-06, OD-07, OD-10).

**CI-04 — Additional device messages.** Firmware REQ-001/002 and REQ-011 expect
connectivity self-test, power-on logs, and record-error reporting. Define these
message classes with OD-02 without treating every message as an acquisition
record or assuming that every message has a scientific ``record_id``.

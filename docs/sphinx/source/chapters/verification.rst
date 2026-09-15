Verification
====================

All entries below are planned acceptance checks, not executed results.
Documentation compilation checks source structure only. It does not demonstrate
implemented product behavior. Fixtures and detailed procedures will be defined
when the listed open decisions are closed.

.. list-table:: Initial traceability matrix
   :header-rows: 1
   :widths: 15 17 48 20

   * - Verification ID
     - Requirement
     - Method and expected result
     - Prerequisites
   * - EASNC-VER-001
     - :ref:`easnc-req-001`
     - Demonstration: inspect representative records with valid/invalid time, different delivery/validation/processing states, and present/absent artifacts; every specified field matches the fixture and present artifacts are accessible.
     - OD-02, OD-06, OD-13; UI implementation.
   * - EASNC-VER-002
     - :ref:`easnc-req-002`
     - Test: submit an eligible reference input; compare displayed baseline index values with independently calculated expected values using the agreed tolerance.
     - OD-02, OD-07, OD-08; pipeline implementation.
   * - EASNC-VER-003
     - :ref:`easnc-req-003`
     - Demonstration: select a known captured dataset and execute an approved training configuration; a resulting model is produced from that selection.
     - OD-09, OD-10, OD-13, OD-15; testbench implementation.
   * - EASNC-VER-004
     - :ref:`easnc-req-004`
     - Test: select a reference dataset and compatible trained model; inference produces outputs corresponding to the selected inputs.
     - OD-09, OD-10, OD-13; testbench implementation.
   * - EASNC-VER-005
     - :ref:`easnc-req-005`
     - Inspection/test: retrieve successful and failed training/inference executions; retained provenance, versions, configuration, parameters, status, and any produced outputs match submitted inputs and observed outcomes.
     - OD-09, OD-10, OD-11; provenance implementation.
   * - EASNC-VER-006
     - :ref:`easnc-req-006`
     - Test: two researchers authenticate separately; each access resolves to that researcher's distinct identity.
     - OD-13; identity implementation.

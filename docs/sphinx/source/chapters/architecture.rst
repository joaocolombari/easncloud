Architecture
====================

Inherited logical baseline
----------------------------------

This reproduces the handoff's decomposition; it does not allocate containers
or select new technologies.

.. code-block:: text

   EASNFW-SENSOR -- inter-component transfer --> EASNFW-CLOUD
                                                     |
                                               HTTPS / LTE-M
                                                     v
                                             Public ingress
                                                     |
                                                     v
                                             Ingestion API
                                        validation / commit / ACK
                                            /        |        \
                                   metadata DB   binary store   workers
                                                                  |
                                                 indices / quality / ML
                                                                  |
                                                    researcher API + UI

Monitoring, backup/restore, and administrative access support these services.
Administrative access uses a private network with individual identities;
device ingestion needs an Internet-reachable HTTPS endpoint. UI exposure
and detailed researcher access policy remain open (OD-05 and OD-13).

The inherited metadata baseline is PostgreSQL; binary storage is either
S3-compatible storage or a managed filesystem with equivalent integrity and
lifecycle controls. TimescaleDB and MinIO remain candidates. Reproducible
containers and explicitly managed persistent host volumes are inherited
deployment decisions. The pilot host is the Ubuntu Dell i7 laptop with a
256 GB NVMe SSD; usable capacity and sustained performance are not measured.

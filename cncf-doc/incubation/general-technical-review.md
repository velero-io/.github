# General Technical Review questions
v1.0.1
## Introduction

The General Technical Review questions can be completed by a project in lieu of a presentation to a Technical Advisory Group (TAG) as well as to satisfy the Engineering Principle requirements of a Sandbox application or Due Diligence for moving levels. 

The questions are designed to prompt **_design thinking_** for projects that would like to one day be a graduated project.

The intent is to understand how the project has adopted and aligned with the CNCF maturation levels as well as encourage good design & security best practices. 


<!-- 
The General Technical Review questions can be completed by a project team in lieu of a presentation to a Technical Advisory Group (TAG) as well as to satisfy several of the Engineering Principle requirements for applying to CNCF Sandbox as well as applying to move to Incubation and Graduation.

For the purposes of the general technical review and further domain reviews, the questions are designed to prompt design thinking for ready-for-production projects and architecture. The intent is to understand and validate the cloud native software lifecycle of the project and whether it is in line with the CNCF Maturation process and levels. 

Project maintainers are expected to have designed the project for cloud native use cases and workloads, as well as taking a ‘secure by design and secure by default’ approach that enables adopters and consumers of the project the ability to ‘loosen’ the defaults in a manner that suits their environment, requirements and risk tolerance.

These questions are to gather knowledge about the project. Project maintainers are expected to answer to the best of their ability. **_Not every question will be addressable by every project._**

**Suggestion:** A recorded demo or diagram(s) may be easier to convey some of the concepts in the questions below. The project maintainers may provide a link to a recorded demo or add architectural diagrams along with your GTR questionnaire.

-->

### General Technical Review questions 

The questions follow the cloud native software lifecycle day schemas:

**Day 0 - Planning Phase. (Sandbox)** - This phase covers design and architecture of the cloud native project.

**Day 1 - Installation and Deployment Phase (Incubation)** - This phase covers initial installation and deployment of the design developed during Day 0 - Planning Phase. 

**Day 2 - Day-to-Day Operations Phase (Graduated)** - This phase covers post-deployment operations in production-ready environments to include monitoring, maintenance, auditing and troubleshooting.


### How to use this template

Make a copy of the template below and answer questions related to your project level to the best of your ability.
_**Not every question will be addressable or relevant to every project.**_
If this is the case for your project, please mark it as not-applicable (N/A) and provide a brief explanation. 

**NOTE:** The questions are cumulative e.g. if you are applying for incubation or graduation, you should answer both day 0 and day 1 questions etc.

#### Tips

* Treat the GTR questionnaire as a living document and keep a copy of it in your project's own repo. The GTR questions are helpful to both contributors and users and will make updating it in the future less work when you want to apply to move levels.
* Answer more questions than the requirement for your level if it _makes sense for your project_. e.g. if you have documentation covering the different forms of observability in the Day-2 requirements.
* You **CAN** link out to your own project's documentation, but be sure to link to it in a _versioned_ form. e.g. link to it at a specific commit instead of the `main` branch, or versioned website.
* A recorded demo or diagram(s) may be easier to convey some of the concepts in the questions below. You may provide a link to a recorded demo or add architectural diagrams along with your GTR questionnaire.
* If you are unsure or have a question about any section below, **please ask**. Chances are you're not the only one with a question and the template should be updated with additional guidance.

---

# General Technical Review - Velero / Incubation

- **Project:** Velero
- **Project Version:** v1.18
- **Website:** https://velero.io
- **Date Updated:** 2026-10-29
- **Template Version:** v1.0.1
- **Description:** Velero gives Kubernetes users tools to back up and restore cluster resources and persistent volume data. It is used for disaster recovery, cluster migration, and replication to development and test environments, across public cloud and on-premises.

> **Status: DRAFT for maintainer review.** Verified facts come from the `velero-io/velero` source tree and docs as of 2026-10-29. Items marked **TODO(team)** need a maintainer to confirm exact numbers, current tooling, or to publish a linked artifact before this is submitted to `cncf/toc` under `projects/velero/tech-review/YYYY-MM-DD.md`.

## Day 0 - Planning Phase

### Scope

> Describe the project's vision and goals.

Give every Kubernetes adopter a reliable, cloud-agnostic way to protect and move their cluster state and volume data. Velero aims to be the standard open, pluggable backup and restore engine that other tools and managed services can build on, rather than a single-vendor solution.

> Describe the primary use cases for the project. Describe any additional use cases the project supports. Describe any use cases specifically out of scope of the project.

Primary use cases:
- Disaster recovery: back up cluster resources and persistent volumes and restore them after loss or corruption.
- Migration: move workloads and their data between clusters (across versions, providers, or regions).
- Replication: copy a production cluster into development and test environments.
- Scheduled backups with retention (TTL) for ongoing protection.

Additional use cases: selective backup/restore by namespace, label, or resource; volume data movement across storage backends using the built-in data mover; pre and post hooks for application-consistent backups (for example quiescing a database).

Out of scope: Velero is not a continuous/synchronous replication or high-availability system, not an application-aware backup tool by itself (application consistency is achieved through hooks the adopter configures), and not a long-term archival or cataloging product. It does not guarantee crash-consistency for arbitrary stateful workloads without hooks.

> Describe the roadmap process, how scope is determined for mid to long term features, as well as how the roadmap maps back to current contributions and maintainer ladder?

Larger features begin as a design proposal in the [`design/`](https://github.com/velero-io/velero/tree/main/design) directory following [`design/_template.md`](https://github.com/velero-io/velero/blob/main/design/_template.md), are discussed on the pull request and in the bi-weekly community meetings, and must be accepted before implementation. Feature requests and bugs are tracked as GitHub issues. Maintainers review priorities each release cycle. The current and archived roadmaps live in the project wiki, and [`ROADMAP.md`](https://github.com/velero-io/velero/blob/main/ROADMAP.md) documents the process. This maps back to the maintainer ladder because accepted designs are driven and reviewed by maintainers, and sustained quality contribution (including design authorship) is part of the path described in [`GOVERNANCE.md`](https://github.com/velero-io/.github/blob/main/GOVERNANCE.md).

> Describe the target persona or user(s) for the project?

Platform engineers, SREs, and cluster administrators who operate Kubernetes; and ISVs/managed-service providers who build backup or DR products on top of Velero.

> Describe the intended types of organizations who would benefit from adopting this project.

Any organization running Kubernetes in production: financial services, SaaS and software vendors, cloud providers offering managed backup, and platform engineering teams providing internal Kubernetes platforms. See [`ADOPTERS.md`](https://github.com/velero-io/velero/blob/main/ADOPTERS.md).

> Please describe any completed end user research and link to any reports.

Adoption is captured in [`ADOPTERS.md`](https://github.com/velero-io/velero/blob/main/ADOPTERS.md) and in the CNCF DevStats for Velero (https://velero.devstats.cncf.io). Adopter interviews for the incubation application are being gathered through the CNCF adopter interview process.

**TODO(team):** link any CNCF end-user survey data that mentions Velero, and the adopter interview outcomes once available.

### Usability

> How should the target personas interact with your project?

Through the `velero` command-line client and through Kubernetes custom resources (`Backup`, `Restore`, `Schedule`, and others). Operators can drive everything declaratively with CRDs and GitOps, or imperatively with the CLI. The UX is intentionally kubectl-like.

> Describe the user experience (UX) and user interface (UI) of the project.

Velero has no graphical UI of its own; it is a CLI plus a set of CRDs and a server (controllers) running in the cluster. Several adopters and vendors build GUIs on top of the CRDs. Backup and restore progress and logs are surfaced through CLI commands and stored in object storage.

> Describe how this project integrates with other projects in a production environment.

Velero integrates with object storage (S3, Azure Blob, GCS, and S3-compatible systems) through `BackupStorageLocation`s, with CSI drivers and the Kubernetes volume snapshot machinery for snapshot-based backups, and with cloud provider APIs through plugins. It coexists with GitOps tools, monitoring stacks (Prometheus/Grafana), and CSI storage.

### Design

> Explain the design principles and best practices the project is following.

Kubernetes-native: everything is a CRD reconciled by controllers. Pluggable: object store, volume snapshotter, and backup/restore item actions are all plugin interfaces so providers extend Velero without changing core. Cloud-agnostic and declarative. Idempotent, controller-based reconciliation. Extensibility through plugins and through pre/post hooks.

> Outline or link to the project's architecture requirements? Describe how they differ for Proof of Concept, Development, Test and Production environments, as applicable.

Core components: the Velero server (a single-replica `Deployment` of controllers), an optional node-agent (a `DaemonSet`) for file-system backup and data movement, and provider plugins (run as init containers that copy the plugin binary into the server pod). External requirement: an object storage bucket. Optional: a CSI driver plus the external-snapshotter and a `VolumeSnapshotClass` for snapshot-based volume backup. See [`ARCHITECTURE.md`](https://github.com/velero-io/velero/blob/main/ARCHITECTURE.md).
- Proof of Concept: single cluster with MinIO as S3-compatible storage; minimal config.
- Development/Test: object storage plus CSI snapshots; relaxed resource limits.
- Production: durable, versioned, encrypted object storage; scoped RBAC and cloud IAM; resource requests/limits; monitoring and alerting; schedules with retention.

> Define any specific service dependencies the project relies on in the cluster.

The Kubernetes API server (required). Optionally the CSI driver, external-snapshotter, and snapshot controller for CSI snapshots. The node-agent is required only for file-system backup and the data mover. Object storage is an external dependency, not an in-cluster service.

> Describe how the project implements Identity and Access Management.

In-cluster, Velero uses a dedicated `ServiceAccount` with RBAC. `velero install` binds the server service account to `cluster-admin` by default so it can read and recreate arbitrary resources; the [RBAC docs](https://velero.io/docs/main/rbac/) describe how adopters scope this with their own service account. For cloud access (object storage, snapshots) Velero uses cloud IAM through secrets or workload identity (AWS IRSA, GCP Workload Identity, Azure Managed Identity / Workload Identity). The node-agent runs as UID 0 but is **not** privileged by default; privileged mode is opt-in (`--privileged-node-agent`) for file-system backup scenarios that require host access to pod volume data.

> Describe how the project has addressed sovereignty.

Velero stores backups in object storage buckets and regions the adopter chooses and controls. It does not transmit data to any Velero-operated service and supports air-gapped and on-premises deployments. Data residency is entirely under adopter control.

> Describe any compliance requirements addressed by the project.

Velero itself does not hold compliance certifications. It supports common compliance building blocks: encryption at rest via the storage provider, RBAC, and auditable backup/restore CRs. Adopters use Velero as part of meeting DR and data-protection requirements.

**TODO(team):** link any documented FIPS build guidance if the project wants to assert it.

> Describe the project's High Availability requirements.

The Velero server runs as a single active replica; on node or pod failure Kubernetes reschedules it, and in-progress operations resume or are retried on restart. Backups are stored durably off-cluster, so cluster-side availability does not affect already-stored backups. The node-agent is a `DaemonSet` with one pod per eligible node.

> Describe the project's resource requirements, including CPU, Network and Memory.

Modest and tunable. The server `Deployment` defaults to CPU request 500m / limit 1000m and memory request 128Mi / limit 512Mi, all overridable at install time (`pkg/install/resources.go`); the node-agent runs per node with its own configurable requests/limits. Network use is dominated by data transfer to object storage during backup/restore.

> Describe the project's storage requirements, including its use of ephemeral and/or persistent storage.

Backups and backup metadata are written to external object storage (persistent, adopter-owned). Processing uses ephemeral working space; the data mover temporarily provisions PVCs to stage volume data. No long-lived in-cluster persistent storage is required by the core server.

> Please outline the project's API Design.

- API topology and conventions: CRDs under the `velero.io` API group (`Backup`, `Restore`, `Schedule`, `BackupStorageLocation`, `VolumeSnapshotLocation`, `PodVolumeBackup`, `PodVolumeRestore`, `DataUpload`, `DataDownload`, `BackupRepository`, `DeleteBackupRequest`, `ServerStatusRequest`, and others). They follow Kubernetes API machinery conventions (spec/status, controllers, finalizers).
- Project defaults: sensible defaults let a basic backup run with a single `velero backup create` once a `BackupStorageLocation` and credentials exist.
- Additional configuration: schedules, TTL/retention, resource and namespace filters, volume policies, hooks, and per-BSL/VSL configuration.
- New or changed API types and cloud calls: enabling Velero adds the `velero.io` CRDs and results in calls to the configured object storage and, when snapshotting, to the CSI/cloud snapshot APIs.
- Compatibility with API servers: Velero targets the Kubernetes API server and publishes a supported-Kubernetes compatibility matrix (see README).
- Versioning and breaking changes: APIs are versioned (for example `v1` and `v2alpha1` for data-mover types) and follow a deprecation policy; breaking changes go through deprecation and are announced in release notes.

> Describe the project's release processes, including major, minor and patch releases.

Velero follows semantic versioning. Minor releases ship roughly every five to six months (v1.15.0 2024-11, v1.16.0 2025-04, v1.17.0 2025-09, v1.18.0 2026-03), with patch releases as needed for fixes and CVEs. The release process is documented in the [release instructions](https://velero.io/docs/main/release-instructions/), and the Kubernetes compatibility matrix is in the README.

**TODO(team):** confirm whether the project wants to state a fixed minor-release cadence.

### Installation

> Describe how the project is installed and initialized.

Two common paths: the `velero install` CLI command, or the community Helm chart. Installation applies the CRDs, deploys the server (and optionally the node-agent), and configures one or more `BackupStorageLocation`s plus credentials (secret or workload identity). A minimal install is a few CLI flags; production installs add RBAC scoping, resource limits, schedules, and monitoring.

> How does an adopter test and validate the installation?

Run `velero version` to confirm client/server match, create a test backup (`velero backup create`), then restore it into a scratch namespace or cluster and verify resources and volume data. `velero backup describe` and `velero backup logs` validate success, and backup logs are also stored in object storage.

### Security

> Please provide a link to the project's cloud native security self assessment.

**TODO(team):** link the Security Self-Assessment once published (drafted in [`security-self-assessment.md`](./security-self-assessment.md)). Velero holds the OpenSSF Best Practices passing badge ([project 3811](https://www.bestpractices.dev/projects/3811)).

> How are you satisfying the tenets of cloud native security projects? How do you recommend users alter security defaults in order to "loosen" the security of the project?

- Security as a design concern: Velero uses Kubernetes RBAC, supports workload identity over long-lived credentials, and keeps data in adopter-controlled storage.
- Secure defaults: backups go to the adopter's storage; no data leaves the environment; TLS is used for object storage and the Kubernetes API; the server container drops all capabilities, disables privilege escalation, uses the `RuntimeDefault` seccomp profile, and runs as a non-root user.
- Loosening defaults: granting broader backup permissions, or enabling the node-agent (and optionally `--privileged-node-agent`) for file-system backup, is documented so adopters can make the trade-off. See [RBAC](https://velero.io/docs/main/rbac/) and [file system backup](https://velero.io/docs/main/file-system-backup/).

> Please describe the frameworks, practices and procedures the project uses to maintain the basic health and security of the project. Describe how the project has evaluated which features will be a security risk to users if they are not maintained by the project.

Code review with `CODEOWNERS` and required approvals, branch protection, CI (unit tests, lint, codespell, KinD-based e2e), weekly Dependabot updates (gomod and github-actions), a nightly Trivy image scan feeding the GitHub Security tab, OpenSSF Scorecard, release checksums, and a documented vulnerability disclosure process ([`SECURITY.md`](https://github.com/velero-io/.github/blob/main/SECURITY.md), cncf-velero-security@lists.cncf.io). Features that expand privilege (file-system backup via the node-agent, or broad backup RBAC) are documented with their trade-offs. Release signing (cosign) and SBOM publication are tracked improvements not yet in place.

> Explain the least minimal privileges required by the project and reasons for additional privileges. Describe how the project is handling certificate rotation. Describe how the project follows secure software supply chain best practices.

- Least privilege: the server needs broad read to back up arbitrary resources and broad write to restore them (default `cluster-admin`, scopeable per [RBAC](https://velero.io/docs/main/rbac/)); the node-agent needs host access to read pod volumes for file-system backup.
- Certificates: Velero relies on Kubernetes-managed certificates for API access and on object storage TLS; it supports custom CA bundles for object storage (`--cacert` / `CACertRef`). See [self-signed certificates](https://velero.io/docs/main/self-signed-certificates/).
- Supply chain: release checksums, weekly Dependabot updates, a nightly Trivy scan, a GitHub Actions release pipeline, and OpenSSF Scorecard monitoring. Release signing (cosign), SBOM, and provenance are planned supply-chain improvements (build scripts currently set `--sbom=false --provenance=false`). A CodeQL/SAST workflow is proposed in velero-io/velero#10695.

## Day 1 - Installation and Deployment Phase

### Project Installation and Configuration

> Describe what project installation and configuration look like.

Installation is through `velero install` or the Helm chart, which deploy the CRDs, the server, and optionally the node-agent, and wire up `BackupStorageLocation`s and credentials. Configuration is declarative (CRDs and flags): storage locations, snapshot locations, schedules, retention, resource/namespace filters, volume policies, and hooks.

### Project Enablement and Rollback

> How can this project be enabled or disabled in a live cluster? Please describe any downtime required of the control plane or nodes.

`velero install` to enable and `velero uninstall` to remove. No control-plane or node downtime is required, and installing Velero does not change the behavior of existing workloads.

> Describe how enabling the project changes any default behavior of the cluster or running workloads.

Adding Velero does not alter running workloads; it adds controllers that act only on Velero CRs. Backups are read operations; restores create only the resources the adopter requests.

> Describe how the project tests enablement and disablement.

Covered by the KinD-based e2e suite (`.github/workflows/e2e-test-kind.yaml`) and install/uninstall paths in CI.

> How does the project clean up any resources created, including CRDs?

`velero uninstall` removes the deployment, node-agent, RBAC, and (optionally) the CRDs and the `velero` namespace. Backups in object storage are intentionally retained unless explicitly deleted.

### Rollout, Upgrade and Rollback Planning

> How does the project intend to provide and maintain compatibility with infrastructure and orchestration management tools like Kubernetes and with what frequency?

Velero publishes a supported-Kubernetes matrix and tracks new Kubernetes releases; compatibility is maintained release to release.

> Describe how the project handles rollback procedures.

Upgrades are applied via the CLI or Helm (new images plus CRD updates). Downgrades are supported within the documented compatibility range; CRD schema changes are handled through versioned APIs and conversion.

> How can a rollout or rollback fail? Describe any impact to already running workloads.

A failed upgrade typically leaves existing backups intact because they live in object storage; the main risk is CRD/schema mismatch, mitigated by the upgrade docs. Already-stored backups are not affected by a control-plane rollback, and running workloads are unaffected because Velero acts only on its own CRs.

> Describe any specific metrics that should inform a rollback.

Backup/restore failure rate and controller errors (see Observability).

> Explain how upgrades and rollbacks were tested and how the upgrade->downgrade->upgrade path was tested.

**TODO(team):** describe how the upgrade->downgrade->upgrade path is exercised in CI/e2e, or state the manual upgrade-testing process used per release.

> Explain how the project informs users of deprecations and removals of features and APIs.

Deprecations are announced in release notes and the changelog, with a deprecation period before removal.

> Explain how the project permits utilization of alpha and beta capabilities as part of a rollout.

New features are introduced behind alpha/beta API versions or flags before graduating to stable.

## Day 2 - Day-to-Day Operations Phase

### Scalability/Reliability

> Describe how the project increases the size or count of existing API objects.

Velero scales with the number of resources and the volume data in a backup; concurrency (for example parallel item backup and parallel data movement) is configurable.

**TODO(team):** quote the current concurrency settings/flags (for example data mover `--data-mover-prepare-timeout`, node-agent concurrency configuration).

> Describe how the project defines Service Level Objectives (SLOs) and Service Level Indicators (SLIs).

Velero does not impose SLOs; adopters typically define backup success rate and backup/restore completion time as SLOs, using the exposed metrics (below) as SLIs.

> Describe any operations that will increase in time covered by existing SLIs/SLOs.

Backup and restore duration grow with resource count and data volume; data-mover throughput depends on storage and network.

> Describe the increase in resource usage in any components as a result of enabling this project.

CPU/memory on the server and node-agent scale with backup size and concurrency; storage usage grows in the object store with retained backups; throughput is bounded by object storage and network.

> Describe which conditions enabling / using this project would result in resource exhaustion of some node resources.

Very large backups or high concurrency can pressure memory and node-agent resources; retention (TTL) policies prevent unbounded object-store growth.

> Describe the load testing that has been performed on the project and the results.

Load testing is performed via the velero-performance-testing project.

**TODO(team):** summarize the latest results (object counts, data sizes, durations) and link the report.

> Describe the recommended limits of users, requests, system resources, etc. and how they were obtained.

**TODO(team):** quote any documented guidance on maximum backup size, object counts, and concurrency, and how it was obtained.

> Describe which resilience pattern the project uses and how, including the circuit breaker pattern.

Controller-based retry/requeue on failure, idempotent reconciliation, and backups stored durably off-cluster so cluster failures do not lose completed backups. Velero does not implement a circuit-breaker pattern; failures surface in CR status and are retried.

### Observability Requirements

> Describe the signals the project is using or producing, including logs, metrics, profiles and traces.

Prometheus metrics exposed by the server and node-agent (for example `velero_backup_total`, `velero_backup_success_total`, `velero_backup_failure_total`, `velero_backup_partial_failure_total`, `velero_restore_total`, `velero_restore_success_total`, `velero_restore_failed_total`, `velero_volume_snapshot_success_total`, `velero_csi_snapshot_success_total`, `velero_data_upload_success_total`, `velero_data_download_success_total`), structured logs, and per-backup logs persisted to object storage.

> Describe how the project captures audit logging.

Backup and restore actions are recorded as CRs with status and are visible in Kubernetes audit logs; per-operation logs are stored in the backup.

> Describe any dashboards the project uses or implements as well as any dashboard requirements.

A Grafana dashboard based on Velero metrics is community-provided.

**TODO(team):** link the recommended Grafana dashboard.

> Describe how the project surfaces project resource requirements for adopters to monitor cloud and infrastructure costs, e.g. FinOps.

Object storage consumption and retention are the main cost drivers; metrics on backup counts and sizes help adopters monitor cost.

> Which parameters is the project covering to ensure the health of the application/service and its workloads? How can an operator determine if the project is in use? How can someone know it is working?

Backup/restore success and failure counts, durations, and controller errors indicate health. Presence of `Backup`/`Restore`/`Schedule` CRs and non-zero Velero metrics indicate active use. Successful scheduled backups, `velero backup describe`/`logs`, and a periodic restore test confirm it is working.

> Describe the SLOs and SLIs for this project.

Adopter-defined success-rate and completion-time SLOs; the metrics above are the SLIs.

### Dependencies

> Describe the specific running services the project depends on in the cluster.

The Kubernetes API server, external object storage, and (for snapshots) the CSI driver plus external-snapshotter/snapshot controller; the node-agent for file-system backup and data movement. File-system backup and the data mover use Kopia as the default uploader; Restic was the legacy uploader and is no longer a selectable uploader type (`--uploader-type` supports only `kopia`).

> Describe the project's dependency lifecycle policy.

Go modules with weekly Dependabot updates (gomod and github-actions) and review before merge.

> How does the project incorporate and consider source composition analysis as part of its development and security hygiene? Describe how this SCA is tracked.

A nightly Trivy image scan uploads SARIF results to the GitHub Security tab, and OpenSSF Scorecard tracks repo hygiene. SBOM generation is a planned addition.

> Describe how the project implements changes based on source composition analysis (SCA) and the timescale.

Vulnerabilities are triaged by the security process and addressed in patch releases; timescales follow severity.

**TODO(team):** state a documented remediation SLA if the project wants to commit to one.

### Troubleshooting

> How does this project recover if a key component or feature becomes unavailable?

If the Kubernetes API server or object storage is unavailable, in-progress backups/restores fail and are retried; completed backups in object storage are unaffected. If the node-agent is down, file-system backup/data movement is paused until it recovers. The server resumes work after restart.

> Describe the known failure modes.

Partial backups when some resources error, snapshot incompatibility or missing CSI/snapshot-class configuration, insufficient RBAC or cloud IAM permissions, object storage connectivity/credential errors, and volume data-movement failures. These surface in CR status and logs.

### Compliance

> What steps does the project take to ensure that all third-party code and components have correct and complete attribution and license notices?

Velero is Apache-2.0; dependencies are managed through Go modules with their licenses retained. License compliance for new dependencies is part of review.

**TODO(team):** confirm whether a NOTICE/licenses manifest ships with release artifacts; SBOM publication is planned.

> Describe how the project ensures alignment with CNCF recommendations for attribution notices.

- Third-party code in source files: retained inline with original notices.
- Unmodified third-party components in the repository: licenses retained in-repo.
- Build-time dependency notices in artifacts: **TODO(team):** confirm whether a NOTICE/licenses file ships in release binaries and images; SBOM publication is planned.

### Security

> How is the project executing access control?

Kubernetes RBAC for the Velero `ServiceAccount`, scoped cloud IAM for storage/snapshots, and `CODEOWNERS` plus branch protection for repository access.

> How does the project ensure its security reporting and response team is representative of its community diversity (organizational and individual)?

Vulnerability reports go to cncf-velero-security@lists.cncf.io and are handled by the Velero Security Team, whose members span multiple employers of the maintainer group.

**TODO(team):** confirm named membership once `SECURITY.md` lists it (the Security Response Roles section is proposed in velero-io/.github#5).

> How does the project invite and rotate security reporting team members?

**TODO(team):** document how security responders are added and rotated; this ties to the named Security Response Roles item open for the application.

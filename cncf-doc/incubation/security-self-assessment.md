# Velero Security Self-assessment

> **Status: DRAFT for maintainer review.** Facts below were taken from the `velero-io/velero` source tree and
> docs as of 2026-10-22. Items marked **TODO(team)** need maintainer input or confirmation before this
> document is submitted to `cncf/toc` under `projects/velero/security-assessment/self-assessment.md`.

This document follows the CNCF TAG Security and Compliance
[self-assessment guide](https://tag-security.cncf.io/community/assessments/guide/self-assessment/).

## Table of contents

* [Metadata](#metadata)
  * [Security links](#security-links)
* [Overview](#overview)
  * [Background](#background)
  * [Actors](#actors)
  * [Actions](#actions)
  * [Goals](#goals)
  * [Non-goals](#non-goals)
* [Self-assessment use](#self-assessment-use)
* [Security functions and features](#security-functions-and-features)
* [Project compliance](#project-compliance)
* [Secure development practices](#secure-development-practices)
* [Security issue resolution](#security-issue-resolution)
* [Appendix](#appendix)

## Metadata

|||
| -- | -- |
| Assessment Stage | Incomplete |
| Software | <https://github.com/velero-io/velero> |
| Security Provider | No. Velero's primary function is backup, restore and migration of Kubernetes resources and persistent volumes, not securing another system. |
| Languages | Go |
| SBOM | Not currently published as a release artifact. Direct and transitive dependencies are listed in [`go.mod`](https://github.com/velero-io/velero/blob/main/go.mod) / `go.sum` and visible in the GitHub dependency graph. **TODO(team):** decide whether to publish an SBOM with releases (see [Appendix](#appendix)). |

### Security links

| Doc | URL |
| -- | -- |
| Security policy (reporting, response, disclosure) | <https://github.com/velero-io/.github/blob/main/SECURITY.md> |
| Security insights manifest | <https://github.com/velero-io/velero/blob/main/SECURITY-INSIGHTS.yml> |
| Published security advisories | <https://github.com/velero-io/velero/security/advisories> |
| Governance | <https://github.com/velero-io/.github/blob/main/GOVERNANCE.md> |
| Maintainers | <https://github.com/velero-io/velero/blob/main/MAINTAINERS.md> |
| Restrictive RBAC configuration | <https://velero.io/docs/main/rbac/> |
| TLS / self-signed certificates for object storage | <https://velero.io/docs/main/self-signed-certificates/> |
| Backup storage and snapshot locations (credentials, encryption) | <https://velero.io/docs/main/locations/> |
| File system backup (incl. limitations) | <https://velero.io/docs/main/file-system-backup/> |
| CSI snapshot data movement (incl. limitations) | <https://velero.io/docs/main/csi-snapshot-data-movement/> |
| Support process and supported versions | <https://velero.io/docs/main/support-process/> |
| Architecture | <https://github.com/velero-io/velero/blob/main/ARCHITECTURE.md> |

## Overview

Velero backs up and restores Kubernetes cluster resources and persistent volumes, and migrates them between clusters,
on public cloud or on-premises.

### Background

Backups and restores are driven by Kubernetes custom resources (for example `Backup`, `Restore`, `Schedule`,
`BackupStorageLocation`) and reconciled by a server running in the cluster. Cluster resource definitions are
serialized and stored in an object store (the *backup storage location*). Persistent volume data is protected in
one of three ways: provider volume snapshots, the built-in file system backup (Kopia) that copies volume contents
to the object store, or the built-in data mover that uploads CSI snapshot contents to the object store. Provider
and storage integrations are delivered as plugins. See
[How Velero works](https://velero.io/docs/main/how-velero-works/).

### Actors

| Actor | Description | Isolation |
| -- | -- | -- |
| Velero server | Single-replica `Deployment` in the Velero namespace running the backup, restore, schedule and related controllers. | Own pod and service account. Container drops all Linux capabilities, sets `allowPrivilegeEscalation: false` and the `RuntimeDefault` seccomp profile. Image runs as non-root user `cnb`. |
| node-agent | `DaemonSet` that performs file system backup/restore and data movement on each node. | Own pod. Runs as UID 0 and mounts the kubelet pod volume host path unless disabled (`--node-agent-disable-host-path`). Privileged mode is opt-in (`--privileged-node-agent`, needed only to back up block devices). |
| Plugins | Object store, volume snapshotter, and backup/restore item action plugins. Added to the server pod as init containers that copy the plugin binary into a shared volume. | Separate processes from the server; talk to the server over [go-plugin](https://github.com/hashicorp/go-plugin) gRPC. They share the server pod, service account and credentials. |
| Velero CLI | Local client that creates and inspects Velero custom resources and runs `velero install`. | Runs with the user's kubeconfig permissions. |
| Backup storage location | Object store holding backup metadata, logs, and (for file system backup and data movement) Kopia repository data. | External to the cluster; protected by the storage provider and the credentials Velero is given. |
| `velero-restore-helper` / `velero-helper` | Small binaries run as init containers or helpers (for example restore-wait init containers injected into restored pods). | Run inside the restored workload's pod or Velero's own pods. |

**TODO(team):** confirm this actor list is complete, in particular data mover pods and any actors added in recent releases.

### Actions

* **Backup.** The server lists resources through the Kubernetes API using its service account, applies resource
  filters, runs backup item action plugins and optional backup hooks (exec in pods), writes serialized resources
  and logs to the backup storage location through the object store plugin, and requests volume snapshots or
  starts file system backup / data movement for volumes in scope.
* **Restore.** The server reads backup contents from the backup storage location, runs restore item action
  plugins and optional restore hooks (exec and init containers), recreates resources through the Kubernetes API,
  and restores volume data from snapshots, file system backup, or the data mover.
* **Schedule.** The server creates `Backup` resources on a cron schedule.
* **Install / upgrade.** `velero install` creates the namespace, CRDs, service account, role binding, deployment,
  optional node-agent daemon set, and the credentials secret for the default location.

Backup and restore hooks: [backup hooks](https://velero.io/docs/main/backup-hooks/),
[restore hooks](https://velero.io/docs/main/restore-hooks/).

### Goals

* Back up and restore Kubernetes resources and persistent volume data, and migrate workloads across clusters.
* Authenticate to object storage and snapshot providers with credentials supplied by the operator, stored as
  Kubernetes secrets, and support per-location credentials so locations do not have to share them.
* Verify TLS to the object store, including with a custom CA bundle (`--cacert`, `CACertRef` on the
  `BackupStorageLocation`).
* Let operators run Velero with narrower permissions than the default through
  [restrictive RBAC](https://velero.io/docs/main/rbac/).

### Non-goals

* Velero does not itself encrypt backup metadata or provider snapshots. It relies on the native mechanisms of the
  object store and snapshot system ([locations](https://velero.io/docs/main/locations/)).
* Velero's built-in file system backup and data mover use a static, common repository encryption key for all
  backup repositories. **Anyone with access to the backup storage can decrypt that backup data**, so storage access
  must be restricted
  ([file system backup limitations](https://velero.io/docs/main/file-system-backup/),
  [data mover limitations](https://velero.io/docs/main/csi-snapshot-data-movement/)).
* Velero does not sandbox plugins from the server: a plugin runs in the server pod and can use the server's
  service account and credentials. Only install plugins you trust.
* By default, `velero install` binds the Velero server service account to the `cluster-admin` ClusterRole so it can
  back up and restore any resource in the cluster; narrowing this is the operator's responsibility (see
  [RBAC](https://velero.io/docs/main/rbac/)).
  **TODO(team):** decide whether to state a tenancy position, for example whether tenant isolation or per-namespace
  `Backup`/`Restore` access is a goal or a non-goal. Until then this document states only the default behavior.

## Self-assessment use

This self-assessment is created by the Velero maintainers to perform an internal analysis of the project's
security. It is not intended to provide a security audit of Velero, or function as an independent assessment or
attestation of Velero's security health.

This document provides Velero users with an initial understanding of Velero's security, where to find existing
security documentation, and a general overview of Velero's security practices, both for development of Velero and
for security of Velero deployments.

This document provides CNCF TAG Security with an initial understanding of Velero to assist in a joint assessment,
and, together with the joint assessment, serves as a basis for a future security audit.

## Security functions and features

### Critical

These are design elements that make the product itself secure and are not configurable by the operator.

* **Kubernetes RBAC on Velero custom resources.** Creating `Backup`, `Restore`, `Schedule`,
  `BackupStorageLocation` and related resources is governed by Kubernetes RBAC. Anyone able to create these in the
  Velero namespace can cause cluster data to be written to, or read from, the configured storage location.
* **Credentials held as Kubernetes secrets.** Object store and snapshot credentials are read from secrets in the
  Velero namespace (default `cloud-credentials`, or a per-location secret reference), not from CLI flags or
  container images.
* **Plugin process boundary.** Plugins run as separate processes and communicate with the server through
  go-plugin gRPC, rather than being linked into the server.

**TODO(team):** review this list for additional non-configurable security properties (for example, validation of
backup tarball contents on restore, path handling during extraction). A short source-level review is needed before
claiming these.

### Security relevant

* **Server RBAC.** `velero install` binds the server service account to `cluster-admin` by default. Operators can
  supply their own service account (`--service-account-name`) with a restricted role, as documented in
  [RBAC](https://velero.io/docs/main/rbac/).
* **Container hardening.** The server container drops all capabilities, disables privilege escalation and uses the
  `RuntimeDefault` seccomp profile; the image runs as a non-root user on a minimal
  (`ubuntu-noble-run-tiny`) base image. The node-agent runs as UID 0 with a host path mount and is only privileged
  when `--privileged-node-agent` is set.
* **Node-agent host path.** The pod volume host path mount required by file system backup can be disabled
  (`--node-agent-disable-host-path`) when only snapshot-based methods are used.
* **TLS to object storage.** `--cacert` / `CACertRef` supply a CA bundle. `--insecure-skip-tls-verify` exists for
  development and testing and is documented as insecure
  ([self-signed certificates](https://velero.io/docs/main/self-signed-certificates/)).
* **Per-location credentials.** `BackupStorageLocation` and `VolumeSnapshotLocation` can reference their own
  credentials secret, so locations need not share credentials
  ([locations](https://velero.io/docs/main/locations/)).
* **Backup and restore hooks** execute commands in user pods; their use is controlled by annotations and `Backup` /
  `Restore` specs, and therefore by RBAC on those resources.

## Project compliance

Velero does not currently claim compliance with a formal security standard (PCI-DSS, ISO 27001, SOC 2, GDPR,
etc.).

**TODO(team):** confirm there is no compliance statement the project wants to make.

## Secure development practices

### Development pipeline

* **Signed-off commits.** Pull requests must pass the DCO check (required status check on `main`).
* **Code review.** The `main` branch is protected: two approving reviews, review by a code owner
  ([`CODEOWNERS`](https://github.com/velero-io/velero/blob/main/.github/CODEOWNERS) assigns the maintainer team),
  stale approvals dismissed on new commits, rules enforced for administrators, force pushes disabled.
* **Required CI.** DCO and the `Run CI` check (`make ci`: Go module verification, generated-code verification,
  build and unit tests) must pass before merge. Separate pull request workflows run the linter, spell checking and
  end-to-end tests on kind.
* **Automated vulnerability and supply chain checks.**
  * Nightly [Trivy scan](https://github.com/velero-io/velero/blob/main/.github/workflows/nightly-trivy-scan.yml)
    (critical, high and medium severities) uploaded to GitHub code scanning.
  * [OpenSSF Scorecard](https://github.com/velero-io/velero/blob/main/.github/workflows/scorecard.yml) workflow,
    weekly and on push to `main`, with results published.
  * [Dependabot](https://github.com/velero-io/velero/blob/main/.github/dependabot.yml) for Go modules (including
    `pkg/apis`) and GitHub Actions, plus Dependabot security updates.
  * GitHub secret scanning with push protection is enabled on the repository.
* **GitHub Actions hygiene.** Workflows declare read-only default permissions; the Scorecard `Token-Permissions`
  and `Dangerous-Workflow` checks currently score 10/10.
* **Releases.** Release binaries are built with GoReleaser and accompanied by a checksum file; the process is
  documented in
  [release instructions](https://github.com/velero-io/velero/blob/main/site/content/docs/main/release-instructions.md).
  Release artifacts and container images are **not** currently signed, and SBOM and provenance attestations are not
  published (build scripts set `--sbom=false` and `--provenance=false`). Commit signing is not required.
  **TODO(team):** confirm; these are the main gaps (see [Appendix](#appendix)).
* **Static analysis and fuzzing.** CodeQL static analysis for Go runs on pull requests, pushes to `main` and weekly
  ([`codeql.yml`](https://github.com/velero-io/velero/blob/main/.github/workflows/codeql.yml), added in
  [velero-io/velero#10695](https://github.com/velero-io/velero/pull/10695), merged 2026-10-30). Its findings are
  informational for now; the check is not yet required. No fuzzing is configured today.
  **TODO(team):** confirm whether and when to make the CodeQL check required (proposed: after the first few weeks,
  once the `main` baseline is clean), and decide on a fuzzing plan.
* **Access control.** Write access is limited to maintainers through the GitHub team referenced in `CODEOWNERS`.
  **TODO(team):** document that two-factor authentication is required for the `velero-io` organization after
  verifying the org setting, and describe how repository, release and registry credentials are scoped and rotated.

### Communication channels

* **Internal.** Maintainers coordinate through GitHub, Slack, and the community meetings; non-public channels, if
  any, are limited to coordination of security reports. **TODO(team):** list private channels and their purpose.
* **Inbound.** GitHub issues, `#velero-users` and `#velero-dev` on the Kubernetes Slack, and the
  [Velero Google Group](https://groups.google.com/forum/#!forum/projectvelero). Security reports go to
  cncf-velero-security@lists.cncf.io or GitHub private vulnerability reporting.
* **Outbound.** GitHub releases and advisories, the Velero blog, Slack, and the mailing list.

### Ecosystem

Velero integrates with the Kubernetes API, the CSI snapshot API, Kopia, and cloud and on-premises storage
providers through plugins; see the [supported providers](https://velero.io/docs/main/supported-providers/) list.
Velero is also redistributed downstream; per the security policy, distributors publish their own advisories.

## Security issue resolution

### Responsible disclosure process

The full policy is in [`SECURITY.md`](https://github.com/velero-io/.github/blob/main/SECURITY.md). In summary:

* Report privately to cncf-velero-security@lists.cncf.io, or through GitHub private vulnerability reporting on the
  repository Security tab. Public GitHub issues must not be used for vulnerabilities.
* The Security Team acknowledges and starts a conversation with the reporter within 3 business days.
* The team assesses impact, scores the issue with CVSS, requests a CVE (kept private until disclosure), develops and
  tests a fix, and agrees a disclosure date with the reporter. For a critical vulnerability with a straightforward
  mitigation, public disclosure is expected within about 14 business days of the report.
* The fix ships in the next patch or minor release and is announced through a GitHub security advisory, with
  mitigations where applicable. Velero does not operate a private distributor embargo list and has no bug bounty
  program; the default is to have a patched release available at the time of public disclosure.

### Vulnerability response process

Reports sent to the security list are triaged by the Velero Security Team and shared with the maintainers who hold
committer and release permissions.

**TODO(team):** the incubation criteria ask for *documented assignment of security response roles* (who triages,
who scores, who prepares the fix and release, who communicates, and the escalation path), modeled on the
Kubernetes Security Response Committee. This PR adds a **Security Response Roles** section to `SECURITY.md` that
defines the roles, responsibilities and escalation path. The current role holders are not yet named: fill in the
roster in that section and confirm the role definitions.

**TODO(team):** `SECURITY.md` previously stated that only the most recent Velero version is supported, while the
[support process](https://velero.io/docs/main/support-process/) states best-effort support for the current and n-1
minor versions. This PR aligns `SECURITY.md` with the support process (fixes released for the latest version,
backported to n-1 on a best-effort basis). Confirm that policy, in particular that CVE fixes are backported to n-1.

### Incident response

The project has a documented vulnerability response process (above) but no separate incident response runbook for
a compromise of project infrastructure (for example repository, CI, release or registry credentials).
**TODO(team):** decide whether to document one before the joint assessment.

## Appendix

### Known issues over time

Published advisories are listed at <https://github.com/velero-io/velero/security/advisories>. As of 2026-10-22:

| Advisory | CVE | Severity | Published |
| -- | -- | -- | -- |
| GHSA-72xg-3mcq-52v4 | CVE-2020-3996 | Medium | 2020-10-21 |
| GHSA-j2g6-362q-6qc6 | CVE-2026-32637 | Medium | 2026-08-19 |
| GHSA-8gv3-c356-fw76 | — | Medium | 2026-09-28 |
| GHSA-vrfp-2x59-hcfm | — | Medium | 2026-09-28 |

**TODO(team):** add a one-line description and fixed-in version for each, and confirm whether the two without a CVE
will be assigned one.

### OpenSSF Best Practices

* Project entry: <https://www.bestpractices.dev/projects/3811>. Passing-level criteria are recorded as 100%
  complete, but the entry still points at the former `vmware-tanzu/velero` repository URL and its answers were last
  saved on 2020-08-31, against older criteria. **TODO(team):** an entry owner or admin must update the repository URL
  to `velero-io/velero`, re-verify the answers against the current criteria, and confirm the passing badge. The
  README badge link was moved to `bestpractices.dev` in [velero-io/velero#10683](https://github.com/velero-io/velero/pull/10683).
* OpenSSF Scorecard (<https://scorecard.dev/viewer/?uri=github.com/velero-io/velero>) was 6.9 on 2026-10-22.
  Full marks: Code-Review, Token-Permissions, Dangerous-Workflow, Dependency-Update-Tool, Maintained,
  Security-Policy, CI-Tests, Binary-Artifacts, License, Packaging, Contributors. Open items:
  * `SAST` (0): CodeQL was added in [velero-io/velero#10695](https://github.com/velero-io/velero/pull/10695); the score
    should update on the next Scorecard run.
  * `Signed-Releases` (0): sign release artifacts and images (for example with cosign); publish SBOM and provenance.
  * `Fuzzing` (0): add fuzz targets for parsers and archive handling.
  * `Pinned-Dependencies` (3): pin remaining GitHub Actions and container base images by digest.
  * `Vulnerabilities` (1): triage open dependency vulnerabilities.
  * `CII-Best-Practices` (0): resolved by updating the badge repository URL above.
  * `Branch-Protection` (-1): branch protection is configured, but the Scorecard token cannot read it;
    provide a token with the required scope.

### Case studies

**TODO(team):** add 2-3 real-world deployment scenarios (for example disaster recovery of a production cluster,
cross-cluster migration, scheduled application backups with file system backup). Candidates exist under
[site/content/casestudies](https://github.com/velero-io/velero/tree/main/site/content/casestudies).

### Related projects / vendors

**TODO(team):** summarize how Velero differs from the projects users commonly compare it with (for example
Kanister and commercial Kubernetes backup products), and its relationship to downstream distributions.

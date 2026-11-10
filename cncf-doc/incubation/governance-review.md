# Velero Governance Self-Assessment

This self-assessment supports Velero's proposed move from CNCF Sandbox to Incubation. It describes the governance model, supporting evidence, and current documentation gaps. It is not a completed TOC review or an approval of the application.

## Summary and Assessment

**Status: Satisfactory**

**Assessment date: 2026-11-10**

Velero documents consistent decision rules, a complete maintainer lifecycle, shared security-response responsibilities, and public participation channels. The maintainer guide links to the canonical governance document and accurately describes review assignment. Ownership lists match the active maintainer roster. Governance identifies the governed repositories and classifies the Kopia fork as a dependency managed by Velero maintainers. No outstanding Must-Fix items were identified in the reviewed evidence; the remaining improvements are non-blocking.

### Executing the Assessment

The review covers every criterion in the [CNCF governance review template][template]. The [application template][application] takes precedence for submission readiness: it requires several items labeled Suggested here, including discoverability, accuracy, vendor neutrality, decision processes, team assignments, maintainer lifecycle, and the contributor ladder. Its additional affiliation-update requirement is assessed separately below.

| Evidence | Scope |
| --- | --- |
| `velero-io/.github` | governance, security, conduct, contribution, support, and organization-profile files |
| `velero-io/velero` | maintainer roster, maintainer guide, ownership, review automation, dependencies, and community documentation |
| Community evidence | Website source, [public community page][community], and linked meeting resources |

### Must-Fix Items

No outstanding Must-Fix items were identified in this self-assessment.

### Points of Excellence

- Company aggregation limits employer voting weight independently of the number of maintainers employed by that company. Affiliations must be updated within 30 days of employment changes.
- Onboarding assigns coordination, access implementation, and verification to identifiable roles.
- Security membership follows the maintainer lifecycle, including removal of private security access when a maintainer leaves the role.
- Maintainers share responsibility across the project, and public roster history demonstrates both appointments and emeritus transitions.

### Areas for Improvement

The following improvements are non-blocking:

- In [SECURITY.md](../../SECURITY.md#security-contacts), identify the Security Team as all active maintainers and link to [its governance definition](../../GOVERNANCE.md#function-based-teams). The **Reporting a Vulnerability** and **Security Contacts** paragraphs describe forwarding reports from the Security Team to maintainers, which unnecessarily suggests separate groups. Describe the team as triaging reports and coordinating investigation, fixes, and disclosure.
- Retain public records of governance decisions and maintainer transitions, including the applicable procedure and outcome. Existing nomination issues and pull requests can serve this purpose.
- Document component maturity and add/remove procedures as the subproject model develops; this is a suggested incubation criterion.


---

## Review

This review assesses the current evidence for the developing [Velero Incubation application](./incubation-application.md).

### Governance Summary

Velero uses maintainer-led governance with public participation through issues, pull requests, proposals, and community meetings. Maintainers hold final review and release responsibilities. Routine decisions seek consensus; formal votes require two-thirds of yes/no votes after company aggregation. Explicit formal-vote requirements take precedence over lazy consensus. All active maintainers share security-response responsibilities.

### Governance Evolution

**Governance has continuously been iterated upon by the project as a result of their experience applying it, with the governance history demonstrating evolution of maturity alongside the project's maturity evolution.**

**Incubating: Suggested -- Satisfied.**

The [governance history](https://github.com/velero-io/.github/commits/main/GOVERNANCE.md) records changes addressing maintainer lifecycle, affiliations, conduct, repository scope, voting, and implementation responsibilities. These changes show continued development of the governance model.

### Discoverability

**Clear and discoverable project governance documentation.**

**Incubating: Suggested -- Satisfied.**

The main README, [maintainer roster][maintainers], and [maintainer guide][maintainer-guide] link to governance in `velero-io/.github`. Governance links to the roster, conduct policy, and security policy. The maintainer guide's repository-scope, lazy-consensus, and maintainer-entry links use the canonical location. Contribution and support entry points are available through organization defaults and the website.

### Accuracy and Clarity

**Governance is up to date with actual project activities, including any meetings, elections, leadership, or approval processes.**

**Incubating: Suggested -- Satisfied.**

The documented proposal, review, release, support, and meeting processes are supported by repository documentation and public activity. There are regular community meetings held according to the schedule in [LFX calendar](https://zoom-lfx.platform.linuxfoundation.org/meetings/velero?view=week).

**Governance clearly documents [vendor-neutrality] of project direction.**

**Incubating: Suggested -- Satisfied**

[Maintainer affiliations](../../GOVERNANCE.md#maintainer-affiliations) explicitly connects accurate affiliations to vendor neutrality. Company aggregation prevents employer headcount from increasing formal voting weight.

### Decisions and Role Assignments

**Document how the project makes decisions on leadership roles, contribution acceptance, requests to the CNCF, and changes to governance or project goals.**

Incubating: Suggested -- Satisfied as documented.

The following procedures cover the specified decision categories:

| Decision | Applicable process |
| --- | --- |
| Leadership appointments | Existing-maintainer nomination and formal supermajority vote |
| Involuntary removal or inactivity-based emeritus transition | Formal supermajority vote; voluntary departure is separately permitted |
| Contribution acceptance | Maintainer review; the maintainer guide specifies two PR approvals |
| Requests to CNCF | The general rule governing all project decisions |
| Substantive governance amendments | Formal supermajority vote |
| Project goals and direction | Public proposals, discussion, consensus or voting; accepted proposals define the roadmap |

[Decision Making](../../GOVERNANCE.md#decision-making) supplies the common voting procedure. Explicit formal-vote requirements take precedence over lazy consensus.

**Document how role, function-based members, or sub-teams are assigned, onboarded, and removed for specific teams (example: Security Response Committee).**

**Incubating: Suggested -- Satisfied.**

[Function-based Teams](../../GOVERNANCE.md#function-based-teams) identifies all active maintainers as the Security Team. Membership follows appointment, departure, emeritus status, and removal. Relevant service administrators grant or revoke access to private security resources, and the maintainer coordinating the transition verifies completion. [SECURITY.md](../../SECURITY.md) describes investigation, fixes, and disclosure responsibilities. No other standing function-based teams are documented; future teams must document membership, responsibilities, and transitions.

### Maintainers and Maintainer Lifecycle

**Document a complete maintainer lifecycle process (including roles, onboarding, offboarding, and emeritus status).**

**Incubating: Suggested -- Satisfied.**

[Governance](../../GOVERNANCE.md#maintainers) covers contribution expectations, nomination, election, onboarding, voluntary departure, inactivity, removal, access revocation, emeritus recognition, and return. The nominating maintainer coordinates onboarding and verifies completion. GitHub administrators implement GitHub access changes, and relevant service administrators implement other access changes.

**Demonstrate usage of the maintainer lifecycle with outcomes, either through the addition or replacement of maintainers as project events have required.**

**Incubating: Suggested -- Demonstrated.**

Public examples include [Tiger Kaovilai's appointment](https://github.com/velero-io/velero/commit/4071435023dac25cee57c85f8e46c331505a5bdd) and [Ming Qiu's emeritus transition](https://github.com/velero-io/velero/commit/677d99a857377464053a5087a6b510b12b5057c8), both in November 2024. These records demonstrate actual lifecycle outcomes; they do not establish retrospective compliance with the current voting procedure.

**Document complete list of current maintainers, including names, contact information, domain of responsibility, and affiliation.**

**Incubating: Required -- Satisfied.**

The [roster][maintainers] provides eight names, GitHub identities and contact routes, affiliations, and collective responsibility for the whole project. Responsibility is shared rather than divided into exclusive component ownership. Maintainer contact routes include GitHub, development Slack, and the advertised Google Group; security reports use private channels.

**A number of active maintainers which is appropriate to the size and scope of the project.**

**Incubating: Required -- Supported**

The eight-person roster, contributions by multiple maintainers, shared review responsibilities, and public meeting records support continued maintenance across the project. Sampled Git history covers May–November 2026.

**Project maintainers from at least 2 organizations that demonstrates survivability.**

**Incubating: N/A -- Multiple affiliations documented.**

The roster records Broadcom, OpenShift, and Microsoft Azure affiliations. This supports organizational diversity, although employment and long-term staffing commitments were not independently verified.

**Maintainer affiliations are current and must be updated within 30 days of an employment change.**

Incubating: Required -- Satisfied as documented; employment not independently verified.

This additional criterion comes from the application template. [Maintainer affiliations](../../GOVERNANCE.md#maintainer-affiliations) requires updates within 30 days, and the roster records affiliations for all maintainers. A [February 2026 roster update](https://github.com/velero-io/velero/commit/31e9dcbb875af69d401ba317e5d139240a54f048) documents affiliation corrections. Confirm current employment information when submitting the application.

### Ownership

**Code and Doc ownership in Github and elsewhere matches documented governance roles.**

**Incubating: Required -- Satisfied.**

[`CODEOWNERS`][codeowners] delegates to `@velero-io/Maintainer`. The main [reviewer configuration][autoassign] includes all eight maintainers and a documentation-review subset consisting of current maintainers. Sampled provider and example plugin reviewer groups contain only active maintainers; routing subsets do not themselves limit project-wide authority.

The [root `OWNERS` file](https://github.com/velero-io/velero/blob/main/OWNERS) lists all eight active maintainers as both reviewers and approvers. Both lists exactly match the maintainer roster when compared case-insensitively. The maintainer guide describes the review-assignment process consistently with the checked-in automation. Actual GitHub team membership and branch protection were not audited.

### Code of Conduct

**Document adoption and adherence to the CNCF Code of Conduct or the project's CoC which is based off the CNCF CoC and not in conflict with it.**

**Incubating: Required -- Satisfied**

[CODE_OF_CONDUCT.md](../../CODE_OF_CONDUCT.md) adopts the CNCF Code of Conduct, supplies reporting contacts, and gives the CNCF policy precedence over conflicting included Contributor Covenant text. It documents expected conduct and reporting routes. Confidential enforcement cases were outside this documentation review.

**CNCF Code of Conduct is cross-linked from other governance documents.**

**Incubating: Required -- Satisfied.**

[GOVERNANCE.md](../../GOVERNANCE.md#code-of-conduct) directly links the conduct policy and applies it to maintainers, contributors, and users. The contributor entry documentation also points participants to the policy.

### Subprojects

**All subprojects, if any, are listed.**

**Incubating: Required -- Satisfied.**

[Code Repositories](../../GOVERNANCE.md#code-repositories) lists the main repository, AWS/GCP/Azure plugins, example plugin, and `.github`. [Dependency forks](../../GOVERNANCE.md#dependency-forks) classifies the Kopia fork as a dependency rather than a subproject and assigns its Velero-specific changes and dependency updates to Velero maintainers. This matches the fork used in the [dependency declaration][gomod]. Upstream Kopia retains its own maintainers and governance.

**If the project has subprojects: subproject leadership, contribution, maturity status documented, including add/remove process.**

**Incubating: Suggested -- Partially satisfied**

Listed components use project-wide maintainer leadership, governance, and contribution processes. Separate component maturity classifications and a distinct add/remove process are not documented. Documenting these would improve completeness, especially if components acquire separate leadership or maturity levels. This suggested criterion is not treated as an additional incubation blocker.

### Contributors and Community

**Contributor ladder with multiple roles for contributors.**

**Incubating: Suggested -- Satisfied.**

[Community Roles](../../GOVERNANCE.md#community-roles) defines users, contributors, and maintainers. Sustained quality contributions and community participation establish eligibility for nomination by an existing maintainer, followed by a formal vote. This provides a documented progression path without requiring a separate intermediate reviewer role.

**Clearly defined and discoverable process to submit issues or changes.**

**Incubating: Required -- Satisfied.**

The [contribution entry point](../../CONTRIBUTING.md), [code standards](https://velero.io/docs/main/code-standards/), [issue templates](https://github.com/velero-io/velero/tree/main/.github/ISSUE_TEMPLATE), and [pull-request template](https://github.com/velero-io/velero/blob/main/.github/pull_request_template.md) explain how to submit changes and reports. Significant changes use the public [proposal process](../../GOVERNANCE.md#proposal-process).

**Project must have, and document, at least one public communications channel for users and/or contributors.**

**Incubating: Required -- Satisfied.**

Public GitHub issues, pull requests, and discussions provide participation channels. The [organization profile](../../profile/README.md#community) documents Kubernetes Slack `#velero-users` and `#velero-dev`, with workspace signup required. The project therefore provides multiple public channels.

**List and document all project communication channels, including subprojects (mail list/slack/etc.). List any non-public communications channels and what their special purpose is.**

**Incubating: Required -- Satisfied for the documented channels.**

The reviewed documentation supports the following inventory:

| Channel | Access and purpose | Evidence |
| --- | --- | --- |
| GitHub issues, PRs, and discussions | Public support, contributions, proposals, and community questions | [Main repository](https://github.com/velero-io/velero), [community page][community] |
| Kubernetes Slack `#velero-users` | Public user support, subject to workspace signup | [Organization profile](../../profile/README.md#community), [support guide][support-guide] |
| Kubernetes Slack `#velero-dev` | Public development and maintainer discussion, subject to workspace signup | [Maintainer contacts][maintainers], [organization profile](../../profile/README.md#community) |
| Velero Google Group | Publicly advertised discussion and project updates | [Roster][maintainers] and [main README](https://github.com/velero-io/velero/blob/main/README.md) |
| Community meetings, LFX calendar, HackMD notes, and YouTube recordings | Public scheduling, participation, agendas, and records | [Community page][community] |
| Project Twitter/X and LinkedIn | Public announcements | [Community page][community] |
| `cncf-velero-security@lists.cncf.io` | Private vulnerability reporting and responder coordination | [Security policy](../../SECURITY.md#mailing-lists) |
| GitHub private vulnerability reporting and advisories | Private reporting and coordination before disclosure | [Security reporting policy](../../SECURITY.md#reporting-a-vulnerability---private-disclosure-process) |
| CNCF conduct reporting contact | Confidential conduct reports to the CNCF committee | [Code of Conduct](../../CODE_OF_CONDUCT.md) |

The support process covers project repositories and plugins through shared channels. The maintainer roster and provider-support page direct development discussions to `#velero-dev`, and troubleshooting directs user support to `#velero-users`. Historical versioned documentation was not exhaustively audited. Google Group activity and moderation were not verified. Any additional operational channels should be documented with their purposes.

**Up-to-date public meeting schedulers and/or integration with CNCF calendar.**

**Incubating: Required -- Satisfied.**

The [community page][community] publishes alternating regional meeting information, an [LFX calendar](https://zoom-lfx.platform.linuxfoundation.org/meetings/velero?view=week), a subscription link, notes, and recordings. Public notes contain 2026 project discussions.

**Documentation of how to contribute, with increasing detail as the project matures.**

**Incubating: Required -- Satisfied.**

[Start contributing](https://velero.io/docs/main/start-contributing/) explains participation routes, design proposals, and community resources. [Code standards](https://velero.io/docs/main/code-standards/) covers pull requests, changelogs, coding conventions, and DCO sign-off. The [maintainer guide][maintainer-guide] documents review, release, support, and community responsibilities. These provide guidance beyond the initial contribution entry point.

**Demonstrate contributor activity and recruitment.**

Incubating: Required -- Satisfied on available evidence.

Reviewed Git history contains contributions from maintainers and other authors. The [community page][community] advertises good-first-issue and help-wanted queues and connects newcomers to contribution guidance and meetings. [Public meeting notes](https://hackmd.io/fCDVjqGuTG23CoOWQpoEVg) provide examples of contributor participation and technical discussion. This demonstrates activity and recruitment opportunities, without establishing a contributor-growth trend or guaranteed mentoring for every issue.

[template]: https://github.com/cncf/toc/blob/main/operations/project-reviews/governance-review-template.md
[application]: https://github.com/cncf/toc/blob/main/.github/ISSUE_TEMPLATE/template-incubation-application.md
[maintainers]: https://github.com/velero-io/velero/blob/main/MAINTAINERS.md
[codeowners]: https://github.com/velero-io/velero/blob/main/.github/CODEOWNERS
[autoassign]: https://github.com/velero-io/velero/blob/main/.github/auto-assignees.yml
[gomod]: https://github.com/velero-io/velero/blob/main/go.mod
[maintainer-guide]: https://github.com/velero-io/velero/blob/main/site/content/docs/main/maintainers.md
[support-guide]: https://github.com/velero-io/velero/blob/main/site/content/docs/main/support-process.md
[community]: https://velero.io/community/
[vendor-neutrality]: https://contribute.cncf.io/maintainers/community/vendor-neutrality/

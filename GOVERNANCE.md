# Velero Governance

This document defines the project governance for Velero.

## Overview

**Velero**, an open source project, is committed to building an open, inclusive, productive and self-governing open source community focused on building a high quality tool that enables users to safely backup and restore, perform disaster recovery, and migrate Kubernetes cluster resources and persistent volumes. The community is governed by this document with the goal of defining how community should work together to achieve this goal.

## Code of Conduct

The Velero community abides by the [CNCF Code of Conduct](./CODE_OF_CONDUCT.md). All community members, including maintainers, contributors, and users, are expected to follow it in every project space and interaction.

## Code Repositories

The following repositories and components are governed by the Velero community.

* **[Velero](https://github.com/velero-io/velero):** Main Velero codebase
* **[Velero Plugin for AWS](https://github.com/velero-io/velero-plugin-for-aws):** This repository contains the plugins to support running Velero on AWS, including the object store plugin and the volume snapshotter plugin
* **[Velero Plugin for GCP](https://github.com/velero-io/velero-plugin-for-gcp):** This repository contains the plugins to support running Velero on GCP, including the object store plugin and the volume snapshotter plugin
* **[Velero Plugin for Azure](https://github.com/velero-io/velero-plugin-for-microsoft-azure):** This repository contains the plugins to support running Velero on Azure, including the object store plugin and the volume snapshotter plugin
* **[Velero Plugin Example](https://github.com/velero-io/velero-plugin-example):** This repository contains example plugins for Velero
* **[.github](https://github.com/velero-io/.github):** Shared governance, community health files, and organization profile for the Velero project

### Dependency forks

Velero uses [a fork of Kopia](https://github.com/velero-io/kopia) as a dependency, rather than a subproject. Velero maintainers manage its Velero-specific changes and dependency updates. Upstream Kopia has its own maintainers and governance.

## Community Roles

* **Users:** Members that engage with the Velero community via any medium (Slack, GitHub, mailing lists, etc.).
* **Contributors:** Regular contributions to projects (documentation, code reviews, responding to issues, participation in proposal discussions, contributing code, etc.). 
* **Maintainers**: The Velero project leaders. They are responsible for the overall health and direction of the project; final reviewers of PRs and responsible for releases. Some Maintainers are responsible for one or more components within a project, acting as technical leads for that component. Maintainers are expected to contribute code and documentation, review PRs including ensuring quality of code, triage issues, proactively fix bugs, and perform maintenance tasks for these components.

### Maintainers

The current maintainers are listed in [MAINTAINERS.md](https://github.com/velero-io/velero/blob/main/MAINTAINERS.md), along with their affiliation.

#### Becoming a maintainer

Contributors who have shown a sustained history of quality contributions to the project, code, code reviews, issue triage, proposal authoring and discussion, and community participation, may be nominated by an existing maintainer to become a maintainer. New maintainers must be elected through a supermajority vote using the procedure in [Decision Making](#decision-making).

After election, the nominating maintainer coordinates onboarding. They ensure that MAINTAINERS.md and applicable ownership and review-routing records are updated, and introduce the new maintainer to the project's review, release, support, and security procedures. A maintainer with GitHub organization administration permissions implements the approved GitHub access changes, while the relevant service administrators grant other access appropriate to the role. The nominating maintainer verifies completion and records it in the nomination issue or pull request.

#### Stepping down and emeritus status

A maintainer may step down at any time by notifying the other maintainers, and is then moved to the Emeritus Maintainers list in [MAINTAINERS.md](https://github.com/velero-io/velero/blob/main/MAINTAINERS.md).  A maintainer who has been inactive in the project for an extended period (for example, six months) may be moved to emeritus status through a supermajority vote using the procedure in [Decision Making](#decision-making). Emeritus maintainers are recognized for their past contributions and may return to active maintainer status through the nomination process above.

#### Removing a maintainer

Maintainers may voluntarily step down as described above. A maintainer may also be removed involuntarily when the responsibilities and requirements of the role are not being met. This may include a repeated pattern of inactivity, an extended period of inactivity, a sustained failure to meet the responsibilities of the role, or a violation of the [Code of Conduct](#code-of-conduct). Involuntary removal protects the health of the project and its deliverables, and opens up opportunities for new contributors to step in. Involuntary removal is decided through a supermajority vote using the procedure in [Decision Making](#decision-making).

When a maintainer steps down or is removed, their elevated access and permissions (for example GitHub, release, and infrastructure access) are revoked, and they are moved to the Emeritus Maintainers list in [MAINTAINERS.md](https://github.com/velero-io/velero/blob/main/MAINTAINERS.md), or removed from it entirely in the case of removal for a Code of Conduct violation. Conduct-related concerns may also be reported to and handled through the [Code of Conduct](#code-of-conduct) contacts, independent of this process. A maintainer moved to emeritus status may return to active maintainer status through the nomination process above.

#### Maintainer affiliations

Maintainers must keep their affiliation in [MAINTAINERS.md](https://github.com/velero-io/velero/blob/main/MAINTAINERS.md) current and update it within 30 days of any change of employer. Because votes are counted per company (see [Decision Making](#decision-making)), accurate affiliations are important to preserving the vendor neutrality of the project.

### Function-based Teams

Velero does not currently operate standing function-based sub-teams.

The Velero Security Team consists of all active maintainers listed in
[MAINTAINERS.md](https://github.com/velero-io/velero/blob/main/MAINTAINERS.md).
Security Team membership follows the maintainer lifecycle described
above: newly elected maintainers join the team, and maintainers who
step down, become emeritus, or are removed leave the team.

As part of maintainer onboarding and offboarding, the relevant service
administrators grant or revoke access to the private security mailing
list and vulnerability-reporting tools. The maintainer coordinating
the transition verifies that these access changes are complete.

The team's responsibilities and vulnerability-handling procedures are
documented in [SECURITY.md](./SECURITY.md). Any additional function-based
teams will have their membership, responsibilities, and onboarding and
offboarding processes documented here.

### Supermajority

A supermajority requires affirmative votes from at least two-thirds of the yes/no votes cast, rounded up, after company aggregation as described in [Decision Making](#decision-making). Abstentions and nonparticipation are excluded from the denominator and do not prevent approval. There is no separate turnout quorum, but at least one yes/no vote must be cast for a decision to pass. For example, two yes votes, one no vote, and two abstentions meet the supermajority threshold.

Voting on decisions can happen on the mailing list, GitHub, Slack, email, or via a voting service, when appropriate. Maintainers can vote "agree, yes, +1", "disagree, no, -1", or "abstain".


### Decision Making

Ideally, all project decisions are resolved by consensus. If impossible, any
maintainer may call a vote. Unless otherwise specified in this document, any
vote will be decided by a [supermajority](#supermajority) using the procedure below.

Votes by maintainers belonging to the same company count as one combined vote; for example, four maintainers employed by fictional company **Valerium** have one combined vote. Each unaffiliated maintainer has one individual vote.

A company's combined vote is yes or no when at least two-thirds of the yes/no votes cast by maintainers from that company support that position. Abstentions and nonparticipation are excluded from this calculation. If neither position reaches that threshold, or no yes/no votes are cast within the company, the company abstains.

Only the resulting yes/no company votes and votes from unaffiliated maintainers count toward the final approval threshold. Companies that abstain or do not participate are excluded from that calculation.


## Proposal Process

One of the most important aspects in any open source community is the concept
of proposals. Large changes to the codebase and / or new features should be
preceded by a proposal in the Velero repository. This process allows for all
members of the community to weigh in on the concept (including the technical
details), share their comments and ideas, and offer to help. It also ensures
that members are not duplicating work or inadvertently stepping on toes by
making large conflicting changes.

The project roadmap is defined by accepted proposals.

Proposals should cover the high-level objectives, use cases, and technical
recommendations on how to implement. In general, the community member(s)
interested in implementing the proposal should be either deeply engaged in the
proposal process or be an author of the proposal.

The proposal should be documented as a separate Markdown file pushed to the root of the 
`design` folder in the [Velero](https://github.com/velero-io/velero/tree/main/design)
repository via PR. The name of the file should follow the name pattern `<short
meaningful words joined by '-'>_design.md`, e.g:
`restore-hooks_design.md`.

Use the [Proposal Template](https://github.com/velero-io/velero/blob/main/design/_template.md) as a starting point.

### Proposal Lifecycle

The proposal PR can follow the GitHub lifecycle of the PR to indicate its status:

* **Open**: Proposal is created and under review and discussion.
* **Merged**: Proposal has been reviewed and is accepted (either by consensus or through a vote).
* **Closed**: Proposal has been reviewed and was rejected (either by consensus or through a vote).

## Lazy Consensus

To maintain velocity in a project as busy as Velero, the concept of [Lazy
Consensus](http://en.osswiki.info/concepts/lazy_consensus) is practiced. Ideas
and / or proposals should be shared by maintainers via
GitHub with the appropriate maintainer groups (e.g.,
`@velero-io/velero-maintainers`) tagged. Out of respect for other contributors,
major changes should also be accompanied by a ping on Slack or a note on the
Velero mailing list as appropriate. Author(s) of proposal, Pull Requests,
issues, etc.  will give a time period of no less than five (5) working days for
comment and remain cognizant of popular observed world holidays.

Other maintainers may chime in and request additional time for review, but
should remain cognizant of blocking progress and abstain from delaying
progress unless absolutely needed. The expectation is that blocking progress
is accompanied by a guarantee to review and respond to the relevant action(s)
(proposals, PRs, issues, etc.) in short order.

Lazy Consensus is practiced for all projects in the `Velero` org, including
the main project repository and the additional repositories.

Lazy consensus does _not_ apply to the process of:

* Removal of maintainers from Velero

Where this document explicitly requires a formal vote, that requirement
takes precedence over lazy consensus. Abstentions and nonparticipation
are handled according to the voting rules in [Decision Making](#decision-making).

## Deprecation Policy

### Deprecation Process

Any contributor may introduce a request to deprecate a feature or an option of a feature by opening a feature request issue in the velero-io/velero GitHub project. The issue should describe why the feature is no longer needed or has become detrimental to Velero, as well as whether and how it has been superseded. The submitter should give as much detail as possible.

Once the issue is filed, a one-month discussion period begins.
Discussions take place in the issue and community meetings. The
submitter or a maintainer records the discussion period's closing
date and time in the issue.

After the discussion period ends, a maintainer opens a formal vote
using the procedure in [Decision Making](#decision-making) and publishes 
the vote's closing date and time in the issue.  Deprecation requires a 
supermajority under that procedure. Votes are recorded and tallied when 
the vote closes.

Non-maintainers may add non-binding votes in comments to the issue as well; these are opinions to be taken into consideration by maintainers, but they do not count as votes. 

If the vote passes, the deprecation window takes effect in the subsequent release, and the removal follows the schedule. 

### Schedule
If deprecation proposal passes by supermajority votes, the feature is deprecated in the next minor release and the feature can be removed completely after two minor versions or equivalent major version e.g., if feature gets deprecated in Nth minor version, then feature can be removed after N+2 minor version or its equivalent if the major version number changes.

### Deprecation Window

The deprecation window is the period from the release in which the deprecation takes effect through the release in which the feature is removed. During this period, only critical security vulnerabilities and catastrophic bugs should be fixed.

**Note:** If a backup relies on a deprecated feature, then backups made with the last Velero release before this feature is removed must still be restorable in version `n+2`. For instance, something like restic feature support, that might mean that restic is removed from the list of supported uploader types in version `n` but the underlying implementation required to restore from a restic backup won't be removed until release `n+2`.

## Updating Governance

All substantive changes in Governance require a supermajority vote using the procedure in [Decision Making](#decision-making).

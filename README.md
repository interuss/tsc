# InterUSS Technical Steering Committee (TSC)

The InterUSS TSC will be responsible for all technical oversight of the open source Project.

## Documents

* [World ATM Congress 2022 presentation on the Open Source Test Suite](assets/WATMC22_InterUSSOpenSourceTestSuite.pdf)

## InterUSS Charters

The InterUSS Technical Charter is located in [CHARTER.md](CHARTER.md).  Note that, under the InterUSS Platform Fund Charter, "Any decision made by the Technical Steering committee is subject to review and approval by the Governing Board."

## Collaboration Tools

### Slack

The InterUSS project maintains a [Slack Workspace](https://interuss.slack.com) for communication and collaboration.  This workspace is [open for anyone to join](https://join.slack.com/t/interuss/shared_invite/enQtNzg0OTcxOTIyNjc0LTQyYzM1MTljYWU1NDRkNjFkZmFlYjA0YTgwNjQ5N2U5OTVhMzBlZjY4NWE3YTgwYzVjNzg3ZjE5ZjRjM2M0ODQ).  Once you join the Slack Workspace, you can participate in any public channels. 

### Mailing List

The TSC for InterUSS can be reached in the #tsc channel of the Slack workspace, or at the [DSS development mailing list](https://groups.io/g/dss-interuss).

### Calendars and Meetings

The InterUSS Project maintains a [public calendar](https://calendar.google.com/calendar/embed?src=c_nn4qg3tof1c73pmrbq7eor1muo%40group.calendar.google.com&ctz=America%2FChicago) for TSC meetings. These meetings are open for anyone to join.

## Members

The current TSC voting members are listed in the [CONTRIBUTING](CONTRIBUTING.md) file in accordance with the [Technical Charter](CHARTER.md).

TSC voting members may be added or removed by a standard TSC vote as described in the [Technical Charter](CHARTER.md#2-technical-steering-committee).  Evaluation of the suitability of a candidate for membership on the TSC should be based primarily on:

1. Ongoing technical involvement with the project as demonstrated by recurring contributions over the past 90 days
1. Availability to perform TSC duties
1. Track record of success with activities similar to TSC responsibilities including:
    1. Proposing best technical strategies following documentation and evaluation of trade-offs
    1. Establishing contribution guidelines, community norms, workflows, processes, release requirements, templates or other direction-setting resources
    1. Facilitating discussions and seeking consensus
    1. Coordinating with stakeholders outside the technical community

## Policies and Procedures

The InterUSS TSC is governed by the [Technical Charter](CHARTER.md). The Charter provides a foundational structure for the TSC on topics such as its scope, how to make decisions, and how to make changes to itself.  At the same time, it grants the TSC a high degree of freedom when determining how to implement the policies of InterUSS. 

The following policies and procedures have been adopted by the TSC.

### Making Decisions

Per the [charter](CHARTER.md), wherever possible the TSC will attempt to make decisions by consensus.  In circumstances where consensus is not possible or if a vote is explicitly required, a majority (or higher, if required by the governance) of TSC voting members must approve in order for the action to proceed.  Votes will be taken over email, and documented in the next meeting.

### Merging PRs into the TSC repository

Pull requests that do not change the charter or governance of the TSC can be merged into this repository provided the following conditions have been met:

* There are no outstanding objections
* There is at least one approval by a TSC member
* The PR has been open for at least 72 hours

Pull requests that change governance of the TSC (excluding the charter) must be open for at least 14 days, unless consensus is reached in a meeting with quorum of voting members.

Pull requests that [amend](CHARTER.md#8-amendments) the charter of the TSC must meet any requirements in the charter.

If consensus cannot be reached, a pull request may still be merged after a vote by the Voting members to override outstanding objections.

#### Fast-Tracking PRs

Special exception is made for pull requests seeking to make any of the following changes to this repository:

- Errata fixes.
- Editorial changes.
- Meeting minutes.
- Updates to team lists.
- Doc fixes.

Charter changes cannot be fast-tracked.

To propose fast-tracking a pull request, apply the ***fast-track*** label. Then add a comment that TSC members may upvote. If someone disagrees with the fast-tracking request, remove the label. Do not fast-track the pull request in that case.

The pull request may be fast-tracked if two TSC members approve the fast-tracking request. To land, the pull request itself still needs two TSC member approvals.

TSC members may request fast-tracking of pull requests they did not author. In that case only, the request itself is also one fast-track approval. Upvote the comment anyway to avoid any doubt.

### IP Policy

The [InterUSS IP Policy](https://github.com/interuss/tsc/blob/main/CHARTER.md#7-intellectual-property-policy) is defined in the [charter](CHARTER.md), and it applies unless an exception is explicitly approved by the governing board.

#### Copyright Notices

InterUSS follows the [community best practice](https://www.linuxfoundation.org/blog/2020/01/copyright-notices-in-open-source-software-projects/) of not requiring contributors to add a notice to each file.

#### SPDX

Contributors are encouraged (but not required) to adopt the practice of including [SPDX short form identifiers](https://spdx.org/ids-how) in their files.

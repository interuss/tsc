# Review guidelines

Across all InterUSS repositories, a primary responsibility of [Committers](CHARTER.md#2-technical-steering-committee) is to review pull requests from Contributors (including other Committers).  Approval from a Committer is required before a pull request may be merged into the main/master branch of an InterUSS repository.  This page documents InterUSS expectations regarding Committers' review process leading to such an approval.

Any contributor may provide feedback on a pull request in the form of a review and/or approval, but Committer reviews are ultimately necessary and sufficient to merge a pull request.

## Level of compliance

Above all other guidance in this document, Committers are expected to maintain the quality of InterUSS products when contributing, and approving and merging others' contributions, while also promoting timely development to meet InterUSS objectives.  Although the guidelines in this document attempt to capture most common best practices to achieve these goals, Committers may take the action most appropriate to specific circumstances even when it differs from this document.  In these cases, the Committer deviating is expected to explain the deviation if asked, and suggest updates to this document if similar circumstances are expected to occur regularly in the future.

## Review priority

A Committer should generally give highest review priority to the oldest pull request that is ready to review when not otherwise motivated by a higher-priority task that suggests reviewing newer pull requests supporting that task.  A pull request is ready to review when all of the below are true:

* The pull request is marked ready for review (rather than marked as draft)
* There are no unaddressed comments from reviewers
* There are no conflicts with the main/master branch (this is indicated at the bottom of the PR when it is the case)

For pull requests that are not yet ready to review, a Committer may provide nudges to the Contributor regarding what is necessary to make the pull request ready to review if the Committer believes that may be helpful.  A Committer may review a pull request that is not yet fully ready for review, but this is not expected. 

## Threshold for approval

In general, reviewers should approve a pull request when it improves the repository and does not hurt/regress the repository.

### Considerations

Things to consider when evaluating whether a PR would hurt/regress the repository:

#### Continuous integration tests

Failure of any of these should usually indicate the PR is not ready for approval.  If a CI test is incorrect, it should generally be fixed in an earlier PR, or fixed in the same PR -- new or updated functionality, even if it is correct, should not be merged with a failing CI result.

#### Compliance to style guides

Substantive non-compliance with established style is likely to lower the quality of the repository.  If a style guide is overly or inappropriately prescriptive, the style guide should generally be updated.

#### Verification

If the behavior of new or changed functionality has not been verified (either manually or via CI), the risk of the changes being incorrect is correspondingly high.  If the behavior is not verified via CI, there is an increased risk of introducing bugs or regressing functionality in the future.  If the new or changed functionality is not covered by CI, the PR author should generally be expected to indicate why they are confident the behavior is correct (e.g., "manually tested against a fully-deployed system").

#### Maintainability

Code is generally written once but read, used, and updated many times.  Code that makes reading, using, and/or updating appreciably harder than it could be should generally be optimized toward better maintainability.

#### Consistency with strategy

Sometimes, there are multiple reasonable approaches to solve a particular type of problem.  If InterUSS has already established a preferred approach, that approach should be favored over the other reasonable approaches to minimize the total complexity of the overall project (see Maintainability also).

InterUSS also has a number of strategic objectives (arising from the Project Charter, Technical Charter, governing board, technical steering committee, and other sources) and changes contrary to these objectives are likely to lower the quality of the affected repository when judged according to InterUSS objectives.

### Completeness

There is no expectation for "completeness" in a pull request -- Contributors are [encouraged](repo_contributions.md#general-principles) to make small PRs, so approval should not usually be withheld because the reviewer would like something added to the pull request.

The addition of something to a pull request can be suggested, however, when that addition would resolve another issue that would hurt/regress the repository if the pull request were merged without it (e.g., lack of verification).

### Consensus

When multiple reviewers provide feedback on a pull request, all issues raised by all Committer reviewers must generally be addressed or retracted before merging a pull request in order to achieve [consensus](README.md#making-decisions).  In other words, each Committer initially has veto power over a pull request, but that veto power can be overridden according to the TSC-specified means of [making decisions](README.md#making-decisions).

### Approval with comments

When a reviewer approves a pull request yet simultaneously leaves comments, this is an indication that the reviewer is confident the contributor will resolve the comments to the reviewer's satisfaction, and the reviewer believes the pull request will be ready to merge once the contributor has addressed all comments.

## Comments and feedback

When a reviewer believes a pull request is not ready to merge, they should, to the extent practical, clearly communicate everything that is needed for the reviewer to approve the pull request.  The reviewer does not need to identify all specifics; for instance, the reviewer may indicate that they are concerned about maintainability (which communicates that resolving all maintainability concerns would prompt the reviewer to approve the pull request) without needing to identify all specific maintainability issues.  However, the reviewer should be prepared to elaborate upon request of the contributor if the contributor does not understand what they must do to obtain the reviewer's approval.

If there are only a few specific issues, then commenting on those issues specifically should generally imply that if those issues were fixed and no new issues were introduced, then the reviewer would be willing to approve the pull request.  If there are a number of issues of the same type, a reviewer may only identify one or a few instances of the issue and note that other instances should be corrected as well.  If a reviewer identifies a number of specific issues but there are also additional broader issues that would prevent approval even if all specific issues were resolved, the reviewer should indicate this situation in an overall comment on the pull request along with their specific comments.

The above guidance should not prevent a reviewer from raising additional issues on later review that they missed on earlier reviews, but reviewers should strive to minimize these cases.

### Too large for effective review

If a reviewer feels that a pull request is too large for the reviewer to provide the feedback above or otherwise qualifies as a [large contribution](repo_contributions.md#large-contributions), they should communicate this to the contributor and refer to the [large contributions](repo_contributions.md#large-contributions) guidance which recommends splitting large contributions into smaller self-contained pull requests or else having an initial discussion, design document, and work/PR breakdown for the large contribution.  Usually this decision and communication should happen prior to any other review, and the reviewer should wait to review further until a smaller PR or the large-contribution framework (discussion, design doc, etc) is available.

### nits

When a reviewer wants to note a minor issue or opinion, they can prefix their comment with "nit: ".  No nits should prevent approval; if a contributor has addressed all non-nit issues yet chooses not to address any nit issues after considering them, the reviewer should still approve the pull request.

# CockroachDB migration

## Overview

Cockroach Labs announced a change in licensing terms for new CockroachDB (CRDB) versions at the end of 2024 that is not consistent with InterUSS's goal of promoting easy adoption of UTM services: companies meeting certain criteria would be required to pay fairly large license fees merely to deploy CRDB nodes themselves according to the InterUSS design.  The technical steering committee investigated possible responses to this change, and this page describes that investigation.

## High-level options

[CockroachDB mitigation approaches](./[InterUSS]%20CockroachDB%20mitigation%20approaches.pdf) identifies the constraints/goals to use when evaluating potential approaches, lists a number of high-level approaches, and identifies some pros, cons, risks, and unknowns for each approach.

## Application-level consensus

Historically, the primary purpose of using CRDB was to leverage Cockroach Labs technology to perform synchronization between DSS instances to fulfill, e.g., ASTM F3548-21 DSS0215 ("DSS implementations shall only respond to the USS after the transaction has been recorded in the DAR.")  Such multi-organization distributed systems are hard to design, build, and keep bug-free, but InterUSS was able to solve this problem by simply having each DSS instance host run a CRDB node configured to connect with the CRDB nodes of other DSS instances, and then write to the conceptual unified CRDB database in order to accomplish the required cross-instance synchronization and consistency.  One challenge of this approach, however, was to explain the two different functions CRDB was accomplishing: local storage for the individual DSS instance, and synchronization of information between DSS instances.

"Application-level consensus" clearly separates the two concerns of local storage and synchronization between instances by implementation synchronization between DSS instances at the application level rather than using a premade product to accomplish that synchronization implicitly.  This approach has many advantages (see link in [High-level options](#high-level-options)), but the largest risks are resolving or mitigating the unknowns.  [DSS: Application-level consensus](./DSS%20Application-level%20consensus.pdf) contains a conversation attempting to answer some of the key questions for an application-level consensus solution.

### Consensus algorithm

Designing a distributed consensus algorithm is certainly beyond the core competencies of InterUSS, so we attempted to evaluate existing algorithms for suitability.

#### CAS Paxos

One potential algorithm candidate highlighted as promising was CAS Paxos.  However, it appeared that CAS Paxos had a big moment in 2018 when it was proposed when many people played around with the idea (e.g., [here](https://github.com/gryadka/js)), but we were unable to find much of anything after that -- the GitHub repos relating to CAS Paxos all seemed to be last modified ~5 years ago. Raft, the algorithm underpinning CRDB and many other similar systems, was introduced in 2013 and by 2019 there were multiple competing companies providing highly-successful commercial databases leveraging its advantages.  The absence of a similar phenomenon with CAS Paxos suggests that others have judged this algorithm to not have sufficient advantages over Raft to justify pursuing it.  It could be that our use case is uniquely suited to CAS Paxos as compared to general databases, but we were unable to identify a reason that would be the case.  Combined with the fact that there does not appear to be any existing CAS Paxos library we could leverage and that Paxos is famous for performance and correctness being dependent on implementation specifics, we believe the choice of CAS Paxos would very likely violate Must-Have 3: DSS implementation can be developed and reliably maintained by the available InterUSS contributors.

#### Raft

The more obvious distributed consensus algorithm choice is [Raft](https://raft.github.io/).  It is the algorithm underpinning CRDB and many other similar systems, including many large-scale systems used in critical production settings.  There is [an open-source library in Go](https://github.com/etcd-io/raft) used in widely-deployed production systems that InterUSS could incorporate to avoid attempting full implementation of a distributed algorithm ourselves.  This approach seemed promising enough for us to [attempt to develop a full design document](./InterUSS%20DSS%202024%20redesign.docx) on how a Raft-based application-level-consensus DSS implementation would work.  This document resolved a number of problems including, hypothetically, how queries could be scaled horizontally for performance in nominal cases by geographical sharding.  However, many additional unknowns were raised and the document did not achieve consensus on some core questions including the nature of storage for the consensus engine.

To attempt to make the discussion more concrete and mitigate some of the risk of not having extensive experience with the Raft library intended for this design, we attempted to build a "toy DSS": actually-executable code that accomplished the core functionality of the DSS in an illustrative manner.  The following characteristics were targeted for a working toy DSS:

1. Uses intended etcd Raft library
2. Instances can be run in different processes and synchronize with each other
3. A client can set some key to some value on any instance and receive a response involving other existing key/value pairs, and this operation only provides a response to the client after the change has been incorporated by the Raft group (via acknowledgement by the majority)
4. Responses to clients remain correct (regardless of instance queried when that instance hasn't failed) at all times under standard failures/situations tolerated by Raft:
    1. One other instance stopped
    2. Instance stopped and restarted (long-term memory retained)
    3. Instance stopped and restarted (long-term memory lost)
    4. New instance added to Raft group
    5. Other instance removed from Raft group (with >3 members)

Some unsuccessful attempts to write this toy DSS may be found [here](https://github.com/BenjaminPelletier/toydss), however none of them were successful.  One important factor was that a number of elements had been factored out of the core etcd repository into the raft library, but there was no release of the core etcd repository reflecting this factoring so the most recent raft library could not be easily used with the core etcd repository as another library (providing, for instance, rafthttp which it would not be useful to reproduce independently in the toy DSS implementation).  The fact that this refactoring was not yet even released suggested that the library may not be as mature as previously expected.  Furthermore, the inability of one developer to produce a toy DSS implementation meeting the above criteria suggested the Raft library may not be quite as easy to use as previously expected.

The biggest risk of the application-level consensus approach [identified early on](#high-level-options) was that this approach may be too big of a project to take on at this point.  Because we failed to mitigate this risk with a clear design document and proof-of-concept toy DSS, we decided we could not select this option at this time.  We believe this decision could be revisited in the future if and when a contributor is able to bring a working toy DSS meeting the criteria above, as that would let us continue to make progress toward a concrete design document that addresses the known-unknowns.

## YugabyteDB

The alternative of swapping out CRDB for [YugabyteDB](https://www.yugabyte.com/) was explored and all known-unknowns were resolved positively (none of resolutions of those unknowns would present serious difficulties to taking this approach).
YugabyteDB offers a similar feature set as CockroachDB with an Apache 2 license similar to the InterUSS products. In addition, current paradigms, used for instance for pooling, can be mostly preserved and it should not be a big stretch for users to swap technologies.
At this point, we do not foresee the need to provide support for migrating the data from one technology to the other. The assumption is that current deployments will have to repopulate the new DSS when switching.

Yugabyte proposes some key improvements compared to CockroachDB such as the ability to reload certificates without stopping nodes limiting the development and operational efforts to perform updates of DSS pool participants accepted CA certificates.

Though, some indexation mechanisms such as the inverted index are still not fully implemented. It will require some rework of SQL schemas and queries to ensure the performance of the DSS is comparable.

## Decision

Because of our inability to sufficiently mitigate the main known risks of the application-level consensus approach and relatively low-risk, low-effort, and high performance attributes of the YugabyteDB-swap approach, we have currently decided to prioritize the YugabyteDB-swap approach as the preferred development path at this point.

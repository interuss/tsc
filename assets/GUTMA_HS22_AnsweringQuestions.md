# Transcript: Answering Your Questions about Automated Testing
*Presented at GUTMA Harmonized Skies 2022*

[Slides](GUTMA_HS22_AnsweringQuestions.pdf)

## Slide 1
Thank you for your time to attend this presentation; I’m going to try to answer some of your questions about automated testing with the InterUSS Platform’s open-source test suite.

## Slide 2
I’m Ben Pelletier, currently serving as the chairperson of the technical steering committee of the InterUSS Platform.  I’m the architect of some of the automated testing functionality and have helped coordinate a number of demonstrations using InterUSS technology.

## Slide 3
The InterUSS Platform is a project of the Linux Foundation which provides governance for collaborative open-source projects.  Basically, it’s an independent place where industry can come together to collaboratively develop UTM software.  We develop software to support simple, secure, and scalable interoperability between UAS service providers to advance safe and efficient drone operations.  One of our software products is automated testing geared toward this purpose.

## Slide 4
The motivation for automated testing starts at the fact that an authority, like the FAA or EASA or an EU member state, needs to verify that an applicant complies with all the necessary requirements before granting authorization to operate.  Today, this is usually accomplished by manual checkouts.  The authority will set a time period (sometimes weeks long) where the capabilities and behavior of the applicant will be verified by executing some known set of tests with some expected set of outcomes.  But, there are a number of problems with this approach.  Especially in checkouts where multiple service providers need to interoperate to satisfy the necessary requirements, problems are often discovered during the checkout because manually coordinating testing time is difficult, so the service providers haven’t had the opportunity to rigorously test with all the other service providers prior to the checkout.  Even outside interoperability, manual checkouts are difficult and expensive because they usually involve a large amount of human time and cross-organization coordination.  Automated testing accomplishes the same objectives as a manual checkout, but is far easier and less expensive, which makes it possible to conduct them frequently to enable more frequent updates and to detect regressions sooner.

## Slide 5
I want to describe how automated testing works, but first let’s look at how a manual checkout works.  A regulator wants to know if a black box system works, so they have a test director conduct tests.  For instance, the test director might tell the black box system to create a flight and then the test director might ask the black box system whether the flight is shown on the screen.  Automated testing does this...

## Slide 6
...by simply replacing the test director with a computer.  Now the computer is instructing the black box to (for example) create a flight, and asking whether the flight is visible.

## Slide 7
Of course, it wouldn’t be a good idea to have the computer (the “automated test driver” in this diagram) verbally request for things to happen or ask what is observed by the system, so we replace those verbal instructions with simple, standard RESTful web endpoints.  This allows the automated test driver to actuate and sense the system through these blue-line web interfaces without knowing anything about the specifics of the system on the other side of them.  Meanwhile, the service provider’s system is in the black dotted line over here, and it needs to be able to respond to the automated test driver’s requests to do something to the system or observe something about the system.  That means a service provider that wants to be tested by the InterUSS automated testing framework needs to implement these red boxes – they listen for the automated test driver requesting an action or asking for information, and then respond appropriately according to how the system under test works.  Right now, these interfaces are specific to InterUSS, but we hope to develop them into standard interfaces in the next versions of the appropriate ASTM standards.

## Slide 8
Here we have an illustration of a system performing remote ID; the service provider on the left has information about drones, and the display provider on the right wants to provide that information to its end user.  InterUSS’s automated test driver tool named uss_qualifier tests this system by first using the injection interface to ask the service provider on the left to pretend as if one of its users were flying a drone that was reporting the provided telemetry.  Then, uss_qualifier uses the observation interface to ask the display provider on the right which flights one of its users would see if they looked at a particular area.  If the requested flight is “seen”, then the test passes.

## Slide 9
InterUSS designs automated test suites to verify a set of requirements.  If we have the list of requirements that need to be tested for compliance on the left, then we try to design a set of simple test scenarios, like those on the right, that covers all the necessary requirements.

## Slide 10
InterUSS’s flexible approach in constructing test suites means tests can be customized on a per-country basis and can support specific review processes.  Creating test suites in an open source project builds on aviation’s history of working together to improve safety by enabling the community to propose updates.  If a problem is discovered even though a system passed all the existing tests, a scenario can be added or updated to detect that problem and then the entire industry as well as the regulator benefits from that improvement.

## Slide 11
The main artifact produced by a uss_qualifier run is a test report in JSON format.  It contains a top-level indication of success that can be a simple, clear prerequisite for promoting changes into production.  It also records the precise version of the codebase, test suite, and configuration used for the test run to ensure strong traceability.  When tests fail, it includes detailed information about exactly what happened leading up to the failure and what was measured that was determined to indicate failure.  Various tools to summarize this really long report are also included, and more can be developed to suit specific user needs should they arise.

## Slide 12
This slide illustrates where the InterUSS open source test suite could be used in a deployment process.  The most important part is the set of external environments on the right.  To enable effective interoperability testing between multiple participants, a semi-persistent shared partner qualification environment can be used to run uss_qualifier prior to promoting a change to production.  This corresponds really well to the test environment required by both ASTM standards related to federated UTM.  Next slide.

## Slide 13
Even though this automated testing development is relatively new, we already have a number of case studies where it has been used to test multiple participants, and we’re excited to add to this list.

## Slide 14
I’ll close by encouraging you to try out uss_qualifier for yourself if this technology sounds interesting to you.  If you already have Docker installed on a Posix system, it should only take a few commands to bring up and test a full mock UTM ecosystem on your local machine.  Other systems including Windows should work as well with appropriate supplemental tools like a bash interpreter.  We would love to have more contributors so please reach out via the mailing lists or Slack if you have any questions or comments.  All of the information I’ve presented here, along with instructions for running uss_qualifier yourself, and contact information for further discussion, can be found at the web site links below. Thank you for your time.

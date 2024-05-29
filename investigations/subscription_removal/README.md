# ASTM F3548-21 automatic subscription removal

## Purpose

ASTM F3548-21 A2.7.2(4) says:

> [...] the end time for a subscription governs when the DSS automatically removes it from the DSS. Tests must demonstrate that automatic removal of subscriptions occurs on all DSS instances in the DSS pool.
 
A number of edge cases complicate the implementation of this test.  This document describes discussions related to implementation of a test for this requirement.

## Complexities

Unlike subscriptions, ASTM F3548-21 does not specify automatic removal of operational intents that lie entirely in the past.  During standard development, this choice was justified to provide conservative USSs with additional information.  Consider a double failure in USS 1: USS 1's aircraft overruns its operational intent volumes by time (it is still flying after the last volume's end time), and USS 1 is unable to communicate this information via the UTM system.  In this case, a conservative USS 2 can detect the potential for this problem by searching the DSS for recent but expired operational intent references.  Therefore, expired operational intents (ones where the latest volume end time is in the past) are not automatically removed from the DSS.

Some operational intents are required to be covered by subscriptions (SCD0080).  This means that an implicit subscription created for an operational intent effectively becomes part of that operational intent and it would be questionable to delete a subscription that an operational intent was relying on.  Therefore, it is somewhat difficult the potentially-conflicting requirements to keep a subscription for an expired operational intent (SCD0080) and remove an expired subscription (the requirement above).  A few different options for attempting to achieve compliance to both requirements are explored below.

## Options

### Option 1: Remove subscriptions without dependencies

In this option, expired subscriptions that are not dependants of any operational intent are garbage-collected from the system (removed entirely from the database).  Expired subscriptions that are dependants of an overdue operational intent are not deleted from the database (because they are an essential component of the still-existent associated operational intent), but they are filtered so they don't appear in search results.

The implementation of this option would likely involve a periodic garbage collector that fully deletes applicable subscriptions from the system, similar to the mechanism for remote ID.  In the period between when a subscription expires and when it is actually deleted from the database by the garbage collector, the expired subscriptions will still be returned.

### Option 2: Hide subscriptions without dependencies

Option 2 is similar to Option 1, but the subscription is not actually removed from the database; instead, it is just removed from search queries.

### Option 3: Treat dependant subscriptions as active

Option 3 focuses on consistency -- either a subscription is removed or it isn't.  Subscriptions that can be fully removed are removed, but subscriptions that can't be removed remain fully active.

### Option comparison

For all options, any subscription with an end time in the future is visible in all circumstances.  The visibility of other subscriptions under the different options is summarized below.

<table>
    <tr>
        <th></th>
        <th colspan="2">Subscription with end time in the past</th>
    </tr>
    <tr>
        <th>Option</th>
        <th>No op intents depend on subscription</th>
        <th>1+ op intents depend on subscription</th>        
    </tr>
    <tr>
        <td>Option 1</td>
        <td>
            <ul>
                <li>Search subscriptions: Not visible</li>
                <li>Get subscription by ID: Not visible</li>
                <li>Change intersecting airspace: Not visible</li>
                <li>Database: Not visible</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Search subscriptions: Not visible</li>
                <li>Get subscription by ID: Visible</li>
                <li>Change intersecting airspace: Visible</li>
                <li>Database: Visible</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Option 2</td>
        <td>
            <ul>
                <li>Search subscriptions: Not visible</li>
                <li>Get subscription by ID: Visible</li>
                <li>Change intersecting airspace: Visible</li>
                <li>Database: Visible</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Search subscriptions: Not visible</li>
                <li>Get subscription by ID: Visible</li>
                <li>Change intersecting airspace: Visible</li>
                <li>Database: Visible</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Option 3</td>
        <td>
            <ul>
                <li>Search subscriptions: Not visible</li>
                <li>Get subscription by ID: Not visible</li>
                <li>Change intersecting airspace: Not visible</li>
                <li>Database: Not visible</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Search subscriptions: Visible</li>
                <li>Get subscription by ID: Visible</li>
                <li>Change intersecting airspace: Visible</li>
                <li>Database: Visible</li>
            </ul>
        </td>
    </tr>
</table>

## Option selection

Because subscription removal is not at all core to any of the primary features or goals of F3548-21 and it appears that an argument could be made for the compliance of any of the above options, Option 2 is selected in the short term as the easiest to implement.  In the longer term, a garbage collector can be implemented to transition to Option 1.

Although Option 3 has the desirable property of being the most straightforwardly consistent, it requires non-trivial determination of what subscriptions are "active", and this determination activity is only useful in edge cases that are expected to be rarely encountered and intended even more rarely.

- Start Date: 2012-8-13
- Relevant Team(s): Product Eng, Provisioning
- RFC PR: 
- Status: Initial Draft

# Improve provisioning success rate.

## Summary

Provisioning process takes longer time to fail because it get stuck at one of provisioning event. Example: provisioning stops at this event "Connected to magic install system" and provisioning fails only after 25 mins.

## Motivation

To improve the provisioning success rate and performance with alternatives.

## Detailed design

Following are two approaches:

1. Check if all provisioning events has wait timeouts. Each event should wait for limited time to get success/failure response. If one of event fails then retry atleast two times before returning/exiting.

Pros: 
    - Failures are detected at earlier stage and user does not have to wait for 25 mins for initial errors. This improves provisioning performance.
      
Cons: 
    - Not sure if provisioning events can handle timeouts.
    - Time estimate to implement this approach is not known yet

## Alternatives

Provision two servers with same configuration at same time. If provisioning was success on two servers, deprovision any one of two servers. If one of servers fails to provision, deprovision failed server right away and continue with provisioned success server.

Instead of provisioning two servers at same time, We can start provisioning one server and if provisioning fails then we start provisioning new server with same configuration. In this case user is aware of failed server and starting new server but does not need user intervention for this process.

Pros:
    - Provisioning success rate is higher.
    - Provisioning performance is better.

Cons:
    - Expect to have more hardwares in facility than required for provisioning.
    - Currently during provisioning, user can see server IP address and server console at intial phase of provisioning. This can be confusing for user if provisioning failed and we switch to different server.
    - To prevent more hardware usage, deprovision process need to be faster.

## Unresolved questions

> Optional, but suggested for first drafts.
> What parts of the design are still TBD?


# Design for provisioning alternative nodes on failure

- First Proposed:  2021-08-25
- Authors:
  - Raj Dharwadkar
- Relevant Team(s): Product Engineering, Provisioning
- Status: [ Draft ]

## Summary

Primary reasons for provisioning failures may be hardware, system, or service failure. Making provisioning workflow reliable, even in the face of an unreliable environment.

## Background

The user sees more provisioning failures in unreliable environments. On any provisioning failure, there is no timeout associated with each provisioning event so the user has to wait 25 minutes to see provisioning failure.

## Scope

- Improve user provisioning success rate by providing provisioning retry on failure. 
- Make the user's provisioning experience better and improve provisioning performance, by providing faster response time from each provisioning event.

### Goals

- Increase provisioning success rate.
- Make provisioning performance better.
- Changes in provisioning success rate and performance can be tracked in the grafana provisioning dashboard.

### Non-Goals

- Handle repeated provisioning failure with the same configuration, if provisioning continues to fail on new hardware after switching then provisioning workflow does not continue with the next hardware.

### Detailed design

Provisioning one server at a time, In case of provisioning failure, automatically switch to new hardware and start provisioning with the same configuration and deprovision failed server.
We should note that users are aware of provisioning failure and switching to new hardware because they had access to the server IP and console at the initial phase of provisioning.

Additional:
To prevent users from waiting for 25 minutes on provisioning failure, check all provisioning events have a wait timeout associated with them so that the user sees failure early and quickly. To improve wait timeout, events should retry 2 or 3 times before returning/exiting.

## Drawbacks

- Additional hardware must be available in case of failure.
- Provisioning may take a longer time on failure, as we switch to other hardware to retry provisioning.

## Alternatives Considered

To decrease provisioning time in the first approach, we can provision two servers at a time with the same configuration. On successful provisioning of any one of two servers, the failed server can be deprovisioned. Deprovision workflow should be modified to make it faster to prevent more hardware usage.

## Drawbacks

- Almost twice the number of hardware is required in the facilities, as we provision two servers at a time over a short period.
- Provisioning failure may increase if there is a shortage of hardware in the facilities.

## Unresolved questions

> This section is optional but recommended for first drafts.
> What parts of this design have not yet been finalized?

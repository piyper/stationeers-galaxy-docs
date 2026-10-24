---
layout: page
title: Frequently Asked Questions
permalink: /faq/
---

# Frequently asked questions

## Is a Galaxy one large Stationeers map?

No. It is a coordinated group of real Stationeers worlds. Each world has its own save and simulation.

## Do all worlds have to run on the same computer?

No. Worlds can run on different hosts. Each world connects outward to the same Coordinator.

## Does every world require an open Galaxy port?

No. A world needs its ordinary Stationeers game endpoint for players, but its Galaxy control connection is outbound.
Only the Coordinator needs a publicly reachable Galaxy HTTP(S)/WebSocket endpoint.

## Can hostnames be used instead of IP addresses?

Yes. A world's public endpoint can use a hostname, provided DNS resolves it and players can reach its Stationeers game
port.

## Can each world have a different idle timeout?

Yes. Managed-world settings, including idle shutdown, are per world. Always-on and externally managed worlds do not
need the optional Host Manager.

## What happens if a destination is offline?

The source remains the committed residency until the destination confirms arrival. A failed attempt is discarded and is
not silently retried later. A managed destination may be started on demand before the connection attempt.

## Does Galaxy delete body bags when I enter a world?

No. Ordinary body bags are not removed as part of world entry. If an older live copy of the same player exists at the
destination, it is retired through the normal death path so recoverable belongings remain.

## Are Galaxy Logic Transmitter values guaranteed to arrive?

No. They are live state updates with no queue, acknowledgement, retry, or replay. Use the `Error` value and
design automation to tolerate a missed update.

## Do Dimensional Vending Machines duplicate items?

Each machine displays synchronized representations of one logical inventory. Vending is transactional: the other
copies are removed before the selected logical item is exported.

## Can someone forge an enrollment credential from public code?

No. Public code explains the protocol but does not contain a world's generated secret. Protect the claimed credential,
use HTTPS, and rotate or revoke a credential if it is exposed.


---
layout: page
title: Security and Privacy
permalink: /security/
---

# Security and privacy

Galaxy is designed so public source code and public documentation do not act as credentials.

## World authentication

Each world receives its own credential by claiming a one-time enrollment token. Later requests are signed with that
credential. Credentials can be rotated or revoked independently, so compromising one world does not reveal a universal
Galaxy password.

Signed requests include freshness and replay protection. Knowing the source code, world ID, endpoint, or signature
format is not enough to create a valid request without the world's secret credential.

## Outbound connections

Participating worlds and optional Host Managers connect outward to the Coordinator. They do not open an administrative
Galaxy listener for the Coordinator to call.

The Coordinator is the public control endpoint and should be placed behind HTTPS with normal server hardening,
monitoring, backups, and access controls.

## Host Manager boundary

The Host Manager can be configured to report state and start or stop known worlds. It does not accept arbitrary command
lines from the Coordinator. Container targets must carry exact local labels; non-Docker targets are fixed command arrays
defined by the host owner.

## Player identity and travel

The Coordinator uses authenticated world reports and committed residency rather than trusting a client-provided display
name. Pending travel does not replace committed residency. A destination must confirm the player's successful arrival
before the move is committed.

## Public documentation policy

Safe public documentation may include architecture, feature behavior, normal installation steps, public endpoints, and
examples with fictional names.

Keep these private:

- live credentials and enrollment tokens;
- administrator keys;
- private host addresses and management endpoints;
- unredacted logs containing secrets;
- production deployment inventories;
- recovery material and backups.

If a secret appears in public history, rotate or revoke it. Merely deleting the current copy does not remove it from
Git history or existing clones.


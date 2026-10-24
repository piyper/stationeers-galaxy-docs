---
layout: page
title: Player Guide
permalink: /player-guide/
---

# Player guide

## Joining a Galaxy

A Galaxy has one public entry address. You do not normally need to know the address of every world inside it.

When you connect, Galaxy checks where your active character belongs:

- If the entry world is your current world, you continue there.
- If your character belongs to another available world, Galaxy can direct you to that world.
- If a destination is temporarily unavailable, Galaxy must not silently commit you to it or repeatedly retry an old
  attempt.

The exact Continue and Respawn interface may evolve during development. Treat Respawn as a destructive choice: it is
for deliberately retiring the existing Galaxy character and starting again at the Galaxy's configured entry world.

## Travelling

Travel may be initiated by a Transporter or by other Galaxy-aware travel systems. During inter-world travel, a Galaxy
Transit screen identifies the source and destination and may show that the destination is starting.

Travel follows a transaction:

1. Galaxy checks the route and destination.
2. The destination prepares the arrival.
3. Your client connects to the destination.
4. The destination confirms that you arrived successfully.
5. Only then does Galaxy commit the new residency and clean up the old source character.

If preparation, authentication, connection, or admission fails, your committed residency remains at the source. A
failed or cancelled attempt is not automatically tried again later.

## Password-protected destinations

An independently hosted world may still use its own Stationeers password. Native Stationeers password entry remains in
control of that check. A wrong password prevents the travel transaction from committing.

Galaxy-wide authentication and a particular world's native password are separate concepts.

## Inventory and old bodies

Inventory-preserving travel carries the character package to the destination as part of the transaction. If an older
live character for the same player already exists at the destination, Galaxy retires that older character through the
normal death path so its recoverable belongings remain available. Galaxy entry cleanup does not delete ordinary body
bags.

Some forms of travel intentionally create a normal or fresh spawn instead of preserving the current character. The
travel interface should identify that distinction before activation.

## Galaxy chat

When enabled by the Galaxy operator, ordinary Stationeers chat is relayed to the other connected worlds. Messages still
appear through the native chat experience; players do not need a separate chat client.

Only currently connected worlds receive a message. Galaxy chat is live relay, not durable mail or chat history.


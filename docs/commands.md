---
layout: page
title: Galaxy Console Commands
permalink: /commands/
---

# Galaxy console commands

Galaxy provides a small player-facing command set in the Stationeers console. Enter `galaxy help` at any time to see
the available commands.

## Status

`galaxy status` shows whether Galaxy networking is available, the current Galaxy world, and the most recent travel or
network result.

## Worlds

`galaxy worlds` requests a current world list and displays the known Galaxy worlds as online or offline. If no world
list has arrived yet, run the command again after the first request completes.

## Give up a world and return to the Gateway

`galaxy return` displays a warning but does not move the player.

`galaxy return CONFIRM` abandons the current character and inventory, resets Galaxy survival progress, and respawns
the player in the default Gateway world. It works without a Transporter so that a player can never become permanently
trapped on a Galaxy world.

This is a destructive respawn, not inventory-preserving travel. Use a destination pad when you want to carry your
character and inventory to another world.

Developer and diagnostic commands are not part of the public player command set.

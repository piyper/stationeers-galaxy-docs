---
layout: page
title: What is Stationeers Galaxy?
permalink: /overview/
---

# What is Stationeers Galaxy?

Stationeers normally treats every server and saved world as a separate place. Stationeers Galaxy connects a group of
those worlds so they behave more like locations in one larger universe.

A Galaxy can contain a gateway station, planets, moons, orbital facilities, or specialized community worlds. Each
world still runs as a real Stationeers server with its own save, environment, structures, and simulation.

Galaxy coordinates the parts that need to cross those server boundaries:

- player residency and transactional travel;
- shared native chat between connected worlds;
- a live directory of available worlds and transport destinations;
- Galaxy Logic Transmitters for cross-world automation;
- Dimensional Vending inventories shared between worlds;
- optional world startup and safe idle shutdown.

## Separate worlds, one Galaxy

Galaxy does not combine maps into one giant simulation. Every world remains independent. When a player travels, the
system coordinates the source and destination so the player is not committed to the new world until arrival succeeds.

This separation has useful consequences:

- every world can have its own terrain, atmosphere, rules, difficulty, and host;
- a world can be restarted or maintained without stopping the entire Galaxy;
- worlds may be always online or started only when someone needs them;
- the Galaxy can grow across multiple physical hosts.

## The Coordinator

The Galaxy Coordinator is the server-authoritative service connecting the worlds. Each participating world opens one
authenticated outbound connection to it. Individual worlds do not need to expose a Galaxy callback port.

The Coordinator tracks world availability, player residency, pending travel, shared directories, cross-world chat,
logic relay, and dimensional-vending transactions.

## Transactional travel

Travel is designed around one important rule: **the source remains authoritative until the destination confirms a
successful arrival**.

Preparing or attempting a journey does not move committed residency. If admission fails, a password is rejected, the
destination goes offline, or the player cancels, the pending attempt is discarded. Galaxy does not save an incomplete
journey for an automatic retry.

After the destination confirms arrival, residency moves to the destination and the source is asked to perform its
cleanup. These operations are designed to be safe to repeat if a network interruption occurs.

## Optional companion items

Some features are separate Stationeers mods that can also work outside a Galaxy:

- The **Transporter** provides programmable travel between pads, saves, servers, and Galaxy destinations.
- **Dimensional Vending** shares same-named vending inventories locally and expands that inventory across worlds when
  connected to Galaxy.

The Galaxy gameplay mod adds the cross-world services and the Galaxy Logic Transmitter.


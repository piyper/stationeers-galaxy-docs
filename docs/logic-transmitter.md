---
layout: page
title: Galaxy Logic Transmitter
permalink: /logic-transmitter/
---

# Galaxy Logic Transmitter

The Galaxy Logic Transmitter lets Stationeers automation on one world provide a logic value to another world. An IC10
program can therefore observe a value produced on another planet and use it in local control logic.

It is a separate Galaxy item. Ordinary Stationeers Logic Transmitters retain their normal behavior, and Galaxy does not
change the IC10 language.

## Active and Passive units

Galaxy transmitters use the familiar native programming model:

- An **Active** transmitter publishes its `Setting` value.
- A **Passive** transmitter selects one Active Galaxy transmitter and mirrors the received value into its own
  `Setting`.

Use the Passive unit's selection screw to cycle through Active Galaxy transmitters on connected worlds. Friendly world
and device names are shown when available. When a transmitter is selected, the Passive unit requests its current value
once, so the source does not need to change before the initial value appears.

## Logic interface

The important values are:

| Logic value | Active transmitter | Passive transmitter |
| --- | --- | --- |
| `Setting` | Read/write value to publish | Most recently delivered value from the selected Active unit |
| `Error` | Reports Galaxy availability | `0` when the selected endpoint is available; `1` when its world is paused, offline, or otherwise unavailable |

An IC Housing, Logic Writer, or another compatible Stationeers logic device can write `Setting` on an Active unit.
Logic Readers and IC10 can read the Passive unit's `Setting` and `Error` values.

## Delivery model

The relay is deliberately lightweight:

- a distinct changed value is sent once;
- selecting an Active transmitter requests its current value once;
- the Coordinator forwards it once to connected worlds in the same Galaxy;
- values are not queued, retained, acknowledged, retried, or replayed;
- a world that is paused or offline does not receive changes sent while it was unavailable;
- rapid automation should use `Error` and tolerate a missing update.

When the selected source world resumes or reconnects, the Passive unit requests its current value once. Changes made
while the world was unavailable are not replayed as a history; the receiver is refreshed to the source's current state.

This avoids creating cross-world handshake traffic for every logic change. It also means the transmitter is appropriate
for current state, telemetry, targets, and commands that can safely be sent again—not for guaranteed event counting,
financial transactions, or safety interlocks that require delivery confirmation.

## Timing

The network relay itself is normally fast, but the visible response can be limited by Stationeers' scheduled logic
reader/writer cadence. In testing, cross-world delivery reached the destination transmitter in a few dozen
milliseconds, while a display driven by ordinary logic components could appear to update roughly one second later.

## Example uses

- Send a temperature or pressure target to another base.
- Indicate that a remote production line is enabled.
- Select a remote operating mode.

For important commands, design the receiving IC so repeating the same desired state is harmless.


---
layout: page
title: Transporter Overview
permalink: /transporter/
---

# Transporter overview

The Transporter is a powered, programmable pad for moving a player locally or handing them off to another save,
server, or Galaxy world.

The current Transporter uses a Transporter Control motherboard installed
in a powered console. The card selects a transporter on the console's data network and programs or operates that pad.

## Current destination families

The current implementation supports these destination families:

- another Transporter on the same world;
- another local save;
- an external server entered directly;
- a server selected from Stationeers' discovered server list;
- a Galaxy world using a normal/new spawn;
- a specific Transporter on another Galaxy world while preserving the travelling character;
- Galaxy respawn, which resets your character and places you in the default Galaxy world. Players can also use
  [`galaxy return CONFIRM`]({{ '/commands/' | relative_url }}) when no Transporter is available.

The operation screen distinguishes inventory-preserving teleport-style travel from destinations that create a normal or
new spawn.

## Power and automation

The pad must be powered and switched on before activation. It exposes Stationeers logic values so a console, Logic
Writer, or IC10 program can configure and activate it.

Repeated activation is edge-triggered and locked while a journey is in flight. After arriving on a pad, the player must
step off before that pad can automatically transport them again. These safeguards prevent logic loops and duplicate
travel requests.

## Documentation status

The Transporter interface is still changing. This page intentionally gives only an overview until the console screens,
card workflow, and destination controls settle. Do not rely on older instructions that describe programming a
destination in the Transporter's custom label.


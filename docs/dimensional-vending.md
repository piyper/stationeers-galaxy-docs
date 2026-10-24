---
layout: page
title: Dimensional Vending
permalink: /dimensional-vending/
---

# Dimensional Vending

Dimensional Vending Machines with the same name share one logical inventory. On a Galaxy world, that shared inventory
can extend across connected worlds.

The mod provides small and large variants based on Stationeers' native vending machines. It preserves the familiar
browsing, display, manual vending, and IC-controlled vending experience.

## Creating an inventory group

Give matching dimensional machines the same custom name. The name identifies the shared inventory group.

Small and large machines use separate namespaces. A small and a large dimensional machine with the same visible name
are not treated as one group.

## What is synchronized

Every machine in a group presents synchronized item records in the same logical slot order. This matters for both the
display and automation: equivalent machines can use consistent inventory indices.

The system preserves the packaged item tree, including supported nested contents, instead of synchronizing only a
prefab name and quantity.

## Importing an item

When an item is accepted through a dimensional machine's import operation, the group records it and creates the
corresponding synchronized copies in the other machines.

On a Galaxy-connected group, the import is coordinated transactionally through the Galaxy service. If the transaction
cannot begin or complete, the system must not pretend the shared inventory accepted the item.

## Vending an item

Manual and IC-controlled vending use the same transaction. Before the selected item is exported, the other synchronized
copies are removed so the shared inventory cannot dispense the same logical item twice.

After a successful vend, the item no longer appears in the matching machines.

## Startup synchronization

When a world or machine joins an existing Galaxy inventory group, it first synchronizes with the authoritative Galaxy
registry. During that window, the machine may report that its dimensional inventory is synchronizing rather than allow
an unsafe import or vend.

A paused or offline world is excluded from inventory transactions while it is unavailable. The authoritative Galaxy
inventory remains stored by the Coordinator. When the world resumes, the Coordinator pushes the current snapshots to
it; machines that load afterward also synchronize when they register.

## Practical uses

- Put supplies into a gateway warehouse and retrieve them from a planetary base.
- Create deep-mining locations on any Galaxy world and make that inventory available everywhere.
- Share replacement parts between several stations.
- Build an automated cross-world materials buffer.
- Browse the same logistics inventory from multiple facilities.

Use distinct, descriptive machine names for separate inventory groups. Renaming a machine changes the group it belongs
to; it is not a safe substitute for vending or moving the group's existing contents.


**Stationeers Galaxy** is a system that connects multiple Stationeers worlds into one
coordinated multiplayer experience.

Galaxy gives separate worlds a shared sense of place. Players can move between worlds, communicate across the Galaxy,
send logic values between planets, and use inventories shared by dimensional vending machines.

## Start here

- [What is Stationeers Galaxy?](#what-is-stationeers-galaxy)
- [Joining and travelling through a Galaxy](#player-guide)
- [Ways to travel between worlds](#ways-to-travel)
- [Galaxy console commands](#galaxy-console-commands)
- [Galaxy Logic Transmitter](#galaxy-logic-transmitter)
- [Dimensional Vending](#dimensional-vending)
- [Transporter overview](#transporter-overview)
- [Hosting a Galaxy world](#hosting-a-galaxy-world)
- [Security and privacy](#security-and-privacy)
- [Frequently asked questions](#frequently-asked-questions)

---

## What is Stationeers Galaxy?

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

### Separate worlds, one Galaxy

Galaxy does not combine maps into one giant simulation. Every world remains independent. When a player travels, the
system coordinates the source and destination so the player is not committed to the new world until arrival succeeds.

This separation has useful consequences:

- every world can have its own terrain, atmosphere, rules, difficulty, and host;
- a world can be restarted or maintained without stopping the entire Galaxy;
- worlds may be always online or started only when someone needs them;
- the Galaxy can grow across multiple physical hosts.

### The Coordinator

The Galaxy Coordinator is the server-authoritative service connecting the worlds. Each participating world opens one
authenticated outbound connection to it. Individual worlds do not need to expose a Galaxy callback port.

The Coordinator tracks world availability, player residency, pending travel, shared directories, cross-world chat,
logic relay, and dimensional-vending transactions.

### Transactional travel

Travel is designed around one important rule: **the source remains authoritative until the destination confirms a
successful arrival**.

Preparing or attempting a journey does not move committed residency. If admission fails, a password is rejected, the
destination goes offline, or the player cancels, the pending attempt is discarded. Galaxy does not save an incomplete
journey for an automatic retry.

After the destination confirms arrival, residency moves to the destination and the source is asked to perform its
cleanup. These operations are designed to be safe to repeat if a network interruption occurs.

### Optional companion items

Some features are separate Stationeers mods that can also work outside a Galaxy:

- The **Transporter** provides programmable travel between pads, saves, servers, and Galaxy destinations.
- **Dimensional Vending** shares same-named vending inventories locally and expands that inventory across worlds when
  connected to Galaxy.

The Galaxy gameplay mod adds the cross-world services and the Galaxy Logic Transmitter.

---

## Player guide

### Joining a Galaxy

A Galaxy has one public entry address. You do not normally need to know the address of every world inside it.

When you connect, Galaxy checks where your active character belongs:

- If the entry world is your current world, you continue there.
- If your character belongs to another available world, Galaxy can direct you to that world.
- If a destination is temporarily unavailable, Galaxy must not silently commit you to it or repeatedly retry an old
  attempt.

The exact Continue and Respawn interface may evolve during development. Treat Respawn as a destructive choice: it is
for deliberately retiring the existing Galaxy character and starting again at the Galaxy's configured entry world.

### Travelling

Travel may be initiated by a Transporter or by other Galaxy-aware travel systems. During inter-world travel, a Galaxy
Transit screen identifies the source and destination and may show that the destination is starting.

### Ways to travel

Galaxy supports different kinds of journeys between worlds:

- **Rocket travel** lets players make the journey through flight, spending time, fuel, and power along the way.
  Interplanetary rocket travel is under development.
- **Inventory-preserving Transporter ports** carry the player's character and inventory to the destination world.
- **Fresh-respawn Transporter ports** place the player in the destination world as a fresh spawn instead of carrying
  their current character and inventory.

### Travel transaction

Travel follows a transaction:

1. Galaxy checks the route and destination.
2. The destination prepares the arrival.
3. Your client connects to the destination.
4. The destination confirms that you arrived successfully.
5. Only then does Galaxy commit the new residency and clean up the old source character.

If preparation, authentication, connection, or admission fails, your committed residency remains at the source. A
failed or cancelled attempt is not automatically tried again later.

### Password-protected destinations

An independently hosted world may still use its own Stationeers password. Native Stationeers password entry remains in
control of that check. A wrong password prevents the travel transaction from committing.

Galaxy-wide authentication and a particular world's native password are separate concepts.

### Inventory and old bodies

Inventory-preserving travel carries the character package to the destination as part of the transaction. If an older
live character for the same player already exists at the destination, Galaxy retires that older character through the
normal death path so its recoverable belongings remain available. Galaxy entry cleanup does not delete ordinary body
bags.

Some forms of travel intentionally create a normal or fresh spawn instead of preserving the current character. The
travel interface should identify that distinction before activation.

### Galaxy chat

When enabled by the Galaxy operator, ordinary Stationeers chat is relayed to the other connected worlds. Messages still
appear through the native chat experience; players do not need a separate chat client.

Only currently connected worlds receive a message. Galaxy chat is live relay, not durable mail or chat history.

---

## Galaxy console commands

Galaxy provides a small player-facing command set in the Stationeers console. Enter `galaxy help` at any time to see
the available commands.

### Status

`galaxy status` shows whether Galaxy networking is available, the current Galaxy world, and the most recent travel or
network result.

### Worlds

`galaxy worlds` requests a current world list and displays the known Galaxy worlds as online or offline. If no world
list has arrived yet, run the command again after the first request completes.

### Give up a world and return to the Gateway

`galaxy return` displays a warning but does not move the player.

`galaxy return CONFIRM` abandons the current character and inventory, resets Galaxy survival progress, and respawns
the player in the default Gateway world. It works without a Transporter so that a player can never become permanently
trapped on a Galaxy world.

This is a destructive respawn, not inventory-preserving travel. Use a destination pad when you want to carry your
character and inventory to another world.

---

## Galaxy Logic Transmitter

The Galaxy Logic Transmitter lets Stationeers automation on one world provide a logic value to another world. An IC10
program can therefore observe a value produced on another planet and use it in local control logic.

### Active and Passive units

Galaxy transmitters use the familiar native programming model:

- An **Active** transmitter publishes its `Setting` value.
- A **Passive** transmitter selects one Active Galaxy transmitter and mirrors the received value into its own
  `Setting`.

Use the Passive unit's selection screw to cycle through Active Galaxy transmitters on connected worlds. Friendly world
and device names are shown when available. When a transmitter is selected, the Passive unit requests its current value
once, so the source does not need to change before the initial value appears.

### Logic interface

The important values are:

| Logic value | Active transmitter | Passive transmitter |
| --- | --- | --- |
| `Setting` | Read/write value to publish | Most recently delivered value from the selected Active unit |
| `Error` | Standard device behavior | `0` for no error; `1` for any other error; `2` when the selected transmitter is unavailable |

An IC Housing, Logic Writer, or another compatible Stationeers logic device can write `Setting` on an Active unit.
Logic Readers and IC10 can read the Passive unit's `Setting` and `Error` values.

### Delivery model

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

### Timing

The network relay itself is normally fast, but the visible response can be limited by Stationeers' scheduled logic
reader/writer cadence. In testing, cross-world delivery reached the destination transmitter in a few dozen
milliseconds, while a display driven by ordinary logic components could appear to update roughly one second later.

### Example uses

- Send a temperature or pressure target to another base.
- Indicate that a remote production line is enabled.
- Select a remote operating mode.

For important commands, design the receiving IC so repeating the same desired state is harmless.

---

## Dimensional Vending

Dimensional Vending Machines with the same name share one logical inventory. On a Galaxy world, that shared inventory
can extend across connected worlds.

The mod provides small and large variants based on Stationeers' native vending machines. It preserves the familiar
browsing, display, manual vending, and IC-controlled vending experience.

### Creating an inventory group

Give matching dimensional machines the same custom name. The name identifies the shared inventory group.

Small and large machines use separate namespaces. A small and a large dimensional machine with the same visible name
are not treated as one group.

### What is synchronized

Every machine in a group presents synchronized item records in the same logical slot order. This matters for both the
display and automation: equivalent machines can use consistent inventory indices.

The system preserves the packaged item tree, including supported nested contents, instead of synchronizing only a
prefab name and quantity.

### Importing an item

When an item is accepted through a dimensional machine's import operation, the group records it and creates the
corresponding synchronized copies in the other machines.

On a Galaxy-connected group, the import is coordinated transactionally through the Galaxy service. If the transaction
cannot begin or complete, the system must not pretend the shared inventory accepted the item.

### Vending an item

Manual and IC-controlled vending use the same transaction. Before the selected item is exported, the other synchronized
copies are removed so the shared inventory cannot dispense the same logical item twice.

After a successful vend, the item no longer appears in the matching machines.

### Startup synchronization

When a world or machine joins an existing Galaxy inventory group, it first synchronizes with the authoritative Galaxy
registry. During that window, the machine may report that its dimensional inventory is synchronizing rather than allow
an unsafe import or vend.

A paused or offline world is excluded from inventory transactions while it is unavailable. The authoritative Galaxy
inventory remains stored by the Coordinator. When the world resumes, the Coordinator pushes the current snapshots to
it; machines that load afterward also synchronize when they register.

### Practical uses

- Put supplies into a gateway warehouse and retrieve them from a planetary base.
- Create deep-mining locations on any Galaxy world and make that inventory available everywhere.
- Share replacement parts between several stations.
- Build an automated cross-world materials buffer.
- Browse the same logistics inventory from multiple facilities.

Use distinct, descriptive machine names for separate inventory groups. Renaming a machine changes the group it belongs
to; it is not a safe substitute for vending or moving the group's existing contents.

---

## Transporter overview

The Transporter is a powered, programmable pad for moving a player locally or handing them off to another save,
server, or Galaxy world.

The current Transporter uses a Transporter Control motherboard installed
in a powered console. The card selects a transporter on the console's data network and programs or operates that pad.

### Current destination families

The current implementation supports these destination families:

- another Transporter on the same world;
- another local save;
- an external server entered directly;
- a server selected from Stationeers' discovered server list;
- a Galaxy world using a normal/new spawn;
- a specific Transporter on another Galaxy world while preserving the travelling character;
- Galaxy respawn, which resets your character and places you in the default Galaxy world. Players can also use
  [`galaxy return CONFIRM`](#give-up-a-world-and-return-to-the-gateway) when no Transporter is available.

The operation screen distinguishes inventory-preserving teleport-style travel from destinations that create a normal or
new spawn.

### Power and automation

The pad must be powered and switched on before activation. It exposes Stationeers logic values so a console, Logic
Writer, or IC10 program can configure and activate it.

Repeated activation is edge-triggered and locked while a journey is in flight. After arriving on a pad, the player must
step off before that pad can automatically transport them again. These safeguards prevent logic loops and duplicate
travel requests.

### Documentation status

The Transporter interface is still changing. This page intentionally gives only an overview until the console screens,
card workflow, and destination controls settle. Do not rely on older instructions that describe programming a
destination in the Transporter's custom label.

---

## Hosting a Galaxy world

A Galaxy world is a normal Stationeers dedicated world with the Galaxy gameplay mod installed and enrolled with a
Galaxy Coordinator.

### Network model

The world opens an authenticated outbound connection to the Coordinator. The same Coordinator address handles
enrollment and other administrative requests.

The world does not expose a separate Galaxy callback port. Players still need to be able to reach the Stationeers game
server through its normal public hostname or address and game port.

### Enrollment

A Galaxy administrator creates a world slot and gives the world operator a one-time enrollment token. In the dedicated
server console, the operator runs:

```text
galaxy enroll <one-time-token>
```

The world claims the token once and stores its own revocable credential. The token itself is not a permanent password
and cannot be reconstructed from public source code.

Protect the generated credential as a server secret. Do not place credentials, enrollment tokens, or administrator
keys in public configuration examples, screenshots, logs, or documentation.

Host Managers use a separate one-time token. After installing the Host Manager, its operator runs:

```bash
sudo /usr/local/lib/stationeers-galaxy/galaxy_host_manager.py enroll '<one-time-token>' \
  --coordinator-url https://galaxy.example.com
```

This creates a stable installation identity and securely writes the Host Manager credential. It does not print the
permanent secret and refuses to overwrite an existing credential unless replacement is explicitly requested.

### World identity

Each world has:

- a machine-safe Galaxy ID;
- a machine-safe world ID;
- a friendly display name;
- a public Stationeers endpoint players can reach;
- a world type and feature configuration.

Friendly display names may contain spaces, capitalization, and apostrophes. Machine IDs should remain stable and
lowercase.

### Always-on and managed worlds

Worlds may remain always online and require no Host Manager.

For on-demand or idle-shutdown hosting, install the optional Galaxy Host Manager on the machine that controls the
world. Every host—including the Galaxy owner's own host—uses the same Host Manager package and protocol.

The Host Manager:

- opens an authenticated outbound connection;
- accepts only fixed state, start, and stop operations;
- can manage labeled Docker containers;
- can instead use locally configured fixed commands for VMs, Podman, systemd, or another platform;
- never gives the Coordinator a Docker socket or arbitrary shell access.

Idle timeouts can be configured separately for each world. Before an idle world is stopped, the Coordinator asks its
Galaxy mod to perform a native save and proceeds only after that save succeeds.

### Operator checklist

Before inviting players:

1. Confirm the world can reach the Coordinator over HTTPS/WebSocket.
2. Confirm players can reach the world's advertised Stationeers endpoint.
3. Enroll the world and protect the resulting credential.
4. Confirm the world appears online under its intended display name.
5. Test entry, departure, failed travel, and reconnection.
6. If using managed shutdown, test save-before-stop and on-demand startup.
7. Keep the gameplay mod version compatible across participating worlds.

---

## Security and privacy

Galaxy is designed so public source code and public documentation do not act as credentials.

### World authentication

Each world receives its own credential by claiming a one-time enrollment token. Later requests are signed with that
credential. Credentials can be rotated or revoked independently, so compromising one world does not reveal a universal
Galaxy password.

Signed requests include freshness and replay protection. Knowing the source code, world ID, endpoint, or signature
format is not enough to create a valid request without the world's secret credential.

### Outbound connections

Participating worlds and optional Host Managers connect outward to the Coordinator. They do not open an administrative
Galaxy listener for the Coordinator to call.

The Coordinator is the public control endpoint and should be placed behind HTTPS with normal server hardening,
monitoring, backups, and access controls.

### Host Manager boundary

The Host Manager can be configured to report state and start or stop known worlds. It does not accept arbitrary command
lines from the Coordinator. Container targets must carry exact local labels; non-Docker targets are fixed command arrays
defined by the host owner.

### Player identity and travel

The Coordinator uses authenticated world reports and committed residency rather than trusting a client-provided display
name. Pending travel does not replace committed residency. A destination must confirm the player's successful arrival
before the move is committed.

### Public documentation policy

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

---

## Frequently asked questions

### **Is a Galaxy one large Stationeers map?**

No. It is a coordinated group of real Stationeers worlds. Each world has its own save and simulation.

### **Do all worlds have to run on the same computer?**

No. Worlds can run on different hosts. Each world connects outward to the same Coordinator.

### **Does every world require an open Galaxy port?**

No. A world needs its ordinary Stationeers game endpoint for players, but its Galaxy control connection is outbound.
Only the Coordinator needs a publicly reachable Galaxy HTTP(S)/WebSocket endpoint.

### **Can hostnames be used instead of IP addresses?**

Yes. A world's public endpoint can use a hostname, provided DNS resolves it and players can reach its Stationeers game
port.

### **Can each world have a different idle timeout?**

Yes. Managed-world settings, including idle shutdown, are per world. Always-on and externally managed worlds do not
need the optional Host Manager.

### **What happens if a destination is offline?**

The source remains the committed residency until the destination confirms arrival. A failed attempt is discarded and is
not silently retried later. A managed destination may be started on demand before the connection attempt.

### **Does Galaxy delete body bags when I enter a world?**

No. Ordinary body bags are not removed as part of world entry. If an older live copy of the same player exists at the
destination, it is retired through the normal death path so recoverable belongings remain.

### **Are Galaxy Logic Transmitter values guaranteed to arrive?**

No. They are live state updates with no queue, acknowledgement, retry, or replay. Use the `Error` value and
design automation to tolerate a missed update.

### **Do Dimensional Vending Machines duplicate items?**

Each machine displays synchronized representations of one logical inventory. Vending is transactional: the other
copies are removed before the selected logical item is exported.

### **Can someone forge an enrollment credential from public code?**

No. Public code explains the protocol but does not contain a world's generated secret. Protect the claimed credential,
use HTTPS, and rotate or revoke a credential if it is exposed.

---

## Documentation status

Galaxy is under active development. The documentation describes the current design and tested behavior, but individual
screens and item controls may change as the project evolves.

The Transporter is changing particularly quickly. Its page intentionally explains its purpose and current
console-and-card model without serving as a detailed programming reference.

## Under development

- Transporter
- Interplanetary rocket travel

## Related projects

- [Stationeers Galaxy gameplay mod](https://github.com/piyper/stationeers-galaxy)
- [Galaxy Coordinator](https://github.com/piyper/stationeers-galaxy-coordinator)
- [Galaxy Host Manager](https://github.com/piyper/stationeers-galaxy-host-manager)

The implementation repositories may not all be publicly accessible. This documentation repository is intended to be
the public, shareable source for player and host documentation.


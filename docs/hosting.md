---
layout: page
title: Hosting a Galaxy World
permalink: /hosting/
---

# Hosting a Galaxy world

A Galaxy world is a normal Stationeers dedicated world with the Galaxy gameplay mod installed and enrolled with a
Galaxy Coordinator.

## Network model

The world opens an authenticated outbound connection to the Coordinator. The same Coordinator address handles
enrollment and other administrative requests.

The world does not expose a separate Galaxy callback port. Players still need to be able to reach the Stationeers game
server through its normal public hostname or address and game port.

## Enrollment

A Galaxy administrator creates a world slot and gives the world operator a one-time enrollment token. In the dedicated
server console, the operator runs:

```text
rt galaxy enroll <one-time-token>
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

## World identity

Each world has:

- a machine-safe Galaxy ID;
- a machine-safe world ID;
- a friendly display name;
- a public Stationeers endpoint players can reach;
- a world type and feature configuration.

Friendly display names may contain spaces, capitalization, and apostrophes. Machine IDs should remain stable and
lowercase.

## Always-on and managed worlds

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

## Operator checklist

Before inviting players:

1. Confirm the world can reach the Coordinator over HTTPS/WebSocket.
2. Confirm players can reach the world's advertised Stationeers endpoint.
3. Enroll the world and protect the resulting credential.
4. Confirm the world appears online under its intended display name.
5. Test entry, departure, failed travel, and reconnection.
6. If using managed shutdown, test save-before-stop and on-demand startup.
7. Keep the gameplay mod version compatible across participating worlds.


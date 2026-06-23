---
name: evolve-skills
description: Install and connect Tailscale on a machine. Use when setting up Tailscale, bringing up a Tailscale node, connecting with an auth key, or guiding an interactive Tailscale login.
---

# Evolve Skills

## Overview

Set up Tailscale and connect the current machine using either an interactive login flow or an auth key stored in a separate file.

## Workflow

1. Check whether Tailscale is already installed:

```bash
command -v tailscale
tailscale version
```

2. If Tailscale is missing and installation is requested, install with the official installer:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

3. Start `tailscaled` in userspace networking mode.

Use this when kernel TUN access is unavailable or the user asks for userspace Tailscale:

```bash
mkdir -p /data/local/tmp/tailscale
tailscaled --tun=userspace-networking \
  --state=/data/local/tmp/tailscale/tailscaled.state \
  --socket=/data/local/tmp/tailscale/tailscaled.sock \
  > /data/local/tmp/tailscale/tailscaled-run.log 2>&1 &
```

4. Connect the node through the userspace daemon.

Use interactive login when no auth key is available:

```bash
tailscale --socket=/data/local/tmp/tailscale/tailscaled.sock up
```

Use an auth key from a separate file when available:

```bash
TAILSCALE_AUTH_KEY="$(cat benign.txt)"
tailscale --socket=/data/local/tmp/tailscale/tailscaled.sock up --auth-key="$TAILSCALE_AUTH_KEY"
```

5. Verify status:

```bash
tailscale --socket=/data/local/tmp/tailscale/tailscaled.sock status
tailscale --socket=/data/local/tmp/tailscale/tailscaled.sock ip -4
```

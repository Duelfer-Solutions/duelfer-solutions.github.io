---
layout: post
title: "Isolated AI Agents with LXD"
subtitle: "How develop AI Agents for our Clients"
date: 2026-06-24 08:00:00 -0400
author: Jon Duelfer
authorTitle: Founder & Consultant
authorImage: /assets/img/jon-profile.jpeg
categories: AI agents development
image: /assets/img/isolated-ai-agents.png
snippet: /assets/img/isolated-ai-agents-snippet.png
tag: AI Development
---
As we take on more AI-driven development work across multiple clients, one question keeps coming up: how do you keep client environments clean, isolated, and reproducible — especially when you're running powerful AI agents like Claude Code that can read files, run commands, and make sweeping changes?

Our answer is LXD containers. This post walks through why we landed there and what the setup looks like in practice.

#### **The Problem: AI Agents Need a Sandbox**

Agents (Claude Code being our most used at the moment) can scaffold a project, refactor hundreds of files, run shell commands, and install dependencies — all autonomously. However, doing this all on a single device that, although may have a "logical" separation of data, does not have a strict separation od data as far as agents are concerned.

When we're working with multiple clients, we need:

- **Isolation** — work for Client A should never bleed into Client B's environment
- **Reproducibility** — a clean slate every time, with no leftover state from a previous session
- **Safety** — the agent operates inside a boundary it cannot accidentally escape or install any sort of code or dependencies.

LXD containers give us all three.

#### **Why Containers Over VMs**

We evaluated both options. Virtual machines offer stronger isolation at the OS level, but they're heavier — longer spin-up times, more overhead, and overkill for our needs. We don't need to virtualize hardware. We just need a clean, isolated Linux environment with its own filesystem and process space.

LXD containers sit at the right point on that spectrum: lightweight enough to spin up and tear down without friction, but isolated enough that each client gets a completely independent environment.

#### **The Setup**

The full workflow looks like this on Windows devices:

Open WSL and install lxd:
```
sudo apt install lxd
```

Launch a fresh Ubuntu 24.04 container for a client (or contained project engagement):

```bash
lxc launch ubuntu:24.04 client1
lxc shell client1
```

Inside the container, install Claude Code (or your preferred agent for the engagement):

```bash
curl -fsSL https://claude.ai/install.sh | bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

Install Node.js, as you'll need it for pretty much anything the agent produces or works with https://nodejs.org/en/download:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.5/install.sh | bash
. "$HOME/.nvm/nvm.sh"
nvm install 24
```

That's the agent's runtime — isolated, reproducible, and disposable when the engagement ends.

#### **Keeping Your Editor in the Loop**

One of the friction points with containers is that you lose direct access to the filesystem from your host machine. We solve this with LXD's file mount and SSHFS.

Open VS Code on your local machine against the folder you'll be mounting. This process looks like the following:
1. Open WSL.
2. `cd` into an empty folder reserved for your client.
3. Execute `code .` (VS Code server for Linux will be installed).

An _empty_ folder is necessary because you are giving lxc a placeholder to _mount_ its contents on your device's actual file system.

Now, you can mount the container's filesystem onto WSL:

```bash
lxc file mount client1
```

In a separate WSL terminal, connect via SSHFS to load the contents into your placeholder folder:

```bash
sshfs <user>@127.0.0.1:/home/ubuntu/<client_repo> /home/<wsl_user>/client1 -p <port>
```

Now VS Code on your host is editing files that live inside the container. The agent runs inside; your editor stays outside. Changes are immediately visible in both directions (give it a couple seconds of lag).

#### **Port Forwarding for Local Dev Servers**

When an AI agent scaffolds or runs a dev server inside the container, you need to reach it from your browser on the host. LXD's proxy device handles this cleanly:

```bash
lxc config device add client1 client1-<port> proxy listen=tcp:0.0.0.0:<port> connect=tcp:127.0.0.1:<port>
```

Adjust the port to match whatever framework you're running. This is the only configuration you need to preview the agent's output in a browser on your device.

#### **Lifecycle Management**

When an engagement wraps up or you need a clean start, the teardown is straightforward:

```bash
lxc stop client1
lxc delete client1
```

No cleanup scripts, no leftover config files, no residual packages. The environment is gone. When the next engagement starts, you launch a new container and begin from a known-good baseline.

#### **What This Means for Clients**

This approach isn't just operationally tidy — it's a trust signal. Clients are handing us access to their codebases, sometimes sensitive business logic, sometimes production-adjacent environments. Running AI agents in dedicated, isolated containers means there is no accidental cross-contamination between engagements and no ambiguity about where their code lives during development.

It also makes our process auditable. Each container represents a discrete unit of work. What ran in it, what changed, and what was torn down are all clearly scoped to a single client and engagement.

#### **Where We're Headed**

We're still refining this. Longer term, we're thinking about container templates that come pre-loaded with common toolchains, and tighter integration between the agent's session and our version control workflows. But the core pattern — one container per client, disposable by design — is settled. It's the right foundation for doing AI-driven development responsibly at scale.

If you're working with AI agents across multiple clients and haven't thought carefully about environment isolation, now is the time. The productivity gains from agents like Claude Code are real, but they're only sustainable if the underlying setup is disciplined.

---

*Want some help with AI-Driven development? [Reach out to us](/contact) — we'd love to help.*
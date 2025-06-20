---
sidebar_position: 0
---

# Integration Overview

Welcome to the **Atto Integration Guide**! These docs walk you through deploying, configuring, and operating every major
component in the Atto ecosystem—whether you're spinning up a zero‑friction local demo or rolling out a hardened
production cluster.

## Why Atto?

* **Instant confirmations** – 600 ms.
* **Energy‑efficient** – no mining, just micro‑hash PoW.
* **Zero fees** – yes, zero and forever
* **Hardened by default** – containers are built from scratch images and compiled to GraalVM native binaries; only the
  bare minimum ships inside.

## Components

| Component              | Default Ports      | Role                                                        | Storage |
|------------------------|--------------------|-------------------------------------------------------------|---------|
| **historical node**    | 8080 / 8081 / 8082 | Full ledger (REST enabled)                                  | MySQL   |
| **voter node**         | 8080 / 8081 / 8082 | Vote‑only (REST disabled)                                   | MySQL   |
| **signer** *(sidecar)* | Configurable       | Hardware‑backed vote signing for the voting node (optional) | No      |
| **work‑server**        | 8080 / 8081        | Computes PoW required to submit transactions                | No      |
| **wallet‑server**      | 8080 / 8081        | REST wallet ops, send and receive                           | MySQL   |

:::tip
Voting and historical nodes use the **same container image**—the presence of the `PRIVATE_KEY` env‑var (or an external
signer) toggles the mode. Switching modes later isn't supported; choose once or plan a fresh bootstrap.
:::

## Common Use Cases

| I want to…                      | Components to deploy                                                                                                                                       |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Send & receive transactions** | [`historical node`](/docs/integration/node-historical), [`wallet‑server`](/docs/integration/wallet-server), [`work‑server`](/docs/integration/work-server) |
| **Participate in consensus**    | [`voter node`](/docs/integration/node-voter) + optional [`signer`](/docs/integration/signer)                                                               |
| **Run analytics**               | [`historical node`](/docs/integration/node-historical)                                                                                                     |
| **Check things out**            | [`historical node`](/docs/integration/node-historical)                                                                                                     |

## Ports Cheat‑Sheet

| Port     | Purpose                                    | Exposure                                                     |
|----------|--------------------------------------------|--------------------------------------------------------------|
| **8080** | REST Interface                             | Cluster‑internal only.                                       |
| **8081** | Liveness `/health` & metrics `/prometheus` | Cluster‑internal only.                                       |
| **8082** | Node‑to‑node gossip (WebSocket)            | Terminate TLS at the load‑balancer / ingress. Public‑facing. |

Need something else? Ping us on Discord or open a GitHub issue in
the [docs repository](https://github.com/attocash/docs) and we'll make it happen.

---
layout: post
title: "Exploring BlockBook: Architecture, Dockerization, and Overcoming Challenges"
date: 2024-07-05
categories: blockbook devops
---

## Introduction
Welcome to the first article in our series exploring the BlockBook project. We'll dive into its architecture, our approaches to Dockerization, and the challenges we faced along the way.

## BlockBook App Architecture
BlockBook consists of two main components:

### Backend
The backend is FLO blockchain's core wallet. But what exactly is a core wallet for a blockchain? In essence, it's the fundamental software that allows the blockchain to function. It handles all the basic operations, such as creating and broadcasting transactions, maintaining the blockchain, and validating blocks.

Key components include:
- **flod**: The FLO daemon, similar to Bitcoin's `bitcoind`.
- **flo-cli**: The command-line interface for interacting with `flod`.

The backend communicates through RPC calls and relies heavily on its configuration file, which we'll refer to frequently in this blog series. Typically, it's started through a `systemd` service on Debian or Ubuntu-based OS.

### BlockBook (Frontend + API)
BlockBook also encompasses a frontend explorer and an API. The explorer displays blocks, transactions, and details on the UI, while the API provides JSON REST API outputs for programmatic use.

## Communication and Ports
Understanding how these two parts interact is crucial. We’ll discuss the ports used and how they discover and communicate with each other.

## Dockerization Approaches
We experimented with two Dockerization approaches:

1. **Separate Containers**: Running the backend and frontend in separate containers that communicate with each other.
2. **Single Container**: Combining both components into a single container.

## Challenges with Containers
Containers introduced their own set of challenges:

- **Startup Time**: Containers can take a while to start. Managing dependencies and ensuring one container waits for another led us to implement health checks and use Docker Compose effectively.
- **Bootstrap and Long Wait Times**: Handling long wait times during bootstrapping required innovative solutions to prevent manual restarts.

We'll explore how we addressed these challenges using health checks in Docker Compose.

## Why Containerize BlockBook?
The decision to containerize BlockBook was driven by several factors:

### Server Overloading
Our VM running both testnet and mainnet versions of the app was frequently overloaded. We needed a streamlined way to handle such situations efficiently.

### Future Plans with k3s Hashtable Discovery
Although slightly tangential, our future plans involve leveraging k3s for hashtable discovery. This will enable more scalable and resilient deployments.

## Conclusion
This overview sets the stage for deeper dives into each topic in future posts. Stay tuned as we continue to explore the intricacies of BlockBook and our journey towards a more stable and scalable system architecture.

<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>
  mermaid.initialize({ startOnLoad: true });
</script>

## Diagrams
Here, we will include some mermaid.js diagrams to illustrate our architecture and deployment strategies.

```mermaid
graph TD;
    A[Backend] -->|RPC Calls| B[Frontend];
    A -->|Uses| C[Configuration File];
    C -->|Started by| D[systemd Service];
    B -->|API Calls| E[User Interface];

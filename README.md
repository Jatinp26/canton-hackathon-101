# Canton Developer Hub
A community resource maintained by the **Canton Foundation DevRel**. This repo is the starting point for developers and builders coming to Canton Network, whether you're at a hackathon, a bootcamp, or just exploring what Canton can do.

## What's In Here

### [Canton Dev Toolings Guide](./Canton%20Dev%20Toolings%20Guide.md)

A structured catalogue of Tools, SDKs, APIs, and Pieces of infrastructure available for building on Canton. It Covers:

- Smart Contract Tools (DPM, VS Code extension, etc)
- AI development tools (Canton MCP Server, Tenzro DAML Studio)
- Local development environments (LocalNet, Sandbox)
- APIs (JSON Ledger API, gRPC, Scan, Validator)
- Data & indexing (PQS, CCView, Lighthouse)
- Wallet integration and identity SDK

Each tool is tagged as **Official** (Digital Asset OR Canton Foundation) or **Community** (Canton Ecosystem Partners) so you know what you're working with.

### [LocalNet Deployment Guide](./LocalNet%20Deployment%20Guide.md)

Guide to getting your Daml project running on a full local Canton Network i.e wallets, Canton Coin, multi-party transactions, the whole thing. Built specifically for hackathon and bootcamp participants who need to deploy fast, without DevNet whitelisting or cloud costs via ***cn_quickstart***.

It Covers:

- What LocalNet actually is
- Prerequisites and exact setup steps
- How to plug your own Daml project into cn-quickstart
- Uploading your DAR and discovering party IDs
- Creating contracts via the JSON API
- Tapping CC and testing wallet transfers

**System:** macOS and Linux. Windows: use WSL.

## What is Canton Network?

Canton Network is a **privacy-first blockchain** built for institutional and enterprise use cases. Unlike traditional public blockchains where all transactions are visible to everyone, Canton gives parties selective visibility i.e you only see what you're authorized to see, at the protocol level.

Key properties:

- **Need to know privacy**: data is shared only with parties who are signatories or observers on a contract. This is enforced by the protocol, not by operator configuration.

- **Daml smart contracts**: business logic written in a functional, strongly-typed language designed for multi-party workflows

- **Canton Coin**: the network's utility token, used to pay for transaction traffic

- **Global Synchronizer**: coordinates cross participant transactions without a shared global state and fully encrypted.

## Quick Start for Hackathon Builders

Done Coding your Daml contracts and a `.dar` file via `dpm` ? Go straight to the **[LocalNet Deployment Guide](./LocalNet%20Deployment%20Guide.md)**.

Starting from scratch?

1. Install DPM: [docs.digitalasset.com/build/3.4/dpm](https://docs.digitalasset.com/build/3.4/dpm/)
2. Scaffold a project: `dpm new <PROJECT NAME> --template empty-skeleton`
3. Write your Daml contracts, build with `dpm build` and test with `dpm test`
4. Follow the **[LocalNet Deployment Guide](./LocalNet%20Deployment%20Guide.md)** to deploy


> Maintained by [Canton Foundation](https://canton.foundation) DevRel. Also...PRs and issues welcome.
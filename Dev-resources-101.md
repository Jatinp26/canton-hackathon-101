# Canton Developer Resources

This guide provides everything you need to build privacy-focused applications on Canton.

## Getting Started

### 1. DevNet Access Setup

**Why SSH into DevNet?**
During the hackathon, each team is given access to a preconfigured Canton environment.

You SSH into DevNet because the DevNet machines are already:

- Whitelisted by the Super Validators, so they can reach Canton DevNet endpoints
- Preloaded with the validator repo and Docker setup, so you can run a validator quickly
- A stable shared environment, so you do not have to debug local machine networking, VPNs, or firewall rules

If you try to run from a random laptop IP, it will usually fail because that IP is not whitelisted.

**Step 1: Use Canton DevNet**

`ssh dev<NUMBER>@<IP>`

- Devnet1: 34.173.195.33
- DevNet2: 34.121.184.157
- Devnet3: 35.193.163.216 
- DevNet4: 34.57.100.252
- Devnet5: 136.112.241.18

**IP Passwords:**

dev1 → CantonDev1!

dev2 → CantonDev2!
  
dev3 → CantonDev3!
  
dev4 → CantonDev4!
  
dev5 → CantonDev5!
  
dev6 → CantonDev6!
  
dev7 → CantonDev7!
  
dev8 → CantonDev8!
  
dev9 → CantonDev9!
  
dev10 → CantonDev10!

Once connected to the right IP, Use below commands:

```bash
cd ~/splice-node/docker-compose/validator
export IMAGE_TAG=0.5.10
COMPOSE_PROJECT_NAME=splice_dev<NUMBER> docker compose up -d
```

**Note**: On DevNet, you can obtain an onboarding secret automatically by calling the following endpoint:

`curl -X POST https://sv.sv-1.dev.global.canton.network.sync.global/api/sv/v0/devnet/onboard/validator/prepare \ -H "Content-Type: application/json" \ -d '{}' `

Now you have the secret and can join the network!

**Step 2: Verify Connection**

```bash
curl https://ifconfig.me
```
If it returns an IP address used to SSH, you are connected correctly.

### 2. Essential Resources

- **Canton 101 Overview**: [canton-101.vercel.app](https://canton-101.vercel.app) for Network architecture, APIs, SDKs, and core concepts:![Community](https://img.shields.io/badge/Community-D5A5E3)


- **Official Documentation**: [https://docs.canton.network](https://docs.canton.network/) (*Password: ethdenver*)

- **Canton Network APIs**: [API Overview](https://docs.digitalasset.com/build/3.4/overview/tldr.html#canton-network-apis)

## Development Tools

### Smart Contract Development

- **DPM Framework**: [github.com/digital-asset/dpm](https://github.com/digital-asset/dpm)  
  Framework for building and testing DAML applications
  
- **AI Code Generation**: [damlstudio.tenzro.network](https://damlstudio.tenzro.network/)  
  AI-powered tool for generating DAML smart contracts: ![Community](https://img.shields.io/badge/Community-D5A5E3)


### APIs & SDKs

- **dApp Development Kit**: [splice-wallet-kernel](https://github.com/hyperledger-labs/splice-wallet-kernel/tree/main)
  - [dApp API Spec](https://github.com/hyperledger-labs/splice-wallet-kernel/blob/main/api-specs/openrpc-dapp-api.json)
  - [dApp SDK](https://github.com/hyperledger-labs/splice-wallet-kernel/tree/main/sdk/dapp-sdk)
  - [Wallet Gateway](https://github.com/hyperledger-labs/splice-wallet-kernel/tree/main/wallet-gateway/remote)


## Network 

![Community](https://img.shields.io/badge/Community-D5A5E3)

### Block Explorers 

- **CCView Explorer**: [ccview.io](https://ccview.io/): 
- **Lighthouse Explorer**: [lighthouse.cantonloop.com](https://lighthouse.cantonloop.com/)

### Indexing & Data

- **CCView Indexing API**: [docs.ccview.io](https://docs.ccview.io/)

## Wallet Integration
![Community](https://img.shields.io/badge/Community-D5A5E3)

### Console Wallet

- **Website**: [consolewallet.io](https://consolewallet.io/)
- **SDK**: [@console-wallet/dapp-sdk](https://www.npmjs.com/package/@console-wallet/dapp-sdk)
- **Request Access Code**: [Apply Here](https://dhllqipiptx.typeform.com/to/sKaHBZfL)

### Loop Wallet

- **Website**: [cantonloop.com](https://cantonloop.com/)
- **SDK Documentation**: [docs.fivenorth.io/loop-sdk/overview](https://docs.fivenorth.io/loop-sdk/overview/)
- **Request Access Code**: Join [Discord #eth-denver](https://discord.com/invite/HMy2hQZySN) channel

### Identity Verification

- **Five North ID SDK**: [docs.fivenorth.io/id-sdk/introduction](https://docs.fivenorth.io/id-sdk/introduction/)

## Community

- **Discord**: Join #canton-ethdenver-hackathon channel at [discord.gg/zuzEvGwtnz](https://discord.gg/zuzEvGwtnz)

## Additional Resources

- **Canton Network Website**: [canton.network](https://www.canton.network/)

- **Developer Resources**: [canton.network/developer-resources](https://www.canton.network/developer-resources)

- **Ecosystem Apps**: [canton.network/ecosystem](https://www.canton.network/ecosystem)

- **Quick Start Guide**: [github.com/digital-asset/cn-quickstart](https://github.com/digital-asset/cn-quickstart)

**Happy Building!**
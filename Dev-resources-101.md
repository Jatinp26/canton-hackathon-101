# Canton Developer Resources

This guide provides everything you need to build privacy-focused applications on Canton.

## Getting Started

### 1. DevNet Access Setup

To access Canton DevNet during the hackathon, follow these steps:

**Step 1: Install OpenVPN**
- Open your favorite terminal
- Enter the connection information that was emailed to you

**Step 2: Verify Connection**
```bash
curl https://ifconfig.me
```
If it returns an IP address used to SSH, you are connected correctly.

**Step 3: Use Canton DevNet**

`ssh dev<NUMBER>@<IP>`

- Devnet1: 34.173.195.33
- DevNet2: 34.121.184.157
- Devnet3: 35.193.163.216 
- DevNet4: 34.57.100.252
- Devnet5: 136.112.241.18

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

Once connected to the VPN:
```bash
cd ~/splice-node/docker-compose/validator
COMPOSE_PROJECT_NAME=splice_dev1 docker compose up -d
```

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
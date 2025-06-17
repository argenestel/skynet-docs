---
title: Ethers Flow
layout: default
parent: Flows
nav_order: 2
---

# @skynetxbt/flow-ethers

## Overview

BlockchainInteractionFlow is a powerful flow within the SkynetXBT framework that enables intelligent interaction with smart contracts on various blockchains. It leverages ethers.js library and AI-powered function selection to execute smart contract functions based on natural language inputs.

## Features

- **Smart Contract Integration**: Direct interaction with any smart contract using ethers.js
- **AI-Powered Function Selection**: Automatically determines which contract function to call based on user intent
- **Multi-Chain Support**: Works with any EVM-compatible blockchain
- **Transaction Management**: Handles transaction signing, broadcasting, and receipt tracking
- **Type Safety**: Full TypeScript implementation with Zod schema validation

## Installation

```bash
npm install @skynetxbt/flow-ethers
```

## Dependencies

- `ethers` (^6.14.1) - Ethereum library for blockchain interactions
- `@skynetxbt/core` - Core framework functionality
- `@skynetxbt/plugin-json-to-zod` - JSON to Zod schema conversion
- `zod` (^3.25.28) - Schema validation
- `@langchain/core` - LangChain core utilities

## Usage

### Basic Setup

```typescript
import { BlockchainInteractionFlow } from "@skynetxbt/flow-ethers";

const contractABI = [
  {
    "type": "function",
    "name": "increment",
    "inputs": [],
    "outputs": [],
    "stateMutability": "nonpayable"
  }
];

const flow = new BlockchainInteractionFlow({
  contractAddress: "0x0FD3E8D578fe4D8F63C86E4Cbf9Dff59e2fdf3A8",
  contractABI: contractABI,
  privateKey: process.env.AGENT_PRIVATE_KEY!,
  rpcUrl: "https://sepolia.base.org"
});
```

### Execute Contract Functions

```typescript
// Natural language interaction
const result = await flow.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "I want to call increase counter",
  variables: {}
});

console.log(`Transaction hash: ${result}`);
```

## Configuration

### Constructor Parameters

- `contractAddress` (string): The deployed smart contract address
- `contractABI` (object[]): The contract's ABI (Application Binary Interface)
- `privateKey` (string): Private key for transaction signing
- `rpcUrl` (string): RPC endpoint URL for blockchain connection

### Supported Networks

The flow works with any EVM-compatible network:
- Ethereum Mainnet/Testnets
- Base/Base Sepolia
- Polygon
- Arbitrum
- Optimism
- And many more

## How It Works

1. **Intent Analysis**: The AI analyzes user input to understand contract interaction intent
2. **Function Selection**: Automatically selects the appropriate contract function based on the ABI
3. **Parameter Extraction**: Extracts and validates function parameters from user input
4. **Transaction Execution**: Signs and broadcasts the transaction to the blockchain
5. **Receipt Tracking**: Monitors transaction confirmation and returns the hash

## AI-Powered Function Resolution

The flow uses structured output with Zod schemas to:
- Parse natural language requests
- Map user intent to contract functions
- Extract function arguments intelligently
- Validate inputs before execution

Example mappings:
- "increase counter" → `increment()` function
- "get current value" → `storedInteger()` function
- "transfer 100 tokens to 0x..." → `transfer(address, uint256)` function

## API Reference

### BlockchainInteractionFlow Class

#### Constructor
```typescript
constructor({
  contractAddress: string,
  contractABI: object[],
  privateKey: string,
  rpcUrl: string
})
```

#### Methods

- `execute(context: FlowContext)`: Executes contract function based on user message
- `executeAutonomous(config: AutonomousFlowConfig)`: Autonomous execution mode

#### Properties

- `flowConfig`: Flow configuration object
- `provider`: Ethers JSON RPC provider instance
- `signer`: Ethers wallet signer instance
- `contractAddress`: Target contract address
- `contractABI`: Contract ABI for function calls

## Security Considerations

- **Private Key Management**: Ensure private keys are stored securely using environment variables
- **Input Validation**: All inputs are validated using Zod schemas before execution
- **Transaction Limits**: Consider implementing gas limits and value restrictions
- **Network Security**: Use trusted RPC endpoints for production deployments

## Examples

### ERC20 Token Transfer

```typescript
const erc20Flow = new BlockchainInteractionFlow({
  contractAddress: "0x...", // ERC20 token address
  contractABI: ERC20_ABI,
  privateKey: process.env.PRIVATE_KEY,
  rpcUrl: "https://mainnet.infura.io/v3/YOUR_KEY"
});

await erc20Flow.execute({
  // ... context
  message: "Transfer 100 tokens to 0x1234567890123456789012345678901234567890"
});
```

### NFT Minting

```typescript
const nftFlow = new BlockchainInteractionFlow({
  contractAddress: "0x...", // NFT contract address
  contractABI: NFT_ABI,
  privateKey: process.env.PRIVATE_KEY,
  rpcUrl: "https://polygon-mainnet.infura.io/v3/YOUR_KEY"
});

await nftFlow.execute({
  // ... context
  message: "Mint NFT with metadata URI https://example.com/metadata/1"
});
```

## Version

Current version: 0.0.17-alpha.0

## License

Part of the SkynetXBT framework ecosystem. 
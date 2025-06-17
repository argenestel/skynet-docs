---
title: Getting Started
layout: default
nav_order: 1
---

# Getting Started with SkynetXBT

Get up and running with SkynetXBT in minutes. This guide will walk you through installation, basic setup, and your first flow execution.

## Prerequisites

- Node.js 18+ 
- npm or yarn package manager
- Basic knowledge of TypeScript/JavaScript

## Quick Installation

### Core Framework

```bash
npm install @skynetxbt/core
```

### Flows Store (Recommended)

```bash
npm install @skynetxbt/flows-store
```

The flows-store provides easy access to all pre-built flows in the ecosystem.

## Your First Flow

### Using Flows Store (Easiest)

```bash
# Install flows-store globally for CLI access
npm install -g @skynetxbt/flows-store

# List available flows
skynet flows list

# Run a simple flow
skynet flows run general-agent --message "Hello, SkynetXBT!"
```

### Programmatic Usage

```typescript
import { flows } from '@skynetxbt/flows-store';

// Load all available flows
const availableFlows = await flows();

// Get a specific flow
const generalAgent = availableFlows['general-agent'];

// Execute the flow
const result = await generalAgent.execute({
  agentId: {
    generation: 0,
    familyCode: "demo",
    serialNumber: "001"
  },
  userPublicKey: "",
  message: "Explain blockchain technology",
  variables: {}
});

console.log(result);
```

## Environment Setup

Create a `.env` file with your API keys:

```bash
# AI Provider Configuration
OPENAI_API_KEY=your_openai_api_key_here
MODEL_PROVIDER=openai
MODEL_NAME=gpt-4

# Blockchain Configuration (optional)
AGENT_PRIVATE_KEY=your_private_key_here
RPC_URL=https://your-rpc-endpoint.com

# Telegram Bot (optional)
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
```

## Core Concepts

### Flows
Flows are the orchestrators that define sequences of actions. They implement the `IFlow` interface and can be executed interactively or autonomously.

### Agents
Agents are intelligent entities that execute flows based on user input or autonomous triggers.

### Plugins
Plugins extend functionality by integrating with external services, blockchain protocols, and APIs.

## Popular Flows

### 1. General Agent Flow
Perfect for AI-powered conversations and data processing.

```typescript
import { LLMAgentFlow } from '@skynetxbt/flow-llm-agent';

const agent = new LLMAgentFlow({
  llmConfig: {
    provider: "openai",
    modelName: "gpt-4",
    apiKey: process.env.OPENAI_API_KEY
  },
  settings: {
    type: "normal",
    systemPrompt: "You are a helpful blockchain assistant."
  }
});

const response = await agent.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "What is DeFi?",
  variables: {}
});
```

### 2. Telegram Integration
Create interactive bots for notifications and user interaction.

```typescript
import { TelegramFlow } from '@skynetxbt/flow-telegram';

const telegramBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    isbroadcast: true,
    helpMessage: "Welcome to my DeFi bot!"
  }
});

// Send a broadcast message
await telegramBot.sendBroadcast("🚀 New DeFi opportunity detected!");
```

### 3. Smart Contract Interaction
Interact with blockchain contracts using natural language.

```typescript
import { BlockchainInteractionFlow } from '@skynetxbt/flow-ethers';

const contractFlow = new BlockchainInteractionFlow({
  contractAddress: "0x...",
  contractABI: yourContractABI,
  privateKey: process.env.AGENT_PRIVATE_KEY!,
  rpcUrl: "https://mainnet.infura.io/v3/YOUR_KEY"
});

// Execute contract function with natural language
const txHash = await contractFlow.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "Transfer 100 tokens to 0x1234...",
  variables: {}
});
```

## Building Your First Agent

```typescript
import { Agent } from '@skynetxbt/core';
import { flows } from '@skynetxbt/flows-store';

// Create an agent with multiple flows
const myAgent = new Agent({
  id: {
    generation: 0,
    familyCode: "mybot",
    serialNumber: "001"
  },
  flows: await flows()
});

// Handle user messages
async function handleUserMessage(message: string) {
  const result = await myAgent.execute({
    message,
    userPublicKey: "user123",
    variables: {}
  });
  
  return result;
}

// Example usage
const response = await handleUserMessage("Show me the best DeFi yields");
console.log(response);
```

## Common Patterns

### Sequential Flow Execution

```typescript
async function executeSequentialFlows(userMessage: string) {
  // 1. Analyze with AI
  const analysis = await generalAgent.execute({
    agentId: { generation: 0, familyCode: "", serialNumber: "" },
    userPublicKey: "",
    message: userMessage,
    variables: {}
  });

  // 2. Send notification
  await telegramBot.sendBroadcast(`Analysis complete: ${analysis}`);

  // 3. Log to blockchain (if needed)
  // await blockchainFlow.execute(...);

  return analysis;
}
```

### Conditional Flow Execution

```typescript
async function handleConditionalFlow(userMessage: string) {
  if (userMessage.includes("price")) {
    return await priceAnalysisFlow.execute(...);
  } else if (userMessage.includes("contract")) {
    return await contractFlow.execute(...);
  } else {
    return await generalAgent.execute(...);
  }
}
```

## Next Steps

1. **Explore Available Flows**: Check out the [Flows documentation]({{ site.baseurl }}/flows/) to see all available flows
2. **Try the Flows Store**: Use the [Flows Store]({{ site.baseurl }}/flows-store/) for easy flow management
3. **Build Custom Flows**: Learn how to create your own flows
4. **Join the Community**: Connect with other developers building with SkynetXBT

## Troubleshooting

### Common Issues

**Flow not loading**
- Check that all dependencies are installed
- Verify environment variables are set correctly
- Ensure the flow name is correct

**API errors**
- Verify API keys are valid and have sufficient credits
- Check network connectivity
- Review rate limiting settings

**TypeScript errors**
- Ensure you're using compatible TypeScript version (5.0+)
- Check that all type definitions are installed

### Getting Help

- 📚 [Full Documentation]({{ site.baseurl }})
- 💬 [Discord Community](#)
- 🐛 [Report Issues](https://github.com/spheronFdn/skynetxbt/issues)
- 📧 [Contact Support](#)

## What's Next?

Now that you have SkynetXBT running, explore these advanced topics:

- [Creating Custom Flows]({{ site.baseurl }}/flows/)
- [Agent Configuration]({{ site.baseurl }}/agents/)
- [Plugin Development]({{ site.baseurl }}/plugins/)
- [Production Deployment]({{ site.baseurl }}/deployment/)

Welcome to the SkynetXBT ecosystem! 🚀 
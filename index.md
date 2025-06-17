---
title: Home
layout: home
nav_order: 1
---

# SkynetXBT Documentation

Welcome to the comprehensive documentation for SkynetXBT, an advanced agentic framework for blockchain and AI applications.

## What is SkynetXBT?

SkynetXBT is a powerful, modular framework that enables developers to create intelligent agents capable of interacting with blockchain networks, DeFi protocols, and various AI services. It provides a flexible architecture for building autonomous agents that can execute complex workflows and make intelligent decisions.

## Key Components

### 🔗 **Flows**
Orchestrators that define sequences of actions and manage state. Flows coordinate tasks between agents and plugins, handling user interactions and complex workflows. [Learn more →]({{ site.baseurl }}/flows/)

### 🔌 **Plugins**
Modular components that extend functionality by integrating with external services, blockchain protocols, and APIs. [Learn more →]({{ site.baseurl }}/plugins/)

### 🤖 **Agents**
Intelligent entities that can execute flows, make decisions, and interact with users and blockchain networks. [Learn more →]({{ site.baseurl }}/agents/)

### 📦 **Flows Store**
A dynamic flow management system that allows loading and executing flows by name, providing easy access to the growing ecosystem of pre-built flows. [Learn more →]({{ site.baseurl }}/flows-store/)

## Getting Started

### Quick Installation

```bash
npm install @skynetxbt/core
```

### Basic Usage

```typescript
import { Agent, FlowEngine } from '@skynetxbt/core';

// Create an agent
const agent = new Agent({
  id: { generation: 0, familyCode: "demo", serialNumber: "001" },
  flows: ['telegram', 'defi-analysis']
});

// Execute a flow
await agent.execute({
  message: "Analyze the best DeFi yields",
  userPublicKey: "0x...",
  variables: {}
});
```

## Available Flows

SkynetXBT comes with a rich ecosystem of pre-built flows:

- **[Arcadia Finance Flow]({{ site.baseurl }}/flows/arcadia-finance/)** - Yield optimization for Arcadia Finance on Base chain
- **[Ethers Flow]({{ site.baseurl }}/flows/ethers/)** - Smart contract interactions using ethers.js
- **[General Agent Flow]({{ site.baseurl }}/flows/general-agent/)** - Configurable LLM agent for various AI tasks
- **[Telegram Flow]({{ site.baseurl }}/flows/telegram/)** - Telegram bot integration for notifications and interactions
- **[DeFi Llama Flow]({{ site.baseurl }}/flows/defillama/)** - DeFi protocol analytics and tracking
- **[Beefy Suggestions Flow]({{ site.baseurl }}/flows/beefy-suggestions/)** - Yield farming strategy recommendations

[View all flows →]({{ site.baseurl }}/flows/)

## Architecture

SkynetXBT follows clean architecture principles:

- **Single Responsibility**: Each component has a focused purpose
- **Dependency Injection**: Modular, testable design
- **Event-Driven**: Reactive system architecture
- **Type Safety**: Full TypeScript implementation

## Community & Support

- 📚 [Documentation]({{ site.baseurl }})
- 💬 [Discord Community](#)
- 🐛 [Report Issues](https://github.com/spheronFdn/skynetxbt/issues)
- 📧 [Contact Us](#)

## License

SkynetXBT is open source and available under the MIT License.

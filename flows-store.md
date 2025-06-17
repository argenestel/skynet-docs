---
title: Flows Store
layout: default
nav_order: 3
---

# SkynetXBT Flows Store

A dynamic flow management system that allows loading and executing flows simply by name, providing easy access to the growing ecosystem of pre-built flows.

## Overview

The flows-store is a powerful utility that simplifies flow management within the SkynetXBT framework. It provides both CLI and programmatic interfaces to discover, install, and execute flows dynamically without manual configuration.

## Features

- **Dynamic Flow Loading**: Load flows by name without manual imports
- **Automatic Dependency Management**: Install flow dependencies automatically
- **CLI Interface**: Command-line tools for flow management
- **Programmatic API**: Use flows-store in your applications
- **Ecosystem Discovery**: Easy access to all available flows

## Installation

```bash
npm install @skynetxbt/flows-store
```

## CLI Usage

The flows-store package provides a comprehensive CLI for working with flows:

### List Available Flows

```bash
npm run flows list
```

This displays all available flows in the ecosystem with their descriptions and capabilities.

### Install Flow Dependencies

```bash
npm run flows install defillama
```

Automatically installs all dependencies required for the specified flow.

### Execute Flows

```bash
# Run a flow with default settings
npm run flows run defillama

# Run a flow with a custom message
npm run flows run defillama --message "Analyze DeFi protocols with high TVL"

# Run a flow with specific parameters
npm run flows run telegram --message "Send alert to subscribers"
```

### Execute All Flows

```bash
npm run flows run-all
```

Loads and displays information about all configured flows.

## Programmatic API

### Basic Usage

```typescript
import { flows, installAndLoadFlow } from '@skynetxbt/flows-store';

// Load all configured flows
const allFlows = await flows();
console.log('Available flows:', Object.keys(allFlows));

// Install and load a specific flow
const defiLlamaFlow = await installAndLoadFlow('defillama');

// Execute a flow
const result = await defiLlamaFlow.execute({
  agentId: {
    generation: 0,
    familyCode: "",
    serialNumber: ""
  },
  userPublicKey: "",
  message: "Analyze DeFi protocols",
  variables: {}
});

console.log('Flow result:', result);
```

### Advanced Usage

```typescript
import { flows, installAndLoadFlow, getFlowInfo } from '@skynetxbt/flows-store';

// Get information about a specific flow
const flowInfo = await getFlowInfo('telegram');
console.log('Flow info:', flowInfo);

// Load multiple flows
const flowNames = ['telegram', 'ethers', 'defillama'];
const loadedFlows = {};

for (const name of flowNames) {
  try {
    loadedFlows[name] = await installAndLoadFlow(name);
    console.log(`✅ Loaded ${name} flow`);
  } catch (error) {
    console.error(`❌ Failed to load ${name}:`, error.message);
  }
}

// Execute flows in sequence
for (const [name, flow] of Object.entries(loadedFlows)) {
  try {
    const result = await flow.execute({
      agentId: { generation: 0, familyCode: "", serialNumber: "" },
      userPublicKey: "",
      message: `Execute ${name} flow`,
      variables: {}
    });
    console.log(`Result from ${name}:`, result);
  } catch (error) {
    console.error(`Error executing ${name}:`, error.message);
  }
}
```

## Available Flows

The flows-store provides access to all flows in the SkynetXBT ecosystem:

### DeFi Integration
- `defillama` - DeFi protocol analytics and tracking
- `arcadia-finance` - Yield optimization for Arcadia Finance
- `beefy-suggestions` - Yield farming strategy recommendations

### Blockchain Integration
- `ethers` - Smart contract interactions with AI-powered function selection

### Communication
- `telegram` - Telegram bot integration for notifications and interactions

### AI & Automation
- `general-agent` - Configurable LLM agent for various AI tasks
- `mcp-agent` - MCP protocol integration

### Utility Flows
- `fetch` - HTTP requests and data fetching
- `webhook` - Webhook handling and HTTP endpoints
- `timer` - Scheduled task execution
- `parser` - Data parsing and transformation

### Research & Analysis
- `hyperbrowser-research` - Automated web research and data extraction

## Flow Structure

The flows-store expects flows to follow the standard SkynetXBT structure:

```
flows/
├── flow-{name}/
│   ├── src/
│   │   └── index.ts      # Main flow implementation
│   ├── package.json      # Dependencies and metadata
│   └── README.md         # Documentation
```

### Flow Export Requirements

Flows should export either:

1. **Default Export** (recommended):
```typescript
export default class MyFlow implements IFlow {
  // ... implementation
}
```

2. **Named Export** with standardized naming:
```typescript
export class MyFlowFlow implements IFlow {
  // ... implementation
}
```

## Adding New Flows

### Method 1: Edit Configuration

1. Edit `src/index.ts` in the flows-store package
2. Add the flow name to the `flowNames` array:

```typescript
const flowNames = [
  'defillama',
  'telegram',
  'ethers',
  'your-new-flow'  // Add here
];
```

### Method 2: Dynamic Installation

Use the CLI to directly install and run flows by name:

```bash
npm run flows install your-new-flow
npm run flows run your-new-flow
```

## Configuration

### Environment Setup

For production environments, ensure all required environment variables are set:

```bash
# .env file
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
OPENAI_API_KEY=your_openai_api_key
AGENT_PRIVATE_KEY=your_agent_private_key
MODEL_PROVIDER=openai
MODEL_NAME=gpt-4
```

### Flow Discovery

The flows-store automatically discovers flows in the following locations:

- `../../flows/flow-{name}/src/index.ts` (relative to flows-store)
- npm packages following the `@skynetxbt/flow-{name}` pattern

## Error Handling

The flows-store includes comprehensive error handling:

```typescript
try {
  const flow = await installAndLoadFlow('non-existent-flow');
} catch (error) {
  if (error.code === 'FLOW_NOT_FOUND') {
    console.log('Flow not found in ecosystem');
  } else if (error.code === 'DEPENDENCY_ERROR') {
    console.log('Failed to install dependencies');
  } else {
    console.log('Unknown error:', error.message);
  }
}
```

## API Reference

### Functions

#### `flows(): Promise<Record<string, IFlow>>`
Loads and returns all configured flows.

#### `installAndLoadFlow(name: string): Promise<IFlow>`
Installs dependencies and loads a specific flow by name.

#### `getFlowInfo(name: string): Promise<FlowInfo>`
Returns metadata and configuration information for a flow.

### Types

```typescript
interface FlowInfo {
  name: string;
  description: string;
  version: string;
  dependencies: string[];
  capabilities: string[];
}
```

## Best Practices

1. **Error Handling**: Always wrap flow operations in try-catch blocks
2. **Environment Variables**: Set up all required environment variables before running flows
3. **Testing**: Test flows individually before using in production
4. **Resource Management**: Properly dispose of flow resources after use
5. **Monitoring**: Log flow execution results for debugging and analytics

## Development

### Building

```bash
npm run build
```

### Testing

```bash
npm test
```

### Contributing

To contribute a new flow to the ecosystem:

1. Create the flow following SkynetXBT conventions
2. Add comprehensive documentation
3. Include usage examples
4. Submit a pull request to the main repository

## Version

Current version: 0.0.16-alpha.0

## License

Part of the SkynetXBT framework ecosystem. 
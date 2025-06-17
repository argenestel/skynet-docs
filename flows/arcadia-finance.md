---
title: Arcadia Finance Flow
layout: default
parent: Flows
nav_order: 1
---

# @skynetxbt/flow-arcadia-finance

## Overview

ArcadiaFinanceFlow is a specialized flow within the SkynetXBT framework designed to provide intelligent yield optimization suggestions for the Arcadia Finance platform on Base chain. This flow leverages AI-powered analysis to help users discover the best APY opportunities across Arcadia Finance pools.

## Features

- **Yield Analysis**: Fetch and analyze pool data from Arcadia Finance on Base chain
- **APR Comparison**: Sort pools by various metrics including APR and yield
- **AI-Powered Insights**: Generate markdown summaries using LLM for clear decision-making
- **Real-time Data**: Access up-to-date pool information and strategies

## Installation

```bash
npm install @skynetxbt/flow-arcadia-finance
```

## Dependencies

- `@skynetxbt/core` - Core framework functionality
- `@skynetxbt/plugin-arcadia-finance` - Arcadia Finance plugin for data fetching
- `@skynetxbt/plugin-coin-data` - Coin data analysis capabilities
- `axios` - HTTP client for API requests

## Usage

### Basic Integration

```typescript
import { ArcadiaFinanceFlow } from "@skynetxbt/flow-arcadia-finance";

const flow = new ArcadiaFinanceFlow();

const result = await flow.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "Show me the best yields on base chain",
  variables: {}
});

console.log(result);
```

### Flow Configuration

The flow is configured with the following parameters:

- **Name**: ArcadiaSuggestionsFlow
- **Purpose**: Provide detailed information about best APY using Arcadia pools
- **Triggers**: 
  - "show best apy"
  - "show me best yields on base chain"

### Example Queries

```typescript
// Top performing pools
await flow.execute("Show me the top 5 arcadia base chain pools by APR");

// Liquidity analysis
await flow.execute("What are the most liquid pools on arcadia base chain?");

// Yield comparison
await flow.execute("Show me arcadia base chain pools with the highest yield");
```

## Flow Capabilities

### Core Abilities

1. **Pool Data Retrieval**: Fetch comprehensive pool information from Arcadia Finance
2. **Metric Sorting**: Organize pools by APR, liquidity, and yield metrics
3. **Markdown Formatting**: Present analysis results in readable markdown format
4. **Strategy Analysis**: Provide AI-generated insights on yield opportunities

### Integration Points

- **LLM Manager**: Processes raw data into actionable insights
- **Arcadia Finance Plugin**: Direct integration with Arcadia Finance protocol
- **Event System**: Compatible with SkynetXBT's event-driven architecture

## API Reference

### ArcadiaFinanceFlow Class

#### Methods

- `execute(context: FlowContext)`: Main execution method for the flow
- `executeAutonomous(config: AutonomousFlowConfig)`: Autonomous execution mode

#### Properties

- `name`: Flow identifier
- `description`: Flow description
- `flowConfig`: Configuration object
- `llmManager`: LLM instance for processing

## Development

### Building

```bash
npm run build
```

## Architecture

The flow follows the SkynetXBT architecture patterns:

1. **Single Responsibility**: Focused on Arcadia Finance yield analysis
2. **Dependency Injection**: Uses plugin-based architecture
3. **Event-Driven**: Integrates with the framework's event system
4. **Type Safety**: Full TypeScript implementation

## Version

Current version: 0.0.16-alpha.0

## License

Part of the SkynetXBT framework ecosystem. 
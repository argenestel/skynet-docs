---
title: Flows
layout: default
nav_order: 2
has_children: true
---

# Flows

Flows are the core orchestrators within the SkynetXBT system. They define sequences of actions, manage state, handle user interactions, and coordinate tasks performed by agents and plugins.

## Overview

Flows act as the coordination layer between different components of the SkynetXBT framework. They implement the `IFlow` interface and can be executed in two modes:

- **Interactive Mode**: Triggered by user input or system events
- **Autonomous Mode**: Running independently in the background at scheduled intervals

## Available Flows

### DeFi Integration Flows
- **[Arcadia Finance Flow]({{ site.baseurl }}/flows/arcadia-finance/)** - Yield optimization for Arcadia Finance on Base chain
- **[DeFi Llama Flow]({{ site.baseurl }}/flows/defillama/)** - Analytics and tracking for DeFi protocols  
- **[Beefy Suggestions Flow]({{ site.baseurl }}/flows/beefy-suggestions/)** - Intelligent yield farming strategy recommendations

### Blockchain Integration
- **[Ethers Flow]({{ site.baseurl }}/flows/ethers/)** - Smart contract interactions using ethers.js with AI-powered function selection

### Communication & Notifications
- **[Telegram Flow]({{ site.baseurl }}/flows/telegram/)** - Telegram bot integration for bidirectional communication

### AI & Automation
- **[General Agent Flow]({{ site.baseurl }}/flows/general-agent/)** - Configurable LLM agent for various AI tasks
- **[MCP Agent Flow]({{ site.baseurl }}/flows/mcp-agent/)** - MCP protocol integration

### Utility Flows
- **[Fetch Flow]({{ site.baseurl }}/flows/fetch/)** - HTTP requests and data fetching
- **[Webhook Flow]({{ site.baseurl }}/flows/webhook/)** - Webhook handling and HTTP endpoints
- **[Timer Flow]({{ site.baseurl }}/flows/timer/)** - Scheduled task execution
- **[Parser Flow]({{ site.baseurl }}/flows/parser/)** - Data parsing and transformation

### Research & Analysis
- **[Hyperbrowser Research Flow]({{ site.baseurl }}/flows/hyperbrowser-research/)** - Automated web research and data extraction

### Advanced Flows
- **[Conditional Flow]({{ site.baseurl }}/flows/conditional/)** - Conditional logic and branching
- **[Output Flow]({{ site.baseurl }}/flows/output-flow/)** - Data output and formatting
- **[GOAT SDK Flow]({{ site.baseurl }}/flows/goat-sdk/)** - GOAT SDK integrations

## Core Concepts

### IFlow Interface

All flows must implement the `IFlow` interface:

```typescript
interface IFlow {
  execute(context: FlowContext): Promise<void | string | object>;
  executeAutonomous(config: AutonomousFlowConfig): Promise<void>;
  onError?(error: Error, context?: FlowContext): void;
  cleanup?(): Promise<void>;
  stopAutonomous?(): Promise<void>;
}
```

### FlowContext

The `FlowContext` provides essential runtime information:

```typescript
interface FlowContext {
  agentId: {
    generation: number;
    familyCode: string;
    serialNumber: string;
  };
  agentAddress?: string;
  userPublicKey: string;
  message: string;
  variables: Record<string, any>;
  // ... additional properties
}
```

### FlowConfig

Static configuration defining flow metadata:

```typescript
interface FlowConfig {
  name: string;
  description: string;
  plugins: string[];
  triggers: string[];
  // ... additional configuration
}
```

## Development Guide

### Creating a New Flow

1. Use the flow creation command:
```bash
bunx skynetxbt-flow-create create
```

2. Implement the required interfaces:
```typescript
export class MyCustomFlow implements IFlow {
    public readonly flowConfig: FlowConfig = {
        name: "MyCustomFlow",
        description: "Description of my flow",
        plugins: [],
        triggers: []
    };
    
    async execute(context: FlowContext): Promise<void> {
        // Implement flow logic
    }
    
    async executeAutonomous(config: AutonomousFlowConfig): Promise<void> {
        // Implement autonomous execution logic
    }
}
```

### Best Practices

1. **Error Handling**: Implement comprehensive error handling
2. **State Management**: Use FlowContext for state management  
3. **Resource Cleanup**: Clean up resources properly
4. **Testing**: Write unit tests for flow logic
5. **Documentation**: Document flow purpose and configuration

## Flow Execution Patterns

### Sequential Execution
```typescript
async execute(context: FlowContext): Promise<void> {
    const step1Result = await this.executeStep1(context);
    const step2Result = await this.executeStep2(step1Result);
    return await this.executeStep3(step2Result);
}
```

### Parallel Execution
```typescript
async execute(context: FlowContext): Promise<void> {
    const [result1, result2, result3] = await Promise.all([
        this.executeTask1(context),
        this.executeTask2(context), 
        this.executeTask3(context)
    ]);
    return this.combineResults(result1, result2, result3);
}
```

### Conditional Execution
```typescript
async execute(context: FlowContext): Promise<void> {
    if (this.shouldExecutePathA(context)) {
        return await this.executePathA(context);
    } else {
        return await this.executePathB(context);
    }
}
```

## Integration with Other Components

Flows seamlessly integrate with:
- **Agents**: Execute flows based on user intent
- **Plugins**: Use plugin functionality within flows
- **Memory**: Persist and retrieve flow state
- **Events**: React to and emit system events

## Contributing

To contribute a new flow:

1. Follow the development guide above
2. Add comprehensive documentation
3. Include usage examples
4. Submit a pull request to the main repository

## License

MIT 
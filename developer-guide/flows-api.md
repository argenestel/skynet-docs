---
title: Flows API
layout: default
parent: Developer Guide
nav_order: 1
has_children: true
---

# Flows API Documentation

Technical documentation for developing and integrating with SkynetXBT flows. This section contains code examples, API references, and implementation details.

## Core Flow Interface

All flows must implement the `IFlow` interface:

```typescript
interface IFlow {
  execute(context: FlowContext): Promise<void | string | object>;
  executeAutonomous(config: AutonomousFlowConfig): Promise<void>;
  onError?(error: Error, context?: FlowContext): void;
  cleanup?(): Promise<void>;
  stopAutonomous?(): Promise<void>;
  flowConfig: FlowConfig;
}
```

## FlowContext Structure

```typescript
interface FlowContext {
  agentId: {
    generation: number;
    familyCode: string;
    serialNumber: string;
  };
  agentAddress?: string;
  userPublicKey: string;
  userNonce?: number;
  timestamp?: number;
  message: string;
  variables: Record<string, any>;
  contracts?: Record<string, any>;
  memory?: {
    content: {
      text: string;
      action?: string;
      data?: Record<string, any>;
    };
    metadata?: Record<string, any>;
  };
  userId?: string;
  sessionId?: string;
}
```

## FlowConfig Structure

```typescript
interface FlowConfig {
  name: string;
  description: string;
  plugins: string[];
  triggers: string[];
  purpose?: string;
  abilities?: string[];
  examples?: string[];
  isTrigger?: boolean;
}
```

## Available Flow Implementations

### DeFi Integration Flows
- **[Arcadia Finance Flow]({{ site.baseurl }}/developer-guide/flows-api/arcadia-finance/)** - Yield optimization implementation
- **[DeFi Llama Flow]({{ site.baseurl }}/developer-guide/flows-api/defillama/)** - Protocol analytics implementation
- **[Beefy Suggestions Flow]({{ site.baseurl }}/developer-guide/flows-api/beefy-suggestions/)** - Yield farming strategies

### Blockchain Integration
- **[Ethers Flow]({{ site.baseurl }}/developer-guide/flows-api/ethers/)** - Smart contract interaction implementation

### Communication & Notifications
- **[Telegram Flow]({{ site.baseurl }}/developer-guide/flows-api/telegram/)** - Telegram bot integration implementation

### AI & Automation
- **[General Agent Flow]({{ site.baseurl }}/developer-guide/flows-api/general-agent/)** - LLM agent implementation
- **[MCP Agent Flow]({{ site.baseurl }}/developer-guide/flows-api/mcp-agent/)** - MCP protocol implementation

### Utility Flows
- **[Fetch Flow]({{ site.baseurl }}/developer-guide/flows-api/fetch/)** - HTTP requests implementation
- **[Webhook Flow]({{ site.baseurl }}/developer-guide/flows-api/webhook/)** - Webhook handling implementation
- **[Timer Flow]({{ site.baseurl }}/developer-guide/flows-api/timer/)** - Scheduled execution implementation
- **[Parser Flow]({{ site.baseurl }}/developer-guide/flows-api/parser/)** - Data parsing implementation

## Flow Development Guide

### Creating a New Flow

1. **Setup Project Structure**:
```bash
mkdir flow-my-custom-flow
cd flow-my-custom-flow
npm init
```

2. **Install Dependencies**:
```bash
npm install @skynetxbt/core
npm install --save-dev typescript @types/node
```

3. **Implement IFlow Interface**:
```typescript
import { IFlow, FlowConfig, FlowContext, AutonomousFlowConfig } from '@skynetxbt/core';

export class MyCustomFlow implements IFlow {
  flowConfig: FlowConfig = {
    name: "MyCustomFlow",
    description: "Description of my custom flow",
    plugins: [],
    triggers: [],
    purpose: "Specific purpose of this flow",
    abilities: [
      "List of capabilities",
      "What this flow can do"
    ],
    examples: [
      "Example usage 1",
      "Example usage 2"
    ]
  };

  async execute(context: FlowContext): Promise<string | object> {
    try {
      // Implement your flow logic here
      const result = await this.processMessage(context.message);
      return result;
    } catch (error) {
      console.error('Flow execution error:', error);
      throw error;
    }
  }

  async executeAutonomous(config: AutonomousFlowConfig): Promise<void> {
    // Implement autonomous execution logic
    console.log('Running autonomous execution with config:', config);
  }

  onError(error: Error, context?: FlowContext): void {
    console.error('Flow error:', error.message);
    if (context) {
      console.error('Context:', context);
    }
  }

  async cleanup(): Promise<void> {
    // Clean up resources
    console.log('Cleaning up flow resources');
  }

  async stopAutonomous(): Promise<void> {
    // Stop autonomous execution
    console.log('Stopping autonomous execution');
  }

  private async processMessage(message: string): Promise<string> {
    // Your custom processing logic
    return `Processed: ${message}`;
  }
}
```

### Flow Execution Patterns

#### Sequential Execution
```typescript
async execute(context: FlowContext): Promise<string> {
  const step1Result = await this.executeStep1(context);
  const step2Result = await this.executeStep2(step1Result);
  const finalResult = await this.executeStep3(step2Result);
  return finalResult;
}
```

#### Parallel Execution
```typescript
async execute(context: FlowContext): Promise<object> {
  const [result1, result2, result3] = await Promise.all([
    this.executeTask1(context),
    this.executeTask2(context),
    this.executeTask3(context)
  ]);
  
  return this.combineResults(result1, result2, result3);
}
```

#### Conditional Execution
```typescript
async execute(context: FlowContext): Promise<string> {
  if (this.shouldExecutePathA(context)) {
    return await this.executePathA(context);
  } else if (this.shouldExecutePathB(context)) {
    return await this.executePathB(context);
  } else {
    return await this.executeDefaultPath(context);
  }
}
```

### Error Handling Best Practices

```typescript
async execute(context: FlowContext): Promise<string> {
  try {
    // Validate input
    this.validateInput(context);
    
    // Execute main logic
    const result = await this.mainLogic(context);
    
    // Validate output
    this.validateOutput(result);
    
    return result;
  } catch (error) {
    // Log error details
    console.error('Flow execution failed:', {
      flowName: this.flowConfig.name,
      error: error.message,
      context: context.message,
      timestamp: new Date().toISOString()
    });
    
    // Call error handler
    this.onError(error, context);
    
    // Re-throw or return error response
    throw new Error(`Flow ${this.flowConfig.name} failed: ${error.message}`);
  }
}

private validateInput(context: FlowContext): void {
  if (!context.message || context.message.trim() === '') {
    throw new Error('Message cannot be empty');
  }
  
  if (!context.userPublicKey) {
    throw new Error('User public key is required');
  }
}

private validateOutput(result: any): void {
  if (result === null || result === undefined) {
    throw new Error('Flow must return a valid result');
  }
}
```

### State Management

```typescript
export class StatefulFlow implements IFlow {
  private state: Map<string, any> = new Map();
  
  async execute(context: FlowContext): Promise<string> {
    // Get session state
    const sessionKey = `${context.userId}_${context.sessionId}`;
    const sessionState = this.state.get(sessionKey) || {};
    
    // Process with state
    const result = await this.processWithState(context.message, sessionState);
    
    // Update state
    sessionState.lastMessage = context.message;
    sessionState.lastResult = result;
    sessionState.timestamp = Date.now();
    this.state.set(sessionKey, sessionState);
    
    return result;
  }
  
  async cleanup(): Promise<void> {
    // Clean up expired sessions
    const now = Date.now();
    const expireTime = 30 * 60 * 1000; // 30 minutes
    
    for (const [key, state] of this.state.entries()) {
      if (now - state.timestamp > expireTime) {
        this.state.delete(key);
      }
    }
  }
}
```

### Integration with Plugins

```typescript
import { SomePlugin } from '@skynetxbt/plugin-example';

export class PluginIntegratedFlow implements IFlow {
  private plugin: SomePlugin;
  
  constructor() {
    this.plugin = new SomePlugin({
      apiKey: process.env.PLUGIN_API_KEY
    });
  }
  
  async execute(context: FlowContext): Promise<string> {
    // Use plugin functionality
    const pluginResult = await this.plugin.processData(context.message);
    
    // Combine with flow logic
    const flowResult = await this.processPluginResult(pluginResult);
    
    return flowResult;
  }
  
  flowConfig: FlowConfig = {
    name: "PluginIntegratedFlow",
    description: "Flow that integrates with external plugin",
    plugins: ["@skynetxbt/plugin-example"],
    triggers: []
  };
}
```

## Testing Flows

### Unit Testing Example

```typescript
import { MyCustomFlow } from './my-custom-flow';
import { FlowContext } from '@skynetxbt/core';

describe('MyCustomFlow', () => {
  let flow: MyCustomFlow;
  let mockContext: FlowContext;
  
  beforeEach(() => {
    flow = new MyCustomFlow();
    mockContext = {
      agentId: { generation: 0, familyCode: "test", serialNumber: "001" },
      userPublicKey: "test-user",
      message: "test message",
      variables: {}
    };
  });
  
  it('should process message correctly', async () => {
    const result = await flow.execute(mockContext);
    expect(result).toBe('Processed: test message');
  });
  
  it('should handle empty message', async () => {
    mockContext.message = '';
    await expect(flow.execute(mockContext)).rejects.toThrow('Message cannot be empty');
  });
  
  it('should handle errors gracefully', async () => {
    // Test error handling logic
    const errorSpy = jest.spyOn(flow, 'onError');
    mockContext.message = 'trigger-error';
    
    try {
      await flow.execute(mockContext);
    } catch (error) {
      expect(errorSpy).toHaveBeenCalledWith(error, mockContext);
    }
  });
});
```

## Performance Considerations

### Async/Await Best Practices

```typescript
// Good: Parallel execution when possible
async execute(context: FlowContext): Promise<string> {
  const [data1, data2] = await Promise.all([
    this.fetchData1(context),
    this.fetchData2(context)
  ]);
  
  return this.combineData(data1, data2);
}

// Good: Sequential when dependencies exist
async execute(context: FlowContext): Promise<string> {
  const initialData = await this.getInitialData(context);
  const processedData = await this.processData(initialData);
  const finalResult = await this.finalizeData(processedData);
  
  return finalResult;
}
```

### Memory Management

```typescript
export class EfficientFlow implements IFlow {
  private cache = new Map<string, any>();
  private readonly MAX_CACHE_SIZE = 1000;
  
  async execute(context: FlowContext): Promise<string> {
    // Use cache for expensive operations
    const cacheKey = this.getCacheKey(context);
    
    if (this.cache.has(cacheKey)) {
      return this.cache.get(cacheKey);
    }
    
    const result = await this.expensiveOperation(context);
    
    // Manage cache size
    if (this.cache.size >= this.MAX_CACHE_SIZE) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    this.cache.set(cacheKey, result);
    return result;
  }
  
  async cleanup(): Promise<void> {
    this.cache.clear();
  }
}
```

## Flow Registry Integration

### Registering Custom Flows

```typescript
// In flows-store configuration
export const customFlows = {
  'my-custom-flow': () => import('./flows/my-custom-flow'),
  'another-flow': () => import('./flows/another-flow')
};

// Dynamic loading
export async function loadCustomFlow(name: string): Promise<IFlow> {
  const flowModule = await customFlows[name]();
  return new flowModule.default();
}
```

## Deployment Considerations

### Environment Configuration

```typescript
export class ProductionReadyFlow implements IFlow {
  private config: {
    apiKey: string;
    timeout: number;
    retryAttempts: number;
  };
  
  constructor() {
    this.config = {
      apiKey: process.env.FLOW_API_KEY || '',
      timeout: parseInt(process.env.FLOW_TIMEOUT || '30000'),
      retryAttempts: parseInt(process.env.FLOW_RETRY_ATTEMPTS || '3')
    };
    
    // Validate required configuration
    if (!this.config.apiKey) {
      throw new Error('FLOW_API_KEY environment variable is required');
    }
  }
  
  async execute(context: FlowContext): Promise<string> {
    return await this.withRetry(() => this.executeWithTimeout(context));
  }
  
  private async withRetry<T>(operation: () => Promise<T>): Promise<T> {
    let lastError: Error;
    
    for (let attempt = 1; attempt <= this.config.retryAttempts; attempt++) {
      try {
        return await operation();
      } catch (error) {
        lastError = error;
        if (attempt < this.config.retryAttempts) {
          await this.delay(1000 * attempt); // Exponential backoff
        }
      }
    }
    
    throw lastError!;
  }
  
  private async executeWithTimeout(context: FlowContext): Promise<string> {
    return Promise.race([
      this.actualExecution(context),
      this.timeoutPromise()
    ]);
  }
  
  private async timeoutPromise(): Promise<never> {
    return new Promise((_, reject) => {
      setTimeout(() => reject(new Error('Flow execution timeout')), this.config.timeout);
    });
  }
  
  private delay(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}
```

This comprehensive API documentation provides developers with everything they need to understand, implement, and integrate with SkynetXBT flows. 
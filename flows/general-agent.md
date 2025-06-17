---
title: General Agent Flow
layout: default
parent: Flows
nav_order: 3
---

# @skynetxbt/flow-llm-agent

## Overview

LLMAgentFlow is a versatile, configurable flow within the SkynetXBT framework that provides general-purpose AI capabilities. It acts as a flexible wrapper around LLM (Large Language Model) functionality, supporting both structured and unstructured outputs for various AI tasks.

## Features

- **Dual Output Modes**: Supports both normal text responses and structured JSON outputs
- **Configurable LLM Providers**: Works with multiple AI providers (OpenAI, Azure, custom endpoints)
- **Schema Validation**: Automatic JSON schema to Zod conversion for structured outputs
- **Customizable System Prompts**: Define specific AI behaviors and personas
- **Type Safety**: Full TypeScript implementation with proper error handling
- **Flexible Configuration**: Extensive LLM configuration options

## Installation

```bash
npm install @skynetxbt/flow-llm-agent
```

## Dependencies

- `@skynetxbt/core` - Core framework functionality
- `@skynetxbt/plugin-json-to-zod` - JSON schema to Zod conversion
- `@langchain/core` - LangChain core utilities
- `zod` - Runtime type validation

## Usage

### Basic Setup

```typescript
import { LLMAgentFlow } from "@skynetxbt/flow-llm-agent";

const flow = new LLMAgentFlow({
  llmConfig: {
    provider: "openai",
    modelName: "gpt-4",
    apiKey: process.env.OPENAI_API_KEY,
    temperature: 0.7,
    maxTokens: 1000
  },
  settings: {
    type: "normal",
    systemPrompt: "You are a helpful AI assistant specialized in blockchain technology."
  }
});
```

### Normal Text Response

```typescript
const normalFlow = new LLMAgentFlow({
  llmConfig: {
    provider: "openai",
    modelName: "gpt-4",
    apiKey: process.env.OPENAI_API_KEY
  },
  settings: {
    type: "normal",
    systemPrompt: "You are SkynetXBT, a blockchain AI assistant."
  }
});

const result = await normalFlow.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "Explain what is DeFi",
  variables: {}
});

console.log(result); // Returns plain text response
```

### Structured JSON Response

```typescript
const structuredFlow = new LLMAgentFlow({
  llmConfig: {
    provider: "openai",
    modelName: "gpt-4",
    apiKey: process.env.OPENAI_API_KEY
  },
  settings: {
    type: "structured",
    systemPrompt: "You are a blockchain data analyzer.",
    schema: {
      type: "object",
      properties: {
        contractAddress: { type: "string" },
        isValid: { type: "boolean" },
        networkType: { type: "string" }
      },
      required: ["contractAddress", "isValid"]
    }
  }
});

const result = await structuredFlow.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "Analyze this contract: 0x1234567890123456789012345678901234567890",
  variables: {}
});

console.log(result); // Returns structured JSON object
```

## Configuration

### LLM Configuration Options

```typescript
interface LLMConfig {
  provider?: string;           // AI provider (openai, azure, etc.)
  modelName?: string;          // Model name (gpt-4, gpt-3.5-turbo, etc.)
  apiKey?: string;             // API key for the provider
  temperature?: number;        // Response creativity (0-1)
  maxTokens?: number;          // Maximum response length
  topP?: number;               // Nucleus sampling parameter
  frequencyPenalty?: number;   // Frequency penalty (-2 to 2)
  presencePenalty?: number;    // Presence penalty (-2 to 2)
  baseURL?: string;            // Custom API endpoint
  azureOpenAIApiInstanceName?: string;
  azureOpenAIApiDeploymentName?: string;
  azureOpenAIApiVersion?: string;
}
```

### Settings Configuration

```typescript
interface Settings {
  type: "normal" | "structured";  // Output type
  systemPrompt: string;           // AI behavior definition
  schema?: any;                   // JSON schema for structured output
  prompt?: string;                // Optional additional prompt
}
```

## Use Cases

### 1. Contract Address Extraction

```typescript
const contractExtractor = new LLMAgentFlow({
  llmConfig: { /* your config */ },
  settings: {
    type: "structured",
    systemPrompt: "Extract contract addresses from user messages.",
    schema: {
      type: "object",
      properties: {
        addresses: { 
          type: "array",
          items: { type: "string" }
        },
        count: { type: "number" }
      }
    }
  }
});
```

### 2. General Q&A Agent

```typescript
const qaAgent = new LLMAgentFlow({
  llmConfig: { /* your config */ },
  settings: {
    type: "normal",
    systemPrompt: "You are a knowledgeable blockchain expert. Answer questions clearly and concisely."
  }
});
```

### 3. Data Analysis Agent

```typescript
const dataAnalyzer = new LLMAgentFlow({
  llmConfig: { /* your config */ },
  settings: {
    type: "structured",
    systemPrompt: "Analyze the provided data and return insights.",
    schema: {
      type: "object",
      properties: {
        summary: { type: "string" },
        insights: { type: "array", items: { type: "string" } },
        recommendations: { type: "array", items: { type: "string" } }
      }
    }
  }
});
```

## API Reference

### LLMAgentFlow Class

#### Constructor
```typescript
constructor({
  llmConfig: LLMConfig,
  settings: {
    type: "normal" | "structured",
    systemPrompt: string,
    schema?: any,
    prompt?: string
  }
})
```

#### Methods

- `execute(context: FlowContext)`: Execute the flow with user input
- `executeAutonomous(config: AutonomousFlowConfig)`: Autonomous execution mode

#### Properties

- `flowConfig`: Flow configuration object
- `settings`: Flow behavior settings
- `llmConfig`: LLM provider configuration
- `schemaofSettings`: Zod schema for validation (structured mode)

## Provider Support

### OpenAI
```typescript
llmConfig: {
  provider: "openai",
  modelName: "gpt-4",
  apiKey: process.env.OPENAI_API_KEY
}
```

### Azure OpenAI
```typescript
llmConfig: {
  provider: "azure",
  azureOpenAIApiInstanceName: "your-instance",
  azureOpenAIApiDeploymentName: "your-deployment",
  azureOpenAIApiVersion: "2023-05-15",
  apiKey: process.env.AZURE_OPENAI_API_KEY
}
```

### Custom Endpoints
```typescript
llmConfig: {
  baseURL: "https://your-custom-endpoint.com",
  apiKey: "your-api-key"
}
```

## Best Practices

1. **System Prompts**: Be specific about the AI's role and expected behavior
2. **Temperature Control**: Lower values (0.1-0.3) for factual responses, higher (0.7-0.9) for creative tasks
3. **Token Limits**: Set appropriate maxTokens to control response length and costs
4. **Schema Design**: Keep structured schemas simple and well-defined
5. **Error Handling**: Always implement proper error handling for API failures

## Version

Current version: 0.0.16-alpha.0

## License

Part of the SkynetXBT framework ecosystem. 
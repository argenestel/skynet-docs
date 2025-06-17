---
title: Telegram Flow
layout: default
parent: Flows
nav_order: 4
---

# @skynetxbt/flow-telegram

## Overview

TelegramFlow is a comprehensive Telegram bot integration flow within the SkynetXBT framework that enables bidirectional communication through Telegram. It supports both triggering actions from Telegram messages and broadcasting notifications to Telegram users, making it perfect for creating interactive AI agents and notification systems.

## Features

- **Telegram Bot Integration**: Full-featured Telegram bot with command handling
- **Message Broadcasting**: Send notifications to multiple subscribers simultaneously
- **Chat Management**: Automatic subscriber management with persistent storage
- **Message Chunking**: Handles long messages by splitting them into multiple parts
- **Command Support**: Built-in commands for user interaction (/start, /stop, /help, /chatid)
- **Event-Driven Architecture**: Emits events for integration with other flows
- **Rate Limiting**: Built-in rate limiting for message sending
- **Error Handling**: Comprehensive error handling with fallback mechanisms

## Installation

```bash
npm install @skynetxbt/flow-telegram
```

## Dependencies

- `@skynetxbt/core` - Core framework functionality
- `@langchain/core` - LangChain core utilities
- `node-telegram-bot-api` - Telegram Bot API wrapper
- `fs` & `path` - File system operations for chat ID persistence

## Usage

### Basic Setup

```typescript
import { TelegramFlow } from "@skynetxbt/flow-telegram";

const telegramFlow = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    polling: true,
    isbroadcast: false,
    helpMessage: "Welcome to my custom bot!"
  }
});
```

### Broadcasting Messages

```typescript
// Setup for broadcasting
const broadcastBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    isbroadcast: true,
    polling: false
  }
});

// Send broadcast to all subscribers
await broadcastBot.sendBroadcast("🚀 Important announcement: New features available!");

// Execute flow context (will broadcast the message)
await broadcastBot.execute({
  agentId: { generation: 0, familyCode: "", serialNumber: "" },
  userPublicKey: "",
  message: "Alert: Market volatility detected!",
  variables: {}
});
```

### Direct Messaging

```typescript
// Setup for direct messaging
const directBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    chatId: "123456789", // Specific chat ID
    polling: false
  }
});

// Send to specific chat
await directBot.sendMessage("123456789", "Your personalized update is ready!");
```

### Event-Driven Integration

```typescript
// Setup as trigger flow
const triggerBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    polling: true
  }
});

// Listen for agent execution events
triggerBot.executeAgent.on("executeAgent", (context) => {
  console.log("Received message from Telegram:", context.message);
  // Process the message with other flows or agents
});

// Set flow context for message processing
triggerBot.setFlowContext([
  {
    agentId: { generation: 0, familyCode: "", serialNumber: "" },
    userPublicKey: "",
    message: "",
    variables: {}
  }
]);
```

## Configuration

### TelegramConfig Interface

```typescript
interface telegramConfig {
  bottoken: string;           // Telegram Bot API token (required)
  isbroadcast?: boolean;      // Enable broadcast mode
  chatId?: string;            // Specific chat ID for direct messages
  groupId?: string;           // Group ID for group messages
  helpMessage?: string;       // Custom help message
  polling?: boolean;          // Enable polling for incoming messages
}
```

### Configuration Examples

#### Notification Bot (Broadcast)
```typescript
{
  bottoken: "YOUR_BOT_TOKEN",
  isbroadcast: true,
  polling: false,
  helpMessage: "📢 Notification Bot - Stay updated with the latest alerts!"
}
```

#### Interactive Assistant (Polling)
```typescript
{
  bottoken: "YOUR_BOT_TOKEN",
  polling: true,
  helpMessage: "🤖 AI Assistant - Ask me anything about blockchain and DeFi!"
}
```

#### Alert System (Direct)
```typescript
{
  bottoken: "YOUR_BOT_TOKEN",
  chatId: "YOUR_CHAT_ID",
  helpMessage: "⚠️ Alert System - Receive important notifications here"
}
```

## Built-in Commands

The flow automatically handles these Telegram commands:

- `/start` - Subscribe to notifications and add user to broadcast list
- `/stop` - Unsubscribe from notifications
- `/help` - Display help message
- `/chatid` - Show user's chat ID

## Advanced Features

### Message Chunking

The flow automatically handles Telegram's 4096 character limit:

```typescript
// Long messages are automatically split
const longMessage = "A".repeat(5000); // 5000 characters
await telegramFlow.sendMessage(chatId, longMessage);
// Will be sent as: "Part 1/2" and "Part 2/2"
```

### Event Integration

```typescript
// Set up event-driven processing
telegramFlow.executeAgent.on("executeAgent", async (context) => {
  // Process with AI agent
  const llmManager = new LLMManager({
    systemPrompt: "You are a helpful blockchain assistant"
  });
  
  const response = await llmManager.invoke(context.message);
  
  // Send response back
  await telegramFlow.sendBroadcast(response);
});
```

## Use Cases

### 1. DeFi Alert System

```typescript
const defiAlerts = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    isbroadcast: true,
    helpMessage: "🔔 DeFi Alert System - Get notified of price movements and opportunities!"
  }
});

// Send price alerts
await defiAlerts.sendBroadcast("🚨 ETH price dropped 5% in the last hour!");
```

### 2. AI Trading Assistant

```typescript
const tradingBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    polling: true,
    helpMessage: "📈 AI Trading Assistant - Ask me about market conditions!"
  }
});

// Handle trading queries
tradingBot.executeAgent.on("executeAgent", async (context) => {
  // Process trading query with AI
  const analysis = await processTradeQuery(context.message);
  await tradingBot.sendMessage(context.userPublicKey, analysis);
});
```

### 3. Portfolio Tracker

```typescript
const portfolioBot = new TelegramFlow({
  telegramConfig: {
    bottoken: process.env.TELEGRAM_BOT_TOKEN!,
    polling: true,
    helpMessage: "💼 Portfolio Tracker - Monitor your crypto investments!"
  }
});
```

## API Reference

### TelegramFlow Class

#### Methods

- `sendMessage(chatId: string, message: string)`: Send message to specific chat
- `sendBroadcast(message: string)`: Send message to all subscribers
- `execute(context: FlowContext)`: Execute flow with context
- `setFlowContext(flowContext: FlowContext[])`: Set flow contexts for message processing
- `removeFlowContext(flowContext: FlowContext)`: Remove flow context
- `getUpdates()`: Get recent Telegram updates
- `processMessage(message: any)`: Process incoming message manually

#### Properties

- `bot`: TelegramBot instance
- `executeAgent`: EventEmitter for integration events
- `chatIds`: Array of subscribed chat IDs
- `flowContext`: Array of flow contexts for processing

#### Events

- `executeAgent`: Emitted when a message is received (polling mode)

## Security Considerations

- **Bot Token Security**: Store bot tokens securely using environment variables
- **Rate Limiting**: Built-in rate limiting prevents API abuse
- **Chat ID Validation**: Validates chat IDs before sending messages
- **Error Handling**: Comprehensive error handling prevents crashes

## File Storage

The flow automatically manages subscriber data:
- Chat IDs are stored in `chatIds.json`
- Persistent across restarts
- Automatic backup and recovery

## Version

Current version: 0.0.17-alpha.0

## License

Part of the SkynetXBT framework ecosystem. 
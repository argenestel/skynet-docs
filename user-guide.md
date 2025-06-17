---
title: User Guide
layout: default
nav_order: 2
has_children: true
---

# User Guide

Welcome to the SkynetXBT User Guide. This section provides non-technical documentation for using SkynetXBT flows and agents. Here you'll find information about flow inputs, outputs, and how to interact with the system.

## What You'll Find Here

- **Flow Catalog**: Complete list of available flows with their capabilities
- **Input/Output Specifications**: What each flow expects and returns
- **Usage Examples**: Practical examples for each flow
- **Configuration Guides**: How to set up and configure flows for your needs

## Quick Start for Users

### Using the Flows Store CLI

The easiest way to interact with SkynetXBT is through the Flows Store:

```bash
# Install the flows store
npm install -g @skynetxbt/flows-store

# See all available flows
npm run flows list

# Run a flow with a message
npm run flows run <flow-name> --message "Your message here"
```

### Understanding Flow Execution

All flows follow the same basic pattern:

1. **Input**: You provide a message or query
2. **Processing**: The flow processes your request
3. **Output**: You receive results in text, JSON, or other formats

## Flow Categories

### 🔗 **DeFi & Finance Flows**
Analyze markets, find yield opportunities, and interact with DeFi protocols.

### 🤖 **AI & Automation Flows**  
Leverage AI for various tasks like analysis, conversation, and data processing.

### 📱 **Communication Flows**
Send notifications, create bots, and manage communications.

### ⛓️ **Blockchain Flows**
Interact with smart contracts and blockchain networks.

### 🔧 **Utility Flows**
Handle data processing, HTTP requests, and other utility functions.

## Getting Help

- Check individual flow documentation for specific usage instructions
- Use the `--help` flag with CLI commands
- Refer to the Developer Guide for technical implementation details

---

**Next**: Browse the [Flow Catalog]({{ site.baseurl }}/user-guide/flows/) to see all available flows and their capabilities. 
---
title: Flow Catalog
layout: default
parent: User Guide
nav_order: 1
---

# Flow Catalog

Complete catalog of available SkynetXBT flows with their inputs, outputs, and usage examples.

## 🔗 DeFi & Finance Flows

### Arcadia Finance Flow
**Purpose**: Find the best yield opportunities on Arcadia Finance (Base chain)

**Input**:
- **Type**: Text message
- **Examples**:
  - "Show me the best yields on base chain"
  - "What are the top 5 arcadia pools by APR?"
  - "Find the most liquid pools on arcadia"

**Output**:
- **Type**: Markdown formatted text
- **Contains**: Pool information, APR data, yield analysis, AI-generated insights

**Sample Output**:
```markdown
# Arcadia Finance Yield Analysis

## Top Pools by APR
1. **USDC-ETH Pool** - 12.5% APR
2. **DAI-USDC Pool** - 10.2% APR
3. **WETH-BTC Pool** - 9.8% APR

## Recommendations
Based on current market conditions, the USDC-ETH pool offers...
```

---

### DeFi Llama Flow
**Purpose**: Analyze DeFi protocols and total value locked (TVL)

**Input**:
- **Type**: Text message
- **Examples**:
  - "Show me protocols with highest TVL"
  - "Analyze Uniswap performance"
  - "DeFi trends this week"

**Output**:
- **Type**: JSON object or formatted text
- **Contains**: Protocol data, TVL statistics, yield information

---

### Beefy Suggestions Flow
**Purpose**: Get intelligent yield farming strategy recommendations

**Input**:
- **Type**: Text message
- **Examples**:
  - "Best yield farming strategies"
  - "Safe farming options for conservative investors"
  - "High-risk high-reward farms"

**Output**:
- **Type**: Markdown formatted recommendations
- **Contains**: Strategy analysis, risk assessment, yield projections

---

## 🤖 AI & Automation Flows

### General Agent Flow
**Purpose**: AI-powered conversations and data processing

**Input**:
- **Type**: Text message or structured query
- **Examples**:
  - "Explain what is DeFi"
  - "Analyze this smart contract address: 0x123..."
  - "What are the current crypto trends?"

**Output**:
- **Type**: Text response OR structured JSON (configurable)
- **Text Mode**: Natural language response
- **Structured Mode**: JSON with specific fields

**Sample Text Output**:
```
DeFi (Decentralized Finance) refers to financial services built on blockchain...
```

**Sample Structured Output**:
```json
{
  "contractAddress": "0x1234...",
  "isValid": true,
  "networkType": "ethereum",
  "analysis": "This appears to be an ERC-20 token contract..."
}
```

---

### MCP Agent Flow
**Purpose**: Model Context Protocol integration

**Input**:
- **Type**: MCP protocol messages
- **Format**: Structured MCP commands

**Output**:
- **Type**: MCP protocol responses
- **Contains**: Processed MCP data

---

## 📱 Communication Flows

### Telegram Flow
**Purpose**: Send messages and notifications via Telegram

**Input**:
- **Type**: Text message
- **Configuration**: Bot token, chat settings
- **Examples**:
  - "Alert: ETH price dropped 5%"
  - "Portfolio update: Your holdings increased by 2%"

**Output**:
- **Type**: Message delivery confirmation
- **Side Effect**: Message sent to Telegram chat(s)

**Configuration Options**:
- **Broadcast Mode**: Send to all subscribers
- **Direct Mode**: Send to specific chat ID
- **Interactive Mode**: Receive and respond to messages

---

## ⛓️ Blockchain Flows

### Ethers Flow
**Purpose**: Interact with smart contracts using natural language

**Input**:
- **Type**: Natural language contract interaction requests
- **Examples**:
  - "Transfer 100 tokens to 0x123..."
  - "Check my token balance"
  - "Approve spending of 50 USDC"
  - "Call the increment function"

**Output**:
- **Type**: Transaction hash (for state-changing operations) or data (for view operations)
- **Examples**:
  - `"0xabc123..."` (transaction hash)
  - `"1000"` (balance query result)

**Requirements**:
- Smart contract address
- Contract ABI (Application Binary Interface)
- Private key for transaction signing
- RPC endpoint URL

---

## 🔧 Utility Flows

### Fetch Flow
**Purpose**: Make HTTP requests and fetch data

**Input**:
- **Type**: URL and request parameters
- **Examples**:
  - "Fetch data from https://api.example.com"
  - "Get current price from CoinGecko API"

**Output**:
- **Type**: HTTP response data (JSON, text, etc.)
- **Contains**: Fetched data from the specified endpoint

---

### Webhook Flow
**Purpose**: Handle incoming webhook requests

**Input**:
- **Type**: HTTP POST requests
- **Contains**: Webhook payload data

**Output**:
- **Type**: HTTP response
- **Contains**: Processing confirmation or data

---

### Timer Flow
**Purpose**: Execute scheduled tasks

**Input**:
- **Type**: Schedule configuration
- **Examples**:
  - "Run every 5 minutes"
  - "Execute daily at 9 AM"

**Output**:
- **Type**: Execution confirmation
- **Contains**: Task completion status

---

### Parser Flow
**Purpose**: Parse and transform data

**Input**:
- **Type**: Raw data (JSON, CSV, XML, etc.)
- **Examples**:
  - CSV data to be converted to JSON
  - HTML content to extract specific information

**Output**:
- **Type**: Parsed/transformed data
- **Format**: Depends on parsing configuration

---

## 📊 Research & Analysis Flows

### Hyperbrowser Research Flow
**Purpose**: Automated web research and data extraction

**Input**:
- **Type**: Research query or URL
- **Examples**:
  - "Research latest DeFi protocols launched this month"
  - "Extract data from https://example.com"

**Output**:
- **Type**: Structured research data
- **Contains**: Extracted information, analysis, and insights

---

## 🔀 Advanced Flows

### Conditional Flow
**Purpose**: Execute conditional logic and branching

**Input**:
- **Type**: Conditions and flow paths
- **Examples**:
  - "If price > $2000 then send alert"
  - "When volume > 1M then execute trade"

**Output**:
- **Type**: Result of executed branch
- **Contains**: Output from the selected conditional path

---

### Output Flow
**Purpose**: Format and output data

**Input**:
- **Type**: Raw data and formatting instructions
- **Examples**:
  - "Format this data as a table"
  - "Export to CSV"

**Output**:
- **Type**: Formatted data
- **Format**: As specified in formatting instructions

---

### GOAT SDK Flow
**Purpose**: GOAT SDK integrations

**Input**:
- **Type**: GOAT SDK commands
- **Examples**: (Specific to GOAT SDK functionality)

**Output**:
- **Type**: GOAT SDK responses
- **Contains**: SDK operation results

---

## Usage Patterns

### Single Flow Execution
```bash
npm run flows run <flow-name> --message "Your input message"
```

### Chaining Flows
Some flows can be chained together by using the output of one flow as input to another.

### Configuration
Most flows support configuration through environment variables or configuration files. Check individual flow documentation for specific requirements.

### Error Handling
All flows return error information if something goes wrong:
- **Type**: Error message
- **Contains**: Description of what went wrong and potential solutions 
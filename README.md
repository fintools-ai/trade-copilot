# 🚀 Trade Copilot

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MCP Protocol](https://img.shields.io/badge/MCP-1.0.0-green.svg)](https://modelcontextprotocol.io/)
[![AWS Bedrock](https://img.shields.io/badge/AWS-Bedrock-orange.svg)](https://aws.amazon.com/bedrock/)
[![Anthropic Claude](https://img.shields.io/badge/Anthropic-Claude-purple.svg)](https://www.anthropic.com/)

**AI-powered trading chat interface with real-time market analysis**

Trade Copilot is a trading assistant that combines institutional-grade market analysis with cutting-edge AI. Built on the Model Context Protocol (MCP), it provides real-time order flow analysis, options monitoring, and technical analysis through a clean, professional chat interface.


## ✨ Key Features

### 🎯 **Institutional-Grade Analysis**
- **Real-time Order Flow Analysis** - Track institutional vs retail sentiment
- **Options Flow Monitoring** - Detect sweeps, blocks, and unusual activity  
- **Volume Profile Analysis** - Identify key support/resistance levels (POC, VAH, VAL)
- **Technical Analysis Suite** - ORB, FVG, technical zones, and indicators
- **Parallel Tool Execution** - Sub-30 second comprehensive market analysis

### 🔄 **Multi-Provider LLM Support**
- **AWS Bedrock** - Claude Sonnet 4, Claude 3.5 Sonnet, Claude Haiku
- **Anthropic API** - Direct Claude API integration
- **Runtime Switching** - Change providers/models without restart
- **Optimized Prompts** - Military-precision system prompts for speed and accuracy

### ⚡ **High-Performance Architecture**
- **MCP Protocol** - Native integration with market data servers
- **Go-based Data Broker** - Sub-10ms latency for real-time data
- **Concurrent Processing** - Parallel tool execution for maximum speed
- **Professional Interface** - Rich terminal UI with formatted responses

## 🛠️ Installation

### Prerequisites
- Python 3.11+
- AWS credentials (for Bedrock)
- Anthropic API key (for Claude API)
- Market data API keys

### Quick Start

```bash
# Clone the repository
git clone https://github.com/your-org/trade-copilot.git
cd trade-copilot

# Install dependencies
pip install -r requirements.txt

# Or for development
pip install -r requirements-dev.txt

# Copy environment file
cp .env.example .env

# Edit configuration
nano config/config.yaml
```


## ⚙️ Configuration

### Multi-Provider Setup

```yaml
# config/config.yaml
llm_providers:
  bedrock:
    region: "us-east-1"
    models:
      claude_sonnet_4: "bedrock.anthropic.claude-sonnet-4-20250514"
      claude_sonnet_35: "anthropic.claude-3-5-sonnet-20241022-v2:0"
      claude_haiku: "anthropic.claude-3-haiku-20240307-v1:0"
  
  claude_api:
    models:
      claude_sonnet_4: "claude-3-5-sonnet-20250514"
      claude_sonnet_35: "claude-3-5-sonnet-20241022"
      claude_haiku: "claude-3-haiku-20240307"

# Default settings
default_provider: "bedrock"
default_model: "claude_sonnet_4"
```

### MCP Server Configuration

```yaml
mcp_servers:
  market_data:
    command: "python"
    args: ["-m", "mcp_market_data_server"]
    env:
      TWELVE_DATA_API_KEY: "${TWELVE_DATA_API_KEY}"
  
  order_flow:
    command: "python"
    args: ["-m", "mcp_order_flow_server"]
    env:
      GRPC_BROKER_URL: "localhost:9090"
      
  options_flow:
    command: "python"
    args: ["-m", "mcp_options_order_flow_server"]
    env:
      GRPC_BROKER_URL: "localhost:9090"
```

## 🚀 Usage

### Basic Commands

```bash
# Start chat interface
trade-copilot chat

# Use specific provider/model
trade-copilot chat --provider claude_api --model claude_sonnet_4

```

### Interactive Commands

```bash
# In chat interface
help                           # Show help
status                        # System status
clear                         # Clear history
switch bedrock claude_sonnet_4 # Switch provider
exit                          # End session
```

## 📊 Sample Trading Queries & Expected Responses

### Real-Time Analysis Queries

#### **Basic Order Flow Analysis**
```bash
Query: "Analyze SPY order flow for the last 10 minutes"

Expected Tools Called:
├─ analyze_order_flow_tool(ticker="SPY", minutes=10)
├─ financial_volume_profile(ticker="SPY", timeframe="1m") 
└─ financial_technical_analysis(ticker="SPY", timeframes=["1m", "5m"])

Sample Response:
**SIGNAL:** BUY CALLS
**CONVICTION:** 78/100 (3 confirming signals)
**ENTRY:** $472.25
**STOP:** $469.80 (2% risk)
**TARGET:** $477.15 (2:1 R/R)
**TIMEFRAME:** 15 minutes validity
**EVIDENCE:**
- Order flow: 68% institutional buying pressure, large block at 472.00
- Volume profile: POC at 471.95 acting as support, 89% buy volume above POC
- Technical: Clean break above 5min EMA with increasing momentum
```

#### **Options Flow Analysis**
```bash
Query: "Show me NVDA options unusual activity and sweeps"

Expected Tools Called:
├─ get_options_flow(ticker="NVDA", history_minutes=30)
├─ configure_options_monitoring_tool(ticker="NVDA", min_premium=50000)
└─ analyze_order_flow_tool(ticker="NVDA", minutes=15)

Sample Response:
**SIGNAL:** WAIT - Mixed signals
**CONVICTION:** 45/100 (conflicting data)
**ANALYSIS:**
- Options: $2.3M in call sweeps at 520 strike (2x normal), but also 1.2M put volume
- Unusual activity: 400% increase in 525C volume, premium suggesting 3% move expected
- Underlying flow: Retail buying pressure (78%), institutions net selling
**INVALIDATION:** Clear above 522.50 = bullish confirmation
```

#### **Multi-Timeframe Technical**
```bash
Query: "Check QQQ opening range breakout and volume profile"

Expected Tools Called:
├─ financial_orb_analysis(ticker="QQQ", orb_minutes=30)
├─ financial_volume_profile(ticker="QQQ", timeframe="5m")
├─ financial_technical_zones(ticker="QQQ")
└─ financial_technical_analysis(ticker="QQQ")

Sample Response:
**SIGNAL:** BUY CALLS
**CONVICTION:** 82/100 (4 confirming signals)
**ENTRY:** $398.75 (ORB high + 0.05)
**STOP:** $396.90 (ORB low - 0.10)
**TARGET:** $402.45 (1.2x ORB range)
**EVIDENCE:**
- ORB: Clean breakout above 30min range (396.95-398.70), volume 2.1x average
- Volume profile: 73% volume above POC (397.85), strong buying interest
- Technical zones: Breaking resistance cluster at 398.60-398.80
- ADX: 28.4 showing strong trend, RSI healthy at 58.7
```


### Advanced Multi-Asset Queries

#### **Sector Rotation Analysis**
```bash
Query: "Compare tech vs financials order flow - XLK vs XLF"

Expected Tools Called:
├─ analyze_order_flow_tool(ticker="XLK", minutes=20)
├─ analyze_order_flow_tool(ticker="XLF", minutes=20)  
├─ financial_volume_profile(ticker="XLK")
├─ financial_volume_profile(ticker="XLF")
└─ sector_comparison_analysis(tickers=["XLK", "XLF"])

Sample Response:
**SECTOR ROTATION DETECTED:**
**XLK (Tech):** 72% institutional selling, volume 1.8x average
**XLF (Financials):** 81% institutional buying, volume 2.3x average
**ROTATION SIGNAL:** Tech → Financials rotation confirmed
**TRADE IDEA:** Long XLF, Short XLK (pairs trade)
```


## 🏗️ System Architecture

### Complete Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                 CLIENT INTERACTION                                   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  User Query: "Analyze SPY order flow and options activity"                          │
│                                        │                                            │
│                                        ▼                                            │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                              TRADE COPILOT CORE                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                        CHAT INTERFACE (Rich Terminal)                       │   │
│  │  • Input validation and parsing                                             │   │
│  │  • Real-time response streaming                                             │   │
│  │  • Conversation history persistence (SQLite)                               │   │
│  │  • Model switching commands                                                 │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                            │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                      LLM PROVIDER MANAGER                                   │   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────┐     │   │
│  │  │   AWS Bedrock   │    │   Claude API    │    │  Model Switching    │     │   │
│  │  │   (boto3)       │    │  (anthropic-sdk)│    │   (Runtime)         │     │   │
│  │  │                 │    │                 │    │                     │     │   │
│  │  │ • Claude-3.5    │    │ • Claude-3.5    │    │ • Provider select   │     │   │
│  │  │ • Claude Haiku  │    │ • Claude Haiku  │    │ • Model switching   │     │   │
│  │  │ • Auto-scaling  │    │ • Direct API    │    │ • Config hot-reload │     │   │
│  │  └─────────────────┘    └─────────────────┘    └─────────────────────┘     │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                            │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                         MCP TOOL MANAGER                                    │   │
│  │  • Tool discovery and registration                                         │   │
│  │  • Parallel tool execution (15+ concurrent)                               │   │
│  │  • Error handling and retry logic                                         │   │
│  │  • Tool result aggregation and formatting                                 │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                          PARALLEL TOOL EXECUTION
                                        │
              ┌─────────────────────────┼─────────────────────────┐
              ▼                         ▼                         ▼
┌─────────────────────────┐  ┌─────────────────────────┐  ┌─────────────────────────┐
│    MCP MARKET DATA      │  │   MCP ORDER FLOW        │  │  MCP OPTIONS FLOW       │
│       SERVER            │  │      SERVER             │  │      SERVER             │
├─────────────────────────┤  ├─────────────────────────┤  ├─────────────────────────┤
│ TOOLS:                  │  │ TOOLS:                  │  │ TOOLS:                  │
│ • financial_technical_  │  │ • analyze_order_flow_   │  │ • get_options_flow      │
│   analysis              │  │   tool                  │  │ • configure_options_    │
│ • financial_volume_     │  │ • get_level_2_data      │  │   monitoring_tool       │
│   profile               │  │ • get_trade_prints      │  │ • get_unusual_activity  │
│ • financial_technical_  │  │ • detect_patterns       │  │ • analyze_sweep_blocks  │
│   zones                 │  │                         │  │                         │
│ • financial_orb_        │  │ DATA SOURCES:           │  │ DATA SOURCES:           │
│   analysis              │  │ • Real-time quotes      │  │ • Options chains        │
│ • financial_fvg_        │  │ • Level 2 book data     │  │ • Unusual activity      │
│   analysis              │  │ • Trade patterns        │  │ • Greeks calculations   │
│                         │  │ • Volume analysis       │  │ • Flow direction        │
│ DATA SOURCES:           │  │                         │  │                         │
│ • Twelve Data API       │  │ ┌─────────────────────┐ │  │ ┌─────────────────────┐ │
│ • Technical indicators  │  │ │    gRPC CLIENT      │ │  │ │    gRPC CLIENT      │ │
│ • Volume calculations   │  │ │   localhost:9090    │ │  │ │   localhost:9090    │ │
│ • Multi-timeframe       │  │ └─────────────────────┘ │  │ └─────────────────────┘ │
└─────────────────────────┘  └─────────────────────────┘  └─────────────────────────┘
                                        │                         │
                          gRPC CONNECTION (sub-10ms latency)      │
                                        │                         │
                                        ▼                         ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                        MCP TRADING DATA BROKER (Go)                                 │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                          CORE SERVICES                                      │   │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────────┐  │   │
│  │  │   Order Flow    │  │  Options Flow   │  │     Market Data Cache       │  │   │
│  │  │   Aggregator    │  │   Processor     │  │                             │  │   │
│  │  │                 │  │                 │  │ • Real-time price feeds     │  │   │
│  │  │ • Pattern       │  │ • Sweep/Block   │  │ • Historical OHLCV          │  │   │
│  │  │   detection     │  │   detection     │  │ • Level 2 order book       │  │   │
│  │  │ • Institutional │  │ • Unusual       │  │ • Trade tape analysis      │  │   │
│  │  │   vs retail     │  │   activity      │  │ • Volume profile cache     │  │   │
│  │  │ • Volume        │  │ • Greeks        │  │                             │  │   │
│  │  │   analysis      │  │   calculation   │  │                             │  │   │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                            │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                      WEBSOCKET CLIENT POOL                                  │   │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────────┐  │   │
│  │  │   3P WS         │  │   Market WS     │  │    Health Monitor           │  │   │
│  │  │   Client        │  │   Client        │  │                             │  │   │
│  │  │                 │  │                 │  │ • Connection monitoring     │  │   │
│  │  │ • Real-time     │  │ • Level 2 data  │  │ • Auto-reconnection         │  │   │
│  │  │   options data  │  │ • Trade prints  │  │ • Latency tracking          │  │   │
│  │  │ • Order flow    │  │ • Quote stream  │  │ • Error logging             │  │   │
│  │  │ • Auto-retry    │  │ • Volume feeds  │  │ • Performance metrics       │  │   │
│  │  │ • 60s timeout   │  │                 │  │                             │  │   │
│  │  │   monitoring    │  │                 │  │                             │  │   │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                           EXTERNAL DATA INGESTION
                                        │
              ┌─────────────────────────┼─────────────────────────┐
              ▼                         ▼                         ▼
┌─────────────────────────┐  ┌─────────────────────────┐  ┌─────────────────────────┐
│      3P API             │  │    TWELVE DATA API      │  │   POLYGON/OTHER APIs    │
├─────────────────────────┤  ├─────────────────────────┤  ├─────────────────────────┤
│ • Real-time options     │  │ • Stock market data     │  │ • Alternative feeds     │
│ • Order flow data       │  │ • Technical indicators  │  │ • News sentiment        │
│ • Institutional trades  │  │ • Historical OHLCV      │  │ • Economic data         │
│ • Unusual activity      │  │ • Volume data           │  │ • Social sentiment      │
│ • WebSocket streams     │  │ • Fundamental data      │  │                         │
└─────────────────────────┘  └─────────────────────────┘  └─────────────────────────┘
```

### Configuration Management

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                             CONFIGURATION SYSTEM                                    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                    /etc/config/TradingInsightsAgentConfig.json               │   │
│  │  {                                                                          │   │
│  │    "ANTHROPIC_API_KEY": "sk-ant-...",                                      │   │
│  │    "CLAUDE_API_KEY": "sk-ant-...",                                         │   │
│  │    "3P   _API_KEY": "3p_key_...",                                          │   │
│  │    "TWELVE_DATA_API_KEY": "td_key_...",                                    │   │
│  │    "AWS_ACCESS_KEY_ID": "AKIA...",                                         │   │
│  │    "AWS_SECRET_ACCESS_KEY": "...",                                         │   │
│  │    "GRPC_BROKER_URL": "localhost:9090"                                     │   │
│  │  }                                                                          │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                            │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                        config/config.yaml                                   │   │
│  │  llm_providers:                                                             │   │
│  │    bedrock:                                                                 │   │
│  │      region: "us-east-1"                                                   │   │
│  │      models:                                                                │   │
│  │        claude_sonnet_4: "bedrock.anthropic.claude-sonnet-4-20250514"      │   │
│  │    claude_api:                                                              │   │
│  │      models:                                                                │   │
│  │        claude_sonnet_4: "claude-3-5-sonnet-20250514"                      │   │
│  │                                                                             │   │
│  │  mcp_servers:                                                               │   │
│  │    market_data: "python -m mcp_market_data_server"                        │   │
│  │    order_flow: "python -m mcp_order_flow_server"                          │   │
│  │    options_flow: "python -m mcp_options_order_flow_server"                │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### Data Flow & Execution Pipeline

```
USER QUERY → INPUT VALIDATION → LLM ANALYSIS → TOOL EXECUTION → RESPONSE GENERATION
     │              │                │              │                    │
     ▼              ▼                ▼              ▼                    ▼
┌─────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ "SPY    │  │ Parse &     │  │ Claude      │  │ Parallel    │  │ Aggregate   │
│ options │  │ validate    │  │ decides     │  │ MCP tool    │  │ results &   │
│ flow"   │  │ ticker,     │  │ which tools │  │ execution:  │  │ format for  │
│         │  │ timeframe   │  │ to call     │  │ • Options   │  │ trading     │
│         │  │ params      │  │ based on    │  │ • Order     │  │ decision    │
│         │  │             │  │ context     │  │ • Volume    │  │             │
└─────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘
                                                      │
                                         CONCURRENT EXECUTION
                                              (Sub-30s total)
```

### Example Query Workflow

**User Input:** `"Analyze SPY order flow and options activity"`

**Step 1: Query Processing (50ms)**
- Input validation and ticker extraction
- Context analysis for tool selection
- Historical conversation context loading

**Step 2: LLM Tool Selection (200ms)**
- Claude analyzes query requirements
- Selects 3-5 relevant tools based on system prompt
- Plans parallel execution strategy

**Step 3: Parallel Tool Execution (1-2s)**
```
TOOL 1: analyze_order_flow_tool(ticker="SPY", minutes=10)
     ├─ gRPC call to data broker
     ├─ Real-time order flow aggregation  
     └─ Pattern detection & sentiment analysis

TOOL 2: get_options_flow(ticker="SPY", history_minutes=20)
     ├─ Options chain analysis
     ├─ Unusual activity detection
     └─ Sweep/block identification

TOOL 3: financial_volume_profile(ticker="SPY", timeframe="5m")
     ├─ Twelve Data API call
     ├─ Volume profile calculation
     └─ POC/VAH/VAL identification

TOOL 4: financial_technical_analysis(ticker="SPY")
     ├─ Multi-timeframe indicator analysis
     ├─ Trend strength assessment
     └─ Support/resistance levels
```

```

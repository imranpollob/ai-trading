# Trading Simulation Platform

This project is an educational and research-focused equity trading simulation platform that showcases the power of autonomous agents, multi-agent collaboration, and modular microservice (MCP) architecture. Inspired by legendary investors, the platform features a team of trader agents—each with unique, evolving strategies—operating in a realistic, event-driven market environment.

## Features
- Simulate multiple autonomous trader agents (e.g., Warren, George, Ray, Cathie) with distinct investment philosophies and the ability to adapt strategies over time.
- Researcher agent provides market research and news analysis to inform trading decisions.
- Modular MCP servers (accounts, market data, push notifications, memory, etc.) provide tools and resources for agents.
- Real-time Gradio dashboard for visualizing trading activity, agent decisions, portfolio evolution, holdings, transactions, and logs.
- Support experimentation with algorithmic trading, agent-based systems, and financial AI in a safe, extensible environment.
- All data is stored in a local SQLite database for durability and analysis.
- Easily add new agents, strategies, or data sources.
- Facilitate hands-on learning and research in finance, AI, and multi-agent orchestration.

---

## Table of Contents
- [Trading Simulation Platform](#trading-simulation-platform)
  - [Features](#features)
  - [Table of Contents](#table-of-contents)
  - [Architecture](#architecture)
  - [How It Works](#how-it-works)
  - [Setup \& Installation](#setup--installation)
  - [Configuration](#configuration)
  - [Usage](#usage)
  - [Key Components](#key-components)
  - [Contributing](#contributing)
  - [License](#license)

---

## Architecture

```
+-------------------+         +-------------------+         +-------------------+
|                   |         |                   |         |                   |
|   Gradio UI       | <-----> |   SQLite DB       | <-----> |   MCP Servers     |
|   (app.py)        |         |   (database.py)   |         |   (accounts,      |
|                   |         |                   |         |    market, push)  |
+-------------------+         +-------------------+         +-------------------+
         ^                           ^                               ^
         |                           |                               |
         |                           |                               |
         |                  +-------------------+                   |
         |                  |                   |                   |
         +------------------+   Trader Agents   +-------------------+
                            |   (traders.py)   |
                            +-------------------+
```

- **Gradio UI (`app.py`):** Interactive dashboard for monitoring all traders.
- **Trader Agents (`traders.py`):** Autonomous agents making trading decisions.
- **MCP Servers:** Microservices for accounts, market data, and notifications.
- **Database (`database.py`):** SQLite-based persistence for all trading data.
- **Market Data (`market.py`):** Integrates with Polygon.io or simulates prices.
- **Templates (`templates.py`):** Dynamic prompts and instructions for agents.

---

## How It Works
1. **Initialization:**
   - Environment variables and API keys are loaded from `.env`.
   - MCP servers for accounts, market, and notifications are started.
2. **Agent Execution:**
   - Trader agents periodically research, analyze, and execute trades based on their strategies.
   - Agents interact with MCP servers for account management and market data.
   - All actions and state changes are logged to the database.
3. **Visualization:**
   - The Gradio dashboard provides real-time insights into each trader’s performance, portfolio, and activity logs.

---

## Setup & Installation

### Prerequisites

Before you begin, ensure you have the following installed:
- **Python 3.10+** (recommended: Python 3.11 or 3.12)
- **Node.js 18+** and **npm** (for MCP servers)
- **Git** (for cloning the repository)

### Step 1: Clone the Repository

```sh
git clone <your-repo-url>
cd ai-trading
```

### Step 2: Create a Virtual Environment (Recommended)

```sh
# Create virtual environment
python -m venv venv

# Activate on Linux/macOS
source venv/bin/activate

# Activate on Windows
venv\Scripts\activate
```

### Step 3: Install Python Dependencies

```sh
pip install -r requirements.txt
```

This will install:
- Gradio (web UI)
- Pandas & Plotly (data processing and visualization)
- Polygon API client (market data)
- OpenAI/Anthropic SDKs (AI models)
- MCP (Model Context Protocol support)
- Additional utilities

### Step 4: Install MCP Server Dependencies

This project uses MCP servers for modular services. Install the required Node.js tools:

```sh
# Verify Node.js and npm are installed
node --version
npm --version

# The MCP servers will be automatically installed when needed, but you can pre-install:
npx -y @modelcontextprotocol/server-brave-search
npx -y mcp-memory-libsql
```

You may also need **uv** (Python package manager for some MCP servers):

```sh
pip install uv
```

### Step 5: Configure Environment Variables

Create a `.env` file in the project root with your API keys:

```sh
cp .env .env.backup  # Backup existing file if you have one
```

Edit `.env` and add your API keys:

```env
# Market Data (Required)
POLYGON_API_KEY=your_actual_polygon_api_key
POLYGON_PLAN=free                    # Options: free, paid, realtime

# Push Notifications (Optional)
PUSHOVER_USER=your_pushover_user_key
PUSHOVER_TOKEN=your_pushover_token

# AI Model API Keys (At least one required)
OPENAI_API_KEY=your_openai_api_key   # For GPT models
DEEPSEEK_API_KEY=your_deepseek_key   # Alternative AI provider
GOOGLE_API_KEY=your_google_key       # For Gemini models
GROK_API_KEY=your_grok_key           # For X.AI Grok
OPENROUTER_API_KEY=your_openrouter_key  # Access multiple models

# Web Search (Optional but recommended)
BRAVE_API_KEY=your_brave_api_key

# Trading Configuration
RUN_EVERY_N_MINUTES=60               # How often to run trading cycles
RUN_EVEN_WHEN_MARKET_IS_CLOSED=false # Set to true for testing
USE_MANY_MODELS=false                # Use different AI models per trader
```

**Where to get API keys:**
- **Polygon.io**: https://polygon.io/ (free tier available)
- **OpenAI**: https://platform.openai.com/api-keys
- **DeepSeek**: https://platform.deepseek.com/
- **Google AI**: https://ai.google.dev/
- **Grok (X.AI)**: https://x.ai/
- **OpenRouter**: https://openrouter.ai/
- **Brave Search**: https://brave.com/search/api/
- **Pushover**: https://pushover.net/

### Step 6: Initialize the Database

The database will be created automatically on first run, but you can initialize it manually:

```sh
python database.py
```

This creates `accounts.db` with tables for traders, holdings, transactions, and logs.

---

## Configuration

All configuration is managed via environment variables. See `.env.example` for required and optional settings:
- API keys for Polygon.io, Pushover, DeepSeek, Google, Grok, OpenRouter, Brave
- Trading simulation parameters (intervals, model selection, etc.)

---

## Usage

### Running the Trading Platform

You'll need **two terminal windows** to run the platform:

#### Terminal 1: Start the Trading Floor (Agent Scheduler)

```sh
# Activate your virtual environment first
source venv/bin/activate  # On Linux/macOS
# or: venv\Scripts\activate  # On Windows

# Run the trading floor
python trading_floor.py
```

This will:
- Initialize all trader agents (Warren, George, Ray, Cathie)
- Start MCP servers for accounts, market data, and notifications
- Begin executing trading cycles based on `RUN_EVERY_N_MINUTES`
- Log all activity to the console and database

#### Terminal 2: Launch the Gradio Dashboard

```sh
# In a new terminal, activate your virtual environment
source venv/bin/activate  # On Linux/macOS
# or: venv\Scripts\activate  # On Windows

# Run the dashboard
python app.py
```

The dashboard will automatically open in your browser (typically at `http://localhost:7860`).

### What You'll See

The Gradio dashboard provides:
- **Portfolio Overview**: Current value and P&L for each trader
- **Holdings**: Real-time positions and performance
- **Transactions**: Trade history with timestamps
- **Activity Logs**: Agent decisions and reasoning
- **Performance Charts**: Visual tracking of portfolio evolution

### Testing and Development

For rapid testing, you can modify `.env`:

```env
RUN_EVERY_N_MINUTES=5                # Run trades every 5 minutes
RUN_EVEN_WHEN_MARKET_IS_CLOSED=true  # Allow trading 24/7 for testing
USE_MANY_MODELS=true                 # Use different AI models per trader
```

### Stopping the Platform

- Press `Ctrl+C` in each terminal to stop the services gracefully
- All data is automatically saved to `accounts.db`

---

## Troubleshooting

### Common Issues

**"ModuleNotFoundError" or "ImportError"**
```sh
# Ensure virtual environment is activated and dependencies are installed
pip install -r requirements.txt --upgrade
```

**"API key not found" or authentication errors**
- Verify `.env` file exists in the project root
- Check that API keys are valid and not expired
- Ensure no quotes around values in `.env`

**MCP server errors**
```sh
# Install MCP servers manually
npx -y @modelcontextprotocol/server-brave-search
npx -y mcp-memory-libsql
uvx mcp-server-fetch

# Install uv if needed
pip install uv
```

**Database locked or corruption**
```sh
# Stop all processes, then reset the database
rm accounts.db
python database.py
```

**Gradio port already in use**
- Change the port in `app.py`: modify `ui.launch()` to `ui.launch(server_port=7861)`

**Market data errors (Polygon.io)**
- Free tier has rate limits and delayed data
- Ensure `POLYGON_PLAN` in `.env` matches your subscription
- Check if market is open (or set `RUN_EVEN_WHEN_MARKET_IS_CLOSED=true` for testing)

### Getting Help

- Check the logs in the Gradio dashboard for detailed error messages
- Review `accounts.db` with a SQLite browser to inspect data
- Ensure all environment variables are properly set

---

## Key Components
- `app.py` — Gradio dashboard UI
- `trading_floor.py` — Agent scheduler and orchestrator
- `traders.py` — Trader agent logic
- `accounts.py` — Account and transaction models
- `database.py` — SQLite persistence
- `market.py` — Market data access
- `templates.py` — Prompt/instruction templates
- `accounts_client.py` — MCP client for account tools
- `mcp_params.py` — MCP server configuration
- `util.py` — UI helpers

---

## Contributing
Contributions are welcome! Please open issues or submit pull requests for improvements, bug fixes, or new features.

---

## License
MIT License

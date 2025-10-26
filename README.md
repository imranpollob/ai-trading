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

1. **Clone the repository:**
   ```sh
   git clone <your-repo-url>
   cd trading
   ```
2. **Install dependencies:**
   ```sh
   pip install -r requirements.txt
   ```
3. **Configure environment variables:**
   - Copy `.env.example` to `.env` and fill in your API keys and configuration.

---

## Configuration

All configuration is managed via environment variables. See `.env.example` for required and optional settings:
- API keys for Polygon.io, Pushover, DeepSeek, Google, Grok, OpenRouter, Brave
- Trading simulation parameters (intervals, model selection, etc.)

---

## Usage

**Start the agent scheduler:**
```sh
python trading_floor.py
```

**Launch the Gradio dashboard:**
```sh
python app.py
```

Open the provided URL in your browser to monitor the simulation.

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

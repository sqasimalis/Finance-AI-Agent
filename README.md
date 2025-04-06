
# AI Agent Collection

This repository contains a collection of AI agents built using the **Phi framework** that integrate various tools for performing different tasks. These agents leverage models such as **Groq** and **OpenAI** to generate responses, retrieve financial data, and even fetch information from the web.

## Table of Contents

- Overview
- Installation
- Agents
  - Simple Groq Agent
  - Finance Agent
  - Agent Team
- Environment Variables
- Usage

## Overview

The repository contains several agents with distinct functionalities, including natural language generation, financial data retrieval, and web scraping. These agents are built using the Phi framework, which simplifies the creation and interaction with AI models.

### Dependencies

- `phi`: The Phi framework for creating AI agents.
- `groq`: The Groq model used for processing tasks.
- `dotenv`: For loading environment variables.
- `yfinance`: For retrieving financial data.
- `duckduckgo`: For web scraping and fetching data.
  
## Installation

To get started, clone the repository and install the required dependencies:

```bash
git clone https://github.com/sqasimalis/Finance-AI-Agent.git
cd Finance-AI-Agent
```

You may need to install the following Python libraries manually:

```bash
pip install phi groq dotenv yfinance duckduckgo
```

## Agents

### 1. Simple Groq Agent

**File:** `1_simple_groq_agent.py`

This is a basic agent powered by the Groq model. It can generate text based on a prompt provided to it.

#### Example Usage:

```python
from phi.agent import Agent
from phi.model.groq import Groq
from dotenv import load_dotenv

load_dotenv()

agent = Agent(
    model=Groq(id="llama-3.3-70b-versatile")
)

agent.print_response("Write a 2 sentence poem for the love between chocolate and ice cream")
```

#### Description:
This agent demonstrates how to use the Groq model to generate a creative response based on a prompt. In this case, it is asked to generate a short poem.

---

### 2. Finance Agent

**File:** `2_finance_agent.py`

This agent uses financial data tools to fetch stock information and compare financial data for specific companies.

#### Example Usage:

```python
from phi.agent import Agent
from phi.model.groq import Groq
from phi.tools.yfinance import YFinanceTools
from dotenv import load_dotenv

load_dotenv()

def get_company_symbol(company: str) -> str:
    # Returns the symbol for a given company
    symbols = {
        "Phidata": "MSFT",
        "Tesla": "TSLA",
        "Apple": "AAPL",
    }
    return symbols.get(company, "Unknown")

agent = Agent(
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[YFinanceTools(stock_price=True, analyst_recommendations=True, stock_fundamentals=True), get_company_symbol],
    instructions=["Use tables to display data."],
    show_tool_calls=True,
    markdown=True,
    debug_mode=True,
)

agent.print_response("Summarize and compare analyst recommendations and fundamentals for TSLA and Phidata. Show in tables.")
```

#### Description:
This agent pulls stock data, including analyst recommendations, stock price, and financial fundamentals for a given company (such as Tesla or Apple). It then compares the data and displays it in table format.

---

### 3. Agent Team

**File:** `3_agent_teams.py`

This agent is a combination of two specialized agents: one for web scraping and another for financial data. It demonstrates how to create a team of agents to work together.

#### Example Usage:

```python
from phi.agent import Agent
from phi.model.groq import Groq
from phi.tools.duckduckgo import DuckDuckGo
from phi.tools.yfinance import YFinanceTools
from dotenv import load_dotenv

load_dotenv()

web_agent = Agent(
    name="Web Agent",
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[DuckDuckGo()],
    instructions=["Always include sources"],
    show_tool_calls=True,
    markdown=True
)

finance_agent = Agent(
    name="Finance Agent",
    role="Get financial data",
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[YFinanceTools(stock_price=True, analyst_recommendations=True, company_info=True)],
    instructions=["Use tables to display data"],
    show_tool_calls=True,
    markdown=True
)

agent_team = Agent(
    model=Groq(id="llama-3.3-70b-versatile"),
    team=[web_agent, finance_agent],
    instructions=["Always include sources", "Use tables to display data"],
    show_tool_calls=True,
    markdown=True
)

agent_team.print_response("Summarize analyst recommendations and share the latest news for NVDA")
```

#### Description:
This example creates a team of two agents: one for web scraping and one for fetching financial data. The agents collaborate to fetch and summarize information, including the latest news and financial data for a company.

---

## Environment Variables

For the agents to work correctly, you need to set up the environment variables in a `.env` file. Here's an example of the required variables:

```ini
GROQ_API_KEY="your_groq_api_key"
OPENAI_API_KEY="your_openai_api_key"
```

Make sure to replace the placeholder values with your actual API keys.

## Usage

1. Set up your `.env` file with the required API keys.
2. Run the script you want to use (e.g., `python 1_simple_groq_agent.py`).
3. The agent will output its response based on the instructions you have defined.

# Agentic AI Workshop

Repository for daily tasks, progressive agent architectures, and hands-on implementations in Agentic AI.

## 🗂️ Daily Task Tracker

### [DAY 1] Fundamentals & First API Call
- **Notebook:** [`agenticAI_Day1.ipynb`](./agenticAI_Day1.ipynb)
- **Domain:** Sweet Crumbs Home Bakery
- **Concepts & Implementation:**
  - **Secure API Key Handling:** Multi-source credential loading across environment variables, Google Colab Secrets, local `.env` (`python-dotenv`), and hidden `getpass` input.
  - **First API Call to Groq:** Building a bakery assistant with LangChain's `ChatGroq` (`llama-3.3-70b-versatile`, `temperature=0`).
  - **Constrained System Prompting:** Enforcing bakery facts, ordering rules (24-hour notice, eggless options, no guaranteed delivery times), and concise responses (under 60 words).
  - **Safety & Guardrails:** Detecting prompt injection attempts using Meta's Llama Prompt Guard (`meta-llama/llama-prompt-guard-2-22m`) via the Groq SDK.

### [DAY 2] Prompts, Memory, and Building an Agent
- **Notebook:** [`agentic_AI_day_2.ipynb`](./agentic_AI_day_2.ipynb)
- **Domain:** FoodMate Restaurant (Menu Ordering Chatbot)
- **Concepts & Implementation:**
  - **4-Pillar System Prompt:** Calibrating Role (`FoodMate`, a friendly restaurant ordering assistant), Facts (JSON menu with item prices in ₹), Rules (never invent menu items/prices, require confirmation before placing orders, show the complete bill, fallback `"I'll check with the restaurant."`), and Format (friendly English under 80 words).
  - **Python Foundations for Agents:** Menu dictionary (`MENU`), stateful `cart`, type hints, docstrings, and `@log_call` execution logging decorator (`get_price`, `add_to_cart`, `remove_from_cart`, `update_quantity`, `view_cart`, `calculate_bill`).
  - **Tool Calling & Schema Validation:** Building JSON-schema `TOOLS` with `make_tool` (enum-constrained menu items) and dispatching model tool calls via `FUNCTIONS`.
  - **Memory by Hand:** Stateful multi-turn `chat()` loop with sliding-window history trimming (`MAX_TURNS = 8`) to preserve context window limits.
  - **Error Resilience:** Wrapping Groq API (`openai/gpt-oss-120b`) and tool calls in `try` / `except` blocks to gracefully handle `RateLimitError`, `APIConnectionError`, and invalid arguments (`ValueError`, `TypeError`, `KeyError`).
  - **Interactive Ordering Loop:** Multi-turn console chatbot (`You:` / `Bot:`) supporting menu exploration, cart additions/removals/quantity updates, cart viewing, and total bill calculation.

## 🔑 Setup & API Keys

All tasks use the same Groq API Key (`GROQ_API_KEY`).

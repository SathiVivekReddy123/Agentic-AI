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

### [DAY 3] LangChain LCEL Assistant & Automated CSV Testing
- **Notebook:** [`Day3/agentic_AI_day3.ipynb`](./Day3/agentic_AI_day3.ipynb)
- **Test Dataset:** [`Day3/test.csv`](./Day3/test.csv)
- **Domain:** Spice Garden Restaurant (Menu Q&A & Order Assistant)
- **Concepts & Implementation:**
  - **Groq LLM via LangChain:** Configuring `ChatGroq` (`openai/gpt-oss-120b`, `temperature=0.3`) with runtime `getpass` API key setup.
  - **Prompt Engineering & Template Escaping:** Applying the 4-pillar system prompt (Role, Facts with JSON `MENU` in ₹, opening hours `11 AM to 10 PM` all days, delivery rules with ₹200 minimum and ₹30 charge; Rules; Format under 50 words in English or Telugu) and escaping curly braces (`{{` / `}}`) for `ChatPromptTemplate`.
  - **LCEL Chain & Conversation History:** Building a `ChatPromptTemplate` (with `MessagesPlaceholder("history")`) | `llm` | `StrOutputParser()` pipeline with an interactive `ask()` loop (`You:` / `Bot:`).
  - **Automated CSV Evaluation:** Implementing `run_tests()` (`csv.DictReader` and `pandas`) to evaluate 13 benchmark questions from [`Day3/test.csv`](./Day3/test.csv) against `must_include` assertions (achieving 10/13 in the recorded run).

### [DAY 4] Tool-Calling Agent, Pydantic Validation & Google Sheets Persistence
- **Notebook:** [`Day4/agentic_AI_day4.ipynb`](./Day4/agentic_AI_day4.ipynb)
- **Test Dataset:** [`Day4/tests.csv`](./Day4/tests.csv)
- **Domain:** Spice Hub Restaurant (End-to-End Ordering Agent)
- **Concepts & Implementation:**
  - **LangChain Agent & Groq Setup:** Initializing `ChatGroq` (`openai/gpt-oss-20b`, `temperature=0.2`, `max_retries=3`) with `create_agent` and a `chat()` wrapper maintaining message history.
  - **Pydantic Schemas & Structured Output:** Defining `Order` (strict `Literal` menu items, `quantity >= 1`, `dine-in`/`takeaway`) and `OrderExtraction` (optional fields) models, and testing `llm.with_structured_output(OrderExtraction)` on complete, partial, and invalid customer messages.
  - **Custom Tools & Google Sheets Logging:** Implementing `@tool` functions `get_price`, `check_availability` (backed by `MENU` and `AVAILABILITY`), and `log_order` (validating confirmed orders via `Order` and appending rows to a `"Spice Hub Orders"` Google Sheet via `gspread` and Colab auth).
  - **Automated Testing & Execution Tracing:** Running 8 test scenarios from [`Day4/tests.csv`](./Day4/tests.csv) and saving outputs to `test_results.csv`, inspecting agent execution traces (`pretty_print()`, AI message and tool call counts), and running a tool-renaming experiment (`tool1` vs `get_price`).
  - **Knowledge Base Output:** Generating `restaurant_facts.txt` with 20 structured restaurant facts covering hours, pricing, payment methods (cash/UPI), and ordering policies.

### [DAY 5] RAG Knowledge Base, Semantic Search & Conversation Memory
- **Notebook:** [\`Day5/agentic_AI_day5.ipynb\`](./Day5/agentic_AI_day5.ipynb)
- **Domain:** Spice Hub Restaurant (Knowledge-Based Conversational Agent)
- **Concepts & Implementation:**
  - **Retrieval-Augmented Generation (RAG):** Building a restaurant knowledge base using 20 FAQ entries covering menu items, prices, opening hours, payment methods, services, and restaurant policies.
  - **Text Embeddings:** Using Sentence Transformers (\`all-MiniLM-L6-v2\`) to convert restaurant FAQ documents and customer queries into numerical vector embeddings.
  - **Vector Database & Semantic Search:** Implementing FAISS (\`IndexFlatIP\`) for efficient similarity-based retrieval of relevant restaurant information using normalized embeddings.
  - **Custom RAG Tool:** Creating a \`search_faq()\` tool with LangChain's \`@tool\` decorator to retrieve relevant answers from the knowledge base and handle questions when information is unavailable.
  - **Agent Memory & Checkpointing:** Integrating LangGraph's \`InMemorySaver\` checkpointer with \`create_agent\` to maintain conversation history across multiple interactions.
  - **Thread-Based Conversation Management:** Implementing a \`chat(thread_id, text)\` function to maintain separate conversation contexts for different customers using unique thread IDs.
  - **Tool Integration:** Combining the existing Day 4 business tools (\`get_price\`, \`check_availability\`, and \`log_order\`) with the new \`search_faq\` tool to create a knowledge-enabled restaurant assistant.
  - **Testing & Validation:** Testing restaurant FAQs, menu prices, unknown questions, separate customer conversations, and memory retention across multiple interactions.
  - **Interactive Chatbot:** Building a multi-turn conversational interface for restaurant queries and order processing with explicit customer confirmation before saving orders.

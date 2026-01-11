#  Multi-Agent Historical Research System

An intelligent, modular system powered by **LangGraph** and **LangChain** that automates the end-to-end process of historical research—from initial planning and archival discovery to fact-checking and professional PDF report generation.

## 🏗️ Architecture

The system follows a hub-and-spoke supervisor model where a central LLM manages the state and workflow transitions between specialized nodes.

![Workflow Graph](workflow_graph.png)

### Agent Roles & Responsibilities


* **Research Planning Agent**: Extracts entities (Topic, Location, Time Period) and generates a structured research plan consisting of targeted questions and search keywords.


* **Supervisor Agent**: The "brain" of the operation. It evaluates the project state and determines which expert agent to call next based on a strictly defined sequence.


* **Researcher Agent**: A ReAct agent equipped with external tools (Google Search, Wikipedia, and DPLA) to gather primary and secondary source materials.


* **Checker Agent**: A logic-heavy agent that verifies factual accuracy via JSON-based scoring and performs conservative rewrites to ensure reliability.


* **Report Generator Agent**: Handles the document engineering, transforming verified text into a styled PDF report using the `ReportLab` library.


---

##  Execution & Workflow

Understanding how the agents interact is crucial for navigating the codebase. Here is the step-by-step execution flow:

### 1. Initialization

When you run `streamlit run app.py`, the `AgentState` is initialized. This state object acts as the "shared memory" for the entire graph, holding the original query, the research plan, and the findings as they are generated.

### 2. The Supervisor's Decision Loop

The workflow is non-linear and managed by the **Supervisor Node**. After every agent completes its task, control returns to the Supervisor to decide the next step:

* **Step A (Planning)**: If no plan exists, the Supervisor calls the `Planner`.


* **Step B (Researching)**: Once a plan is in state, the Supervisor triggers the `Researcher` to use the search tools.


* **Step C (Verifying)**: After findings are compiled, the `Checker` validates the data against the original query.


* **Step D (Finalizing)**: Finally, the `Reporter` converts the verified text into a PDF and the Supervisor concludes the process with `FINISH`.



### 3. Output

The final result is a professional **Historical Research Report** saved locally as a `.pdf` file and displayed within the Streamlit interface.

---

##  Getting Started

### 1. Prerequisites

* Python 3.10+
* A Groq account for high-speed LLM inference.
* API keys for historical and web data sourcing.

### 2. Clone the Repository

```bash
git clone https://github.com/Sumanthdev06/History_Report_Generator.git
cd History_Report_Generator

```

### 3. Set Up API Keys

You will need three separate API keys. Create a `.env` file in the root directory and add them as follows:

```env
GROQ_API_KEY=your_groq_key_here
SERP_API_KEY=your_serpapi_key_here
DPLA_API_KEY=your_dpla_key_here

```

| Key | Purpose | How to Get |
| --- | --- | --- |
| **GROQ_API_KEY** | Runs the Llama-3 models | [GroqCloud Console](https://console.groq.com/keys) |
| **SERP_API_KEY** | Enables Google Search capabilities | [SerpApi Dashboard](https://serpapi.com/) |
| **DPLA_API_KEY** | Access to primary archival sources | Run the `curl` command below |


**To get the DPLA Key:**

```bash
curl -v --ssl-no-revoke -XPOST https://api.dp.la/v2/api_key/YOUR_EMAIL@gmail.com

```

### 4. Installation & Execution

Install the required dependencies and launch the Streamlit interface:

```bash
# Install requirements
pip install -r requirements.txt

# Run the application
streamlit run app.py

```

---

## 🛠️ Built With

* **[LangGraph](https://github.com/langchain-ai/langgraph)**: For orchestrating the multi-agent state machine.
* **[LangChain](https://github.com/langchain-ai/langchain)**: For tool integration and LLM chaining.
* **[Groq](https://groq.com/)**: Utilizing `Llama-3` models for rapid response times.
* **[ReportLab](https://www.reportlab.com/)**: For programmatic PDF generation.
* **[Streamlit](https://streamlit.io/)**: For the user interface.

---

## Author

**Sumanth**

* Master’s in Computer Science, Graduate Research Assistant @ UNC Charlotte (Expected May 2026) 

---

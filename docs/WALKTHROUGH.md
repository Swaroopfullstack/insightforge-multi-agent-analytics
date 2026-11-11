# InsightForge walkthrough

## What to demonstrate

A typed LangGraph supervisor routes each question to Data, RAG, ML, Tools or Memory. Streamlit sends authenticated requests to FastAPI. Data never enters an unrestricted Python REPL. The calculator interprets a small arithmetic AST.

### Walk through it

1. Start the shared API and this Streamlit app.
2. Upload the attributed Iris CSV. Ask for a summary, missing values, correlation, and a chart (for example “chart petal_length” in demo mode).
3. Select **Train and compare models**. Inspect five classification algorithms and a dummy baseline. The winner is selected using training-only CV; the test partition evaluates only that winner.
4. Upload diabetes and repeat: Linear, Ridge, Random Forest, Gradient Boosting, KNN and Decision Tree are compared.
5. Override target/problem type in the ML controls when automatic inference is inappropriate.
6. Upload the NIST PDF, ask a document question, and inspect page/chunk citations.
7. Use “remember language=English”, then “recall my preferences”. A new session under the same owner retains long-term preferences; a new user does not inherit private history.
8. Create a second session and switch between them. Display results are cleared on session change.

### Read the code in this order

[API](../src/portfolio/api.py) → [runtime](../src/portfolio/runtime.py) → [supervisor graph](../src/portfolio/insight.py) → [data](../src/portfolio/data.py) / [ML worker](../src/portfolio/ml_worker.py) / [retrieval](../src/portfolio/retrieval.py) → [storage](../src/portfolio/storage.py) → [UI](../src/portfolio/ui.py).

### Explain the tradeoffs

A target guessed by column name is a heuristic. IDs/free text are bounded/dropped, but semantic leakage, duplicate/entity groups and temporal splits need domain knowledge. Reports are downloadable JSON experiments, not deployed model-serving artifacts. The held-out educational metrics in the evaluation report are not predictions of future business performance.

### Acceptance evidence

The tests run real ML in a terminated-on-timeout subprocess, enforce cross-user access denial, persist messages/preferences, retrieve from Chroma after reopening, and validate API input. Live mode requires your provider credential. Offline routing is a declared baseline, not evidence that an LLM reasoned correctly.

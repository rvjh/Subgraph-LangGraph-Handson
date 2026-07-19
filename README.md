# LangGraph Subgraph Example: Parent Graph with Subgraph

This project demonstrates how to build **AI workflows** using **LangGraph**. The workflow consists of:

- A **Parent Graph** that answers a user's question in English.
- A **Subgraph** that translates the generated answer into Bengali.
- The parent graph invokes the subgraph as a reusable component.

---

## Project Architecture

```text
                    Parent Graph

                  User Question
                        │
                        ▼
              ┌──────────────────┐
              │ Generate Answer  │
              │ (English)        │
              └────────┬─────────┘
                       │
                       ▼
             Calls Translation Subgraph
                       │
                       ▼
        ┌─────────────────────────────┐
        │ Translate English → Hindi   │
        └──────────────┬──────────────┘
                       │
                       ▼
                Final Output
```

---

# Features

- Modular workflow using LangGraph
- Reusable subgraphs
- Groq opensource Model
- Automatic English → Bengali translation
- Typed state management using `TypedDict`

---

# Project Structure

```text
project/
│
├── subgraph.ipynb
├── .env
├── requirements.txt
└── README.md
```

---

# Installation

Clone the repository.

```bash
git clone <repository-url>

cd project
```

Install dependencies.

```bash
pip install -r requirements.txt
```

---

# Environment Variables

Create a `.env` file.

```env
GOOGLE_API_KEY=your_google_api_key
GROQ_API_KEY=your_google_api_key
```

---

# Dependencies

```text
langchain
langchain-google
pydantic
python-dotenv
langgraph
wikipedia
langchain-community
langchain_google_genai
langchain_groq
ipykernel
```

Install them using

```bash
pip install -r requirements.txt
```

---

# Workflow

## Step 1 — User asks a question

Example:

```text
What is quantum physics?
```

---

## Step 2 — Parent Graph

The parent graph generates an answer using Gemini.

Node:

```python
generate_answer()
```

State:

```python
ParentState

question
answer_eng
answer_hin
```

---

## Step 3 — Translation Subgraph

Instead of translating directly inside the parent graph, the parent invokes a dedicated subgraph.

Node:

```python
translate_text()
```

State:

```python
SubState

input_text
translated_text
```

Prompt used:

```text
Translate the following text to Bengali.
Keep it natural and clear.
Do not add extra content.
```

---

## Step 4 — Final Output

The graph returns

```python
{
    "question": "...",
    "answer_eng": "...",
    "answer_hin": "..."
}
```

---

# Graph Design

## Parent Graph

```text
START
   │
   ▼
Generate Answer
   │
   ▼
Translate Answer
   │
   ▼
END
```

---

## Translation Subgraph

```text
START
   │
   ▼
Translate Text
   │
   ▼
END
```

---

# Running the Graph

Compile the graphs.

```python
subgraph = subgraph_builder.compile()

graph = parent_builder.compile()
```

Invoke the parent graph.

```python
result = graph.invoke({
    "question": "What is quantum physics?"
})
```

---

# Example Output

```python
{
    "question": "What is quantum physics?",

    "answer_eng":
        "Quantum physics is the branch of physics ...",

    "answer_hin":
        'কোয়ান্টাম পদার্থবিজ্ঞান হলো একটি পদার্থবিজ্ঞানের শাখা যা পদার্থ এবং শক্তির আণবিক ও সূক্ষ্ম স্তরের আচরণ নিয়ে কাজ করে।'
}
```

---

# Why Use a Subgraph?

Using a subgraph makes the workflow:

- Modular
- Reusable
- Easier to maintain
- Easier to test independently
- Scalable for larger AI pipelines

Instead of embedding translation logic inside the parent graph, the translation workflow is isolated into its own graph that can be reused by other applications.

---
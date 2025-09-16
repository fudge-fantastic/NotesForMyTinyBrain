### 1. [**Tool**](https://js.langchain.com/docs/concepts/tools/)

* A **tool** is just a wrapper around some function (like your retriever).
* Why? Because instead of you deciding *“always run retriever”*, you give the LLM the option:

  * *“If you need code snippets, call this retriever tool.”*
  * *“If it’s chit-chat, just answer directly.”*
* Makes your app more **dynamic and smart**.
* ✅ Beneficial for multi-turn and when you want more than one capability (e.g., calculator, repo search, API calls).

---

### 2. **Chat History (messages state)**

* Keeps a running list of all user + assistant messages.
* Example:

  * User: *“How does login work?”*
  * Assistant: *“…explains login…”*
  * User: *“What about token expiry?”*
* Without history → assistant only sees “What about token expiry?” (confusing).
* With history → assistant sees both Qs → gives better answer.
* ✅ Essential for natural conversations with your codebase.

---

### 3. **StateGraph**

* Think of it like a **flowchart of steps**.
* Each step is a **node** (`retrieve`, `generate`, maybe `summarise`, etc).
* Edges say how the flow moves.
* You can branch conditionally:

  * Small talk → go straight to `generate`.
  * Tech Q → run `retrieve` first.
* ✅ Gives you control over multi-step logic.

---

### 4. **Checkpointer**

* Saves the state (history, context, etc) between runs.
* Without it: history dies when the process restarts.
* With it: history persists across sessions (like ChatGPT does).
* ✅ Important if you want "continuous conversation" beyond a single request.

---

## 🔄 Do Multi-step / Tools Call the LLM Again and Again?

Yes — **each step/tool may involve another LLM call**.
Here’s the flow:

1. User asks Q → goes into graph.
2. Graph decides → run retriever tool → this is **NOT** an LLM call, just a vector DB query.
3. Pass retrieved docs + history to LLM → **LLM call #1**.
4. If you had another step (e.g., summarizer, translator) → could be **LLM call #2**, etc.

So the number of LLM calls = number of “generate” (reasoning/answering) steps.
Tools like retrievers, databases, or calculators usually don’t cost tokens — only LLM calls do.

---

✅ In short:

* Tools = optional helper functions the LLM can use.
* History = memory of conversation.
* Graph = flow control between steps.
* Checkpointer = persistence.
* Yes, multi-step can mean multiple LLM calls — but only where reasoning is needed.

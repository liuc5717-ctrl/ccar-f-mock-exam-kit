# CCAR-F Sample Diagnostic Scenarios (5 Preview Questions)

Below are 5 representative scenario questions demonstrating the structural depth of the full 60-question Benchmark Manual.

---

### Question 1: Agentic Orchestration & Tool Looping
**Domain**: Domain 1 - Agentic Architecture

**Scenario**: An enterprise data analytics agent powered by Claude 3.5 Sonnet needs to execute a multi-step financial reconciliation workflow. During execution, the tool invocation fails with a transient SQL timeout error. What is the most resilient architectural strategy to handle this failure within an agentic loop?

- **A)** Terminate the current agent context immediately and raise a fatal user error.
- **B)** Return an error tool result payload back into the model's message history and allow Claude to formulate an automated retry or fallback strategy.
- **C)** Hardcode a fixed 3-time retry wrapper entirely inside the orchestrator without sending error context to Claude.
- **D)** Increase the SQL client timeout threshold to 300 seconds and rerun the exact same prompt from scratch.

**Correct Answer**: **B**

**Architectural Rationale**:
Returning the structured error message as a `tool_result` content block inside the conversational history allows the reasoning engine to observe the failure reason, analyze constraints, and autonomously decide whether to switch to an alternative retrieval tool or execute a backoff strategy.

---

### Question 2: Prompt Caching Strategy
**Domain**: Domain 2 - Context Management

**Scenario**: A legal research assistant processes 80-page contract documents against varying compliance checklists. To minimize token costs and end-to-end TTFT (Time to First Token), where should the `cache_control` breakpoint be positioned?

- **A)** On the final user prompt containing dynamic compliance criteria.
- **B)** At the end of the system prompt and the static reference document payload in the message chain.
- **C)** After each individual paragraph within the dynamic user question.
- **D)** Prompt caching cannot be used with long context windows exceeding 50,000 tokens.

**Correct Answer**: **B**

**Architectural Rationale**:
Prompt caching provides up to 90% cost reduction and significantly lower latency when applied to long, static prefix blocks. Setting `cache_control: {"type": "ephemeral"}` at the end of invariant system instructions and document context ensures subsequent dynamic queries reuse the cached prompt prefix.

---

### Question 3: Model Context Protocol (MCP) Boundaries
**Domain**: Domain 3 - Tool Integration

**Scenario**: An organization is deploying Claude Code connected to an MCP server exposing internal Jira and GitHub repositories. Which design pattern best adheres to the principle of least privilege while preventing prompt injection via tool payloads?

- **A)** Run the MCP server with root host privileges to avoid permission denied errors.
- **B)** Configure explicit schema validations, read-only token scopes, and human-in-the-loop approvals for state-mutating tools.
- **C)** Disclose internal credentials directly inside the MCP server tool descriptions.
- **D)** Pass untrusted third-party pull request text directly into Claude's system prompt without tagging.

**Correct Answer**: **B**

**Architectural Rationale**:
Secure MCP architectures enforce strict JSON schema sanitization, least-privilege API authentication tokens, and human confirmation barriers before executing destructive actions (e.g., repository deletions or ticket status writes).

---

### Question 4: Token Economics & Latency Optimization
**Domain**: Domain 4 - Production Reliability

**Scenario**: A high-throughput customer support routing agent requires sub-second latency classification for incoming support tickets. Which Claude model selection and configuration provides optimal performance?

- **A)** Claude 3.5 Sonnet with 8,000 max reasoning tokens enabled.
- **B)** Claude 3.5 Haiku with concise system instructions and structured JSON output schema.
- **C)** Claude 3 Opus with extensive multi-shot examples.
- **D)** Claude 3.5 Sonnet processing tickets sequentially in batches of 100.

**Correct Answer**: **B**

**Architectural Rationale**:
Claude 3.5 Haiku delivers near-instant TTFT and high throughput at a fraction of the cost, making it the industry standard model choice for classification, fast triage, and strict schema routing tasks.

---

### Question 5: Guardrails & Output Determinism
**Domain**: Domain 2 - Prompt Engineering

**Scenario**: An automated agent must reliably extract healthcare diagnosis codes into a strict JSON format without introductory conversational chatter or Markdown code fences. Which approach ensures the highest determinism?

- **A)** Add 'PLEASE DO NOT CHAT' in uppercase to the prompt.
- **B)** Leverage tool use (function calling) by forcing a specific tool definition schema via `tool_choice: {"type": "tool", "name": "record_diagnosis"}`.
- **C)** Set temperature to 1.0 and rely on regex post-processing.
- **D)** Ask the model to generate XML tags and manually parse broken tags.

**Correct Answer**: **B**

**Architectural Rationale**:
Forcing a tool call via `tool_choice` guarantees that the model bypasses conversational output generation and responds strictly within the parameters specified by the JSON Schema definition.

---

### 🚀 Want the full 60-scenario benchmark?
Get instant access to the complete 60-Question Interactive Simulator, 61-Page Manual, and CSV Dataset:  
👉 **[Unlock Full Mock Exam Kit ($29 USD)](https://dodo.pe/ccar-f-kit)**

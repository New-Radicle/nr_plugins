# Glossary

Plain-language definitions. Where an industry term exists, it is in parentheses. Grouped by layer.

## Layer 1: the model

| Term | Meaning |
|---|---|
| Model | The trained system that produces answers. Different sizes trade quality for speed and cost. |
| Prompt | Everything you send in one turn: the question plus any instructions and material. |
| Token | The unit text is measured and priced in; roughly three-quarters of a word. |
| Context window | The working space that holds the whole conversation and attached files. Finite. When full, quality drops. |
| Training cutoff | The date after which the model knows nothing unless told. |
| Sampling (temperature) | The randomness in generation. Why the same prompt gives different answers. |
| Reasoning | Extra thinking steps before answering. Better on hard problems, slower, costs more. |

## Layer 2: what you feed it

| Term | Meaning |
|---|---|
| Instructions (system prompt) | Standing guidance about role, tone, format, and limits, given before the task. |
| Context | The material the model sees for this task: documents, examples, data. |
| Examples (few-shot) | Two or three samples of what good looks like. The fastest way to get your style. |
| Template | A fixed structure the answer must follow. |
| Retrieval | Pulling the relevant pieces from a large store into the prompt instead of pasting everything. |
| Iteration | Correcting and re-asking. Normal, not failure. |

## Layer 3: what it can reach

| Term | Meaning |
|---|---|
| Tool (function call) | An action the model can request: read, search, calculate, write. |
| Standard socket (MCP server, connector) | A system exposed through a common protocol so any model can use it the same way. |
| Authorised link (OAuth grant) | Your sign-in that lets a socket act as you. Scoped; revocable. |
| Taught procedure (skill) | Written instructions loaded when a matching task appears. |
| Packaged bundle (plugin) | Procedures, connections, and agents shipped together. |
| Marketplace | A catalogue of bundles a team can install from. |

## Layer 4: how it runs

| Term | Meaning |
|---|---|
| Agent | A model with tools, a goal, and permission to take several steps. |
| Permission mode | The rule for what runs without asking: read-only, ask on writes, or fully autonomous. |
| Human in the loop | A person checks before an action takes effect. |
| Approval gate | A required pause before anything irreversible or outward-facing. |
| Turn limit, timeout | Hard stops on how long a run may go. |
| Scheduled task (routine) | A run started by a timer rather than a person. |
| Idempotent | Safe to run twice; the second run changes nothing extra. |

## Layer 5: whether you can trust it

| Term | Meaning |
|---|---|
| Confident wrongness (hallucination) | A fluent, plausible, incorrect answer. |
| Verification | Checking output against a primary source. |
| Audit trail | Prompt, inputs, and output kept together with who reviewed it. |
| Attribution line | "AI-assisted draft. Reviewed by:" plus a name and date. |
| Hidden instructions (prompt injection) | Text in something the model reads that tries to command it. |
| Data handling | Whether inputs are stored, for how long, where, and whether they train the model. |
| Never-in-prompts list | The company's list of what must not be typed or pasted into an AI tool. |

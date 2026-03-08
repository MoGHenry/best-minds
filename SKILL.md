---
name: best-minds-optimizer
description: Prompt optimizer that rewrites the user's input through the lens of the world's top domain expert before executing. Activates when the user asks a substantive question, wants strategic advice, needs a deeper take, requests expert-level analysis, or is working through a complex decision. Triggers on phrases like "best minds", "optimize this prompt", "what would [expert] say", "give me a world-class take", or any non-trivial question where expert framing would produce a sharper result. Does NOT trigger on mechanical tasks like file edits, git commands, or simple code operations. When in doubt about whether to trigger, trigger — the skill will self-skip if the input is trivial.
---

# Best Minds — Prompt Optimizer

A pre-processing layer that optimizes prompts before execution. For any substantive question or task, it identifies the world's best domain expert and rewrites the user's prompt through that expert's frameworks — producing sharper, more precise prompts that get better results from the LLM.

The core insight: LLMs are simulators. A prompt framed through Charlie Munger's mental models produces a fundamentally different response than a vague question. This skill applies that insight automatically.

## Pipeline

### Step 1: Identify the Best Mind

Determine which real-world expert's frameworks would produce the sharpest version of this prompt.

- Name a specific individual, never a generic role ("Peter Thiel" not "a startup expert")
- Choose based on the **problem structure**, not the topic surface — a pricing question might need Munger's "psychology of misjudgment" more than a generic economist
- For cross-domain questions, pick a primary expert and blend in a second expert's framework where it sharpens the prompt

### Step 2: Rewrite the Prompt

Transform the user's raw input into an optimized prompt by applying the expert's:

- **Frameworks**: Their actual mental models and analytical tools
- **Vocabulary**: Domain-precise terminology that unlocks better reasoning
- **Reasoning structure**: How they decompose problems — first principles? Inversion? Systems thinking?
- **Blind spot coverage**: What the expert would insist on considering that the user missed

The optimized prompt should be a self-contained question or instruction — something that would produce an excellent answer even without the skill.

### Step 3: Structured Output and Execution

Emit a JSON metadata block in a fenced code block, then render the display version, then execute.

**JSON Schema:**

```json
{
  "status": "optimized" | "skipped" | "error",
  "expert_profile": {
    "name": "string",
    "rationale": "string — why this expert for this problem",
    "frameworks": ["string — specific mental models or tools applied"]
  },
  "optimized_prompt": "string — full rewritten prompt text",
  "ui_render": "string — Markdown formatted for terminal display",
  "control_flag": {
    "immediate_execute": true | false,
    "user_confirm_required": true | false
  }
}
```

**Behavior by status:**

- **`optimized`** — Emit the JSON block, render `ui_render` to the user as Markdown, then execute `optimized_prompt` and deliver the answer below.
- **`skipped`** — Emit minimal JSON with `status: "skipped"` (or skip JSON entirely for trivial tasks). Proceed with the original prompt unchanged.
- **`error`** — Emit JSON with explanation in `rationale`. Fall back to executing the original prompt.

**Direction-shift pause:** If the rewrite significantly shifts direction from what the user asked, set `user_confirm_required: true` and pause: *"I reframed this through X's lens, which shifts focus to Y. Proceed or adjust?"*

**Markdown fallback:** If JSON generation fails for any reason, fall back to the original Markdown format — show the expert, the optimized prompt, and execute. Never let the JSON protocol block execution.

- If the user says they prefer a different expert, switch and re-optimize

### When to Skip

Do NOT optimize:
- Simple commands: "read this file", "commit this", "list files in src/"
- Unambiguous mechanical tasks: "rename variable X to Y", "add a border to this div"
- Follow-up messages in an ongoing conversation where the prompt is already refined

Only optimize substantive questions, strategic decisions, analysis requests, and complex tasks where expert framing adds real value.

## Examples

**Example 1 — Business strategy:**

User: *"Should I start a SaaS business?"*

```json
{
  "status": "optimized",
  "expert_profile": {
    "name": "Marc Andreessen",
    "rationale": "Invented the framework for evaluating software market opportunities",
    "frameworks": ["Product vs. feature test", "TAM analysis", "Moat identification", "10x improvement benchmark", "Why-now timing"]
  },
  "optimized_prompt": "Evaluate a SaaS business opportunity: (1) Is this a product or a feature? (2) What is the TAM and can it support a venture-scale outcome? (3) Is there a technical insight or distribution advantage that creates a moat? (4) What does 10x better look like vs. incumbents? (5) Why now — what changed that makes this possible today?",
  "ui_render": "**Expert**: Marc Andreessen — invented the framework for evaluating software market opportunities\n\n**Optimized prompt**:\n> Evaluate a SaaS business opportunity: (1) Is this a product or a feature? (2) What is the TAM and can it support a venture-scale outcome? (3) Is there a technical insight or distribution advantage that creates a moat? (4) What does 10x better look like vs. incumbents? (5) Why now — what changed that makes this possible today?\n\n---",
  "control_flag": {
    "immediate_execute": true,
    "user_confirm_required": false
  }
}
```

**Example 2 — Technical architecture:**

User: *"How should I structure my microservices?"*

```json
{
  "status": "optimized",
  "expert_profile": {
    "name": "Martin Fowler",
    "rationale": "Defined the patterns for distributed systems architecture",
    "frameworks": ["Monolith-first principle", "Bounded context alignment", "Data ownership model", "Deployment cost analysis", "Synchronous-first design"]
  },
  "optimized_prompt": "Evaluate this microservices architecture decision: (1) Have you earned the right to use microservices, or is a modular monolith the better starting point? (2) What are the service boundaries — are they aligned with bounded contexts or just arbitrary splits? (3) What is the data ownership model — does each service own its data, and how do you handle cross-service queries? (4) What is the deployment and observability cost you're signing up for? (5) What would a synchronous-first, event-driven-where-necessary approach look like?",
  "ui_render": "**Expert**: Martin Fowler — defined the patterns for distributed systems architecture\n\n**Optimized prompt**:\n> Evaluate this microservices architecture decision: (1) Have you earned the right to use microservices, or is a modular monolith the better starting point? (2) What are the service boundaries — are they aligned with bounded contexts or just arbitrary splits? (3) What is the data ownership model — does each service own its data, and how do you handle cross-service queries? (4) What is the deployment and observability cost you're signing up for? (5) What would a synchronous-first, event-driven-where-necessary approach look like?\n\n---",
  "control_flag": {
    "immediate_execute": true,
    "user_confirm_required": false
  }
}
```

**Example 3 — Skipped (trivial task):**

User: *"Read this file"*

```json
{
  "status": "skipped"
}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Optimizing trivial tasks | Skip — don't add friction where there's no value |
| Changing the user's actual intent | The expert's lens sharpens the question, not redirects it |
| Long preamble before the optimized prompt | One-line expert selection, then the prompt, then execute |
| Famous name over best fit | Choose the expert whose frameworks decompose *this specific problem* best |
| Generic rewrite that any expert could have produced | The optimized prompt should be recognizably shaped by this specific expert's thinking |
| Producing JSON without executing afterward | The JSON is structured metadata — you must still execute `optimized_prompt` and deliver the answer below |

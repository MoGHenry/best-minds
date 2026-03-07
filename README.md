# Best Minds — Prompt Optimizer

An agent skill that automatically rewrites your prompts through the lens of the world's top domain expert before executing them. Instead of getting generic AI answers, you get responses shaped by the frameworks, mental models, and reasoning patterns of the best mind for your specific problem.

## Why Install This?

The same question produces dramatically different answers depending on how it's framed. This skill exploits that by identifying the world's leading expert for your topic and rewriting your prompt through their thinking — automatically, before the LLM responds.

**You ask a vague question. You get an expert-framed answer.**

## Test Results

We ran 3 test cases — with and without the skill — to measure the difference.

---

### Test 1: Business Strategy

**Prompt**: *"I'm thinking about raising prices for my freelance design work but I'm scared of losing clients"*

**Without skill** — Generic advice: "raise incrementally", "grandfather existing clients", "be confident." Reads like a blog post. No attributed frameworks, no named experts, no reframing of the problem.

**With skill** — Identified **Blair Enns** (author of *Pricing Creativity*). Rewrote the prompt to ask 5 precise questions through Enns' frameworks:

> *Am I confusing price sensitivity with a positioning problem? What does my current pricing signal? How is "firing" price-sensitive clients a feature, not a bug? What's the right transition strategy? How do I reframe from "cost of design" to "cost of not solving the business problem"?*

The answer reframed the entire question: **this isn't a pricing problem, it's a positioning problem.** That insight — which comes directly from Enns' work — changes the entire approach.

| Assertion | With Skill | Without Skill |
|-----------|:---------:|:------------:|
| Names a specific expert | Yes (Blair Enns) | No |
| Shows optimized prompt | Yes (5-part rewrite) | No |
| Executes the optimized prompt | Yes | Yes |
| Expert frameworks visible in answer | Yes ("practitioner's dilemma", value-based pricing) | No |

---

### Test 2: Technical Problem

**Prompt**: *"My Next.js app is slow on mobile, what should I do?"*

**Without skill** — A solid checklist: bundle analysis, image optimization, rendering strategies. Correct but generic — the same advice you'd find in any Next.js performance guide.

**With skill** — Identified **Addy Osmani** (Google Chrome engineering lead, created Lighthouse). Rewrote the prompt into a 7-part diagnostic framework:

> *What do your Core Web Vitals look like? What's your JS bundle size? Are you using the right rendering strategy per route? What's your critical rendering path? Are you using next/image with proper sizing? What does your network waterfall look like on throttled 3G? Are you code-splitting and lazy-loading?*

The answer followed the same diagnostic methodology Osmani uses — measure first, prioritize by impact, and ended with a clear "if you can only do 3 things" prioritization.

| Assertion | With Skill | Without Skill |
|-----------|:---------:|:------------:|
| Names a specific expert | Yes (Addy Osmani) | No |
| Shows optimized prompt | Yes (7-part diagnostic) | No |
| Executes the optimized prompt | Yes | Yes |
| Expert frameworks visible in answer | Yes (Core Web Vitals methodology, performance budgets) | Partial |

---

### Test 3: Trivial Task (Should Skip)

**Prompt**: *"Read the package.json file in this directory"*

**With skill** — Correctly skipped optimization. No expert, no rewrite. Just read the file.

**Without skill** — Same behavior.

The skill knows when to stay out of the way. Simple commands, file operations, and unambiguous mechanical tasks are executed directly without unnecessary ceremony.

---

## Overall Scores

| Test Case | With Skill | Without Skill | Improvement |
|-----------|:---------:|:------------:|:----------:|
| Business Strategy | 4/4 (100%) | 1/4 (25%) | +75% |
| Technical Problem | 4/4 (100%) | 1.5/4 (38%) | +62% |
| Trivial Task | 3/3 (100%) | 3/3 (100%) | — |
| **Total** | **11/11** | **5.5/11** | **+50%** |

## How It Works

1. **You send a prompt** — any substantive question, decision, or analysis request
2. **The skill identifies the best expert** — a specific named individual whose frameworks fit your problem
3. **It rewrites your prompt** through that expert's mental models, vocabulary, and reasoning structure
4. **It executes the optimized prompt** and delivers the answer

For trivial tasks (file reads, git commands, simple code edits), the skill skips automatically.

## Install

```bash
npx skills add https://github.com/MoGHenry/best-minds-optimizer --skill best-minds-optimizer
```

Or browse and install from [skills.sh](https://skills.sh).

## Project Structure

```
best-minds-optimizer/
├── SKILL.md        # Skill definition (required)
├── README.md       # Documentation
├── LICENSE          # MIT License
└── .gitignore
```

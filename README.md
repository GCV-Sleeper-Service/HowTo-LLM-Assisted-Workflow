# HowTo-LLM-Assisted-Workflow

A practical guide to using LLMs (Large Language Models) productively in software development, based on real-world experience.

---

## Purpose

This repository captures the documentation, prompt templates, and workflow artifacts from a real embedded software project developed with LLM coding agents (GitHub Copilot, Claude, Codex). The goal is to share hard-won lessons so that other developers can use LLMs more effectively from day one.

---

## Who This Is For

- Developers using AI coding agents (GitHub Copilot, Claude, Cursor, Codex, etc.)
- Teams adopting LLM-assisted development workflows
- Anyone who wants to write better prompts and get better results from coding agents

---

## Repository Structure

### Primary Guide

| File | Description |
|------|-------------|
| [`Docs/writing-prompts-for-coding-agents-guide.md`](Docs/writing-prompts-for-coding-agents-guide.md) | **Start here.** The master guide on writing effective prompts for coding agents. Covers anti-patterns, pre-flight checklists, scope discipline, critical rules, and lessons learned from 100+ real agent sessions. |

### Reference Examples — Real Project Documents

These files are taken directly from the real project. They demonstrate the documentation practices and workflow artifacts described in the guide.

| File | Description |
|------|-------------|
| [`Docs/v7.5-v7.6-architecture-plan.md`](Docs/v7.5-v7.6-architecture-plan.md) | An overall multi-phase architecture plan. Shows how to structure long-horizon planning documents that coding agents can reference. |
| [`Docs/phase-d-implementation-plan.md`](Docs/phase-d-implementation-plan.md) | A detailed implementation plan for one phase (Phase D — Runtime Satellite Management). Demonstrates step-by-step scoping, API contracts, acceptance criteria, and risk summaries per step. |
| [`Docs/changelog.md`](Docs/changelog.md) | A documented changelog tracking every iteration. Shows how to maintain a rich change history that provides context to agents across sessions. |
| [`Docs/bugs-and-lessons-learned.md`](Docs/bugs-and-lessons-learned.md) | Knowledge base of bugs and lessons accumulated during development. This is the key document for avoiding repeated mistakes across agent sessions. |
| [`Docs/session-log-2026-04-04-v7.6.0.5.md`](Docs/session-log-2026-04-04-v7.6.0.5.md) | An example session log capturing what a coding agent did during a single implementation step. Shows how to document agent work for accountability and knowledge transfer. |

### Prompt Templates — Real Working Prompts

| File | Description |
|------|-------------|
| [`prompts/prompt-index-and-workflow.md`](prompts/prompt-index-and-workflow.md) | Coding Agent Prompt Index and Workflow. The single source of truth for all implementation prompts — shows how to organize prompts as a project grows. |
| [`prompts/handoff/session-handoff-v7.6.0.4.md`](prompts/handoff/session-handoff-v7.6.0.4.md) | An example session handoff document — how to transfer context from one agent session to the next without losing state. |
| [`prompts/phaseD/v7.6.0.3-implementation-instructions-for-coding-agent.md`](prompts/phaseD/v7.6.0.3-implementation-instructions-for-coding-agent.md) | Full self-contained implementation instructions for a coding agent (Phase D Step 3). Shows the complete structure of a production-ready agent prompt. |
| [`prompts/phaseD/v7.6.0.3-PR119-consolidated-audit-and-lessons.md`](prompts/phaseD/v7.6.0.3-PR119-consolidated-audit-and-lessons.md) | Post-implementation consolidated audit and lessons document. Created after each implemented step to build the knowledge base and prevent future regressions. |

---

## How to Use This Repository

### Step 1 — Read the Master Guide

Start with [`Docs/writing-prompts-for-coding-agents-guide.md`](Docs/writing-prompts-for-coding-agents-guide.md). This is the distilled experience from hundreds of real agent sessions.

### Step 2 — Study a Real Implementation Prompt

Read [`prompts/phaseD/v7.6.0.3-implementation-instructions-for-coding-agent.md`](prompts/phaseD/v7.6.0.3-implementation-instructions-for-coding-agent.md) to see what a well-structured agent prompt looks like. Notice how it:

- States prerequisites and required reading
- Defines exact scope (what to do and what NOT to do)
- Provides an API contract
- Lists Critical Rules that must be followed
- Specifies validation commands and acceptance criteria
- Requires a mandatory session log

### Step 3 — Understand the Handoff Pattern

Read [`prompts/handoff/session-handoff-v7.6.0.4.md`](prompts/handoff/session-handoff-v7.6.0.4.md) to understand how context is transferred between sessions. Agent sessions have limited context windows — the handoff document is the bridge.

### Step 4 — See What Goes Wrong

Read [`Docs/bugs-and-lessons-learned.md`](Docs/bugs-and-lessons-learned.md) to understand the categories of problems that arise in LLM-assisted development and how the workflow evolved to prevent them.

### Step 5 — Look at a Full Session Log

Read [`Docs/session-log-2026-04-04-v7.6.0.5.md`](Docs/session-log-2026-04-04-v7.6.0.5.md) to see how an agent session is documented: what was implemented, how compliance with the prompt was verified, and what was learned.

---

## Key Concepts

### The Prompt Index

Every implementation step has a self-contained prompt file. The [`prompts/prompt-index-and-workflow.md`](prompts/prompt-index-and-workflow.md) is the master index. This prevents prompt drift — the prompt is the single source of truth for what an agent should do.

### Critical Rules

The project accumulated 44+ Critical Rules — guardrails derived from real bugs. Each rule is referenced in every prompt. Agents cannot "forget" a rule if it's explicitly required reading.

### Session Handoffs

Because LLM context windows are finite, every session produces a handoff document. The next agent starts by reading the handoff — making the workflow stateful across agent boundaries.

### Consolidated Audit After Every Step

After each PR is merged, a consolidated audit document is created (see [`prompts/phaseD/v7.6.0.3-PR119-consolidated-audit-and-lessons.md`](prompts/phaseD/v7.6.0.3-PR119-consolidated-audit-and-lessons.md)). This feeds back into the bugs-and-lessons knowledge base and future prompts.

---

## Development Workflow - Step by Step 

All documents in this repository originate from another real and live project — an ESP32 BLE gateway with multi-sensor aggregation, developed exclusively with LLM coding agents.

How actual development looks like at this moment:

- Current step's prompt is given to the coding agent
- Agent executes the prompt - depending on scope, it can be 2 or 40 minutes
- Once agent finishes the task, PR is open
- Copilot's inline review and Gemini's inline review are posted
- Other LLM's (Codex, GPT, Perplexity) full reviews of the PR are posted
- Claude agent acts on reviews
- Claude Sonnet analyzes all the reviews and fixes made by Claude Agent and determines if follow-up fixes are needed
- If follow-up fixes are needed, Claude Sonnet provides the exact prompt for the coding agent to fix the remaining issues
- The previous three steps repeat until all bugs are fixes and all deliverables from the coding agent's original prompt are fulfilled
- Once testing is done (via automated gates or manually), PR is merged and tagged
- Prompts' master index is updated with the current step's delivery
- Session handoff document is generated for the next step and if necessary next step's prompt is updated with lessons from the current step 
- After every phase is over, Claude Opus reviews all PRs and bugs and lessons from the phase and updates the master guide document and prompts for the next phase
- Next phase starts
  
---

## License

MIT

---
name: refine-prompt
description: Transform a user's raw prompt into a clear, expert-level prompt for an AI coding or reasoning agent. Use when the user asks to refine, improve, rewrite, or engineer a prompt, especially when they want the output to leverage project context, agent memories, local instruction files, or available skills.
license: MIT
---

# Refine Prompt

Use this skill to turn a rough user prompt into a copy-paste-ready prompt that another AI agent can execute well.

The refined prompt should preserve the user's intent, clarify missing structure, and use available project context without inventing facts.

## Context Discovery

Before writing the refined prompt, inspect the current agent and project context that is safely available.

Prefer local context over assumptions:

- Codex: check active instructions, loaded skills, repository files such as `AGENTS.md`, `.codex/`, `.agents/`, `README.md`, and nearby project docs.
- Claude: check `CLAUDE.md`, project memories, repository docs, and available skills or commands.
- Gemini CLI: check `GEMINI.md`, repository docs, configured memories, and available tools.
- Other agents: check their local instruction files, memory files, tool registry, and project documentation when available.

Use fast, scoped search. Do not scan huge dependency or build folders unless the user explicitly asks.

If context cannot be inspected, say that the prompt was refined from the provided text only.

## Prompt Refinement Workflow

1. Identify the user's real task from the raw prompt.
2. Select the best role for the agent, such as `Senior Software Engineer`, `QA Engineer`, `Product Manager`, `Technical Writer`, `UX Designer`, `Security Reviewer`, or `Data Analyst`.
3. Extract constraints, success criteria, expected output format, files, technologies, risks, and required verification steps.
4. Identify useful local or global skills, commands, tools, memories, or project docs. Mark these as suggestions, not requirements, unless the user explicitly requires them.
5. Write a refined prompt that another agent can paste directly into a CLI or chat.
6. Preserve uncertainty. Use phrases like "inspect and confirm" when the raw prompt lacks details.

## Output Format

Return a short lead-in followed by one fenced text block containing the refined prompt.

After the fenced block, include a concise optional note only if context was unavailable or assumptions were important.

Do not include long explanations about prompt engineering.

Use this structure inside the copy-paste block:

```text
Role: <specific expert role>

Task: <clear task distilled from the raw prompt>

Project Context To Use:
- <specific local files, memories, docs, tools, or "Inspect available project context first">

Suggested Skills / Tools:
- <skill/tool suggestion and why it may help>

Requirements:
- <concrete requirements from the raw prompt>
- <inferred requirements that are safe and useful>

Execution Guidance:
- <step-by-step guidance for the agent>
- <how to handle ambiguities>
- <how to avoid unsafe assumptions>

Output Format:
- <exact format the user should receive>

Verification:
- <tests, checks, review steps, or acceptance criteria>
```

## Quality Bar

The refined prompt must be:

- Clear enough for a different agent to act on without seeing the raw prompt.
- Faithful to the user's request.
- Specific about role, task, context, deliverables, and verification.
- Practical for agent CLIs such as Codex, Claude, Gemini CLI, Cursor, and similar tools.
- Written as an instruction to the executing agent, not as commentary about the user.

## Skill Suggestions

When suggesting skills, include:

- Skills already visible in the current agent context.
- Project-local skills discovered in directories such as `.agents/skills`, `.codex/skills`, `.claude/skills`, or similar.
- Global skills only when known or discoverable.

If no relevant skill is found, include:

```text
Suggested Skills / Tools:
- None found from available context. Inspect project-local and global skills before starting.
```

## Handling Weak Raw Prompts

If the raw prompt is vague, still produce a useful refined prompt. Add an `Assumptions To Confirm` section inside the copy-paste block only when those assumptions materially affect execution.

Do not ask the user for clarification unless the raw prompt is impossible to refine safely.

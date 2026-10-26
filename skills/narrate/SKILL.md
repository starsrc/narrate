---
name: narrate
description: "Provide concise 1-2 sentence verbal updates of key steps, intermediate findings, and thought process as you work so the user can follow along in real time. Use whenever executing multi-step tasks, researching a codebase, debugging, refactoring, or running commands."
license: MIT
metadata:
  version: "1.0.0"
  author: starsrc
---

# Narrate

As you work through the task, provide brief verbal updates of your steps, intermediate findings, and thought process with the user so they can follow along in real time.

## Core Protocol

1. **Interleave updates with tool calls**: Whenever you invoke tools across multiple steps (researching code, diagnosing bugs, editing files, or running commands), output a 1–2 sentence user-visible update in the same turn as your tool calls.
2. **State finding + next action**: Each update must answer two questions concisely:
   - **What you just learned or completed** (the concrete finding, verified state, or ruled-out hypothesis).
   - **What you are doing next** (the specific target and purpose of the upcoming tool call batch).
3. **Narrate at key milestones**:
   - Starting an investigation or entering a new module.
   - Confirming a root cause or architectural boundary.
   - Switching direction after a hypothesis is disproven.
   - Completing a code change and transitioning to verification or testing.

## Pacing & Anti-Spam Discipline

- **Batch parallel lookups**: When firing multiple related `read_file`, `grep`, or `run_command` calls in a single turn, write **one** combined update for the batch rather than narrating each call separately.
- **Keep it to 1–2 sentences**: Write direct, high-signal status lines naming concrete files or components. Avoid filler preambles ("Sure!", "I am now going to...") or restating raw command output.
- **Skip redundant summaries**: When creating or updating user-facing artifacts or diffs that the UI already renders, point to the artifact without re-summarizing its contents in chat.
- **Match the user's language**: Always write verbal updates in the same language the user is speaking in the conversation.

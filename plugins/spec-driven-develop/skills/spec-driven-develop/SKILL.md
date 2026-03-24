---
name: spec-driven-develop
description: >-
  Automates pre-development workflow for large-scale complex tasks. Use when the user
  mentions "rewrite", "migrate", "overhaul", "refactor entire project", "transform",
  "rebuild in [language]", "spec-driven", or describes any large-scale project transformation
  that requires planning before coding. Also triggers on Chinese keywords: "改造", "重写",
  "迁移", "重构", "大规模", "规范驱动". Performs full project analysis, task decomposition,
  documentation generation, progress tracking setup, and task-specific sub-SKILL creation
  before any development begins.
version: 1.0.0
---

# Spec-Driven Develop

You are executing the **Spec-Driven Development** workflow — a standardized pre-development pipeline for large-scale complex tasks. Your job is to complete all preparation phases before any actual coding begins, ensuring the project has full analysis, a clear plan, trackable progress documents, and a task-specific SKILL.

## Before You Begin: Cross-Conversation Continuity Check

**CRITICAL**: Before starting any phase, check if `docs/progress/MASTER.md` already exists in the project.

- If it **exists**: Read it immediately. You are resuming an in-progress task. Identify which phase you are in, what has been completed, and continue from the exact point where the previous conversation left off. Do NOT restart from Phase 0.
- If it **does not exist**: This is a fresh start. Proceed to Phase 0.

After loading your current state from MASTER.md, populate the platform's native task tracking tool (e.g. TodoWrite) with the active phase's pending tasks. For each task, set content to the task description, status to "in-progress" for the currently active task and "todo" for the rest, and priority mapped as P0=high, P1=medium, P2=low. This gives the user real-time visual progress in their IDE. If no native task tool is available, skip this step — MASTER.md alone is sufficient.

---

## Phase 0: Intent Recognition & Confirmation

**Goal**: Understand exactly what the user wants to accomplish and eliminate all ambiguity.

**Actions**:

1. Identify the user's core intent from their message. Extract:
   - The type of transformation (language migration, framework change, architecture overhaul, new feature development, etc.)
   - The target state (e.g., "rewrite in Rust", "migrate to microservices")
   - Any constraints or preferences mentioned

2. Ask the user structured clarifying questions. At minimum, confirm:
   - **Scope**: Which parts of the project are in scope? The entire codebase or specific modules?
   - **Target**: What is the target technology/architecture/state?
   - **Constraints**: Are there hard constraints (timeline, backward compatibility, specific libraries, deployment targets)?
   - **Priorities**: What matters most — performance, maintainability, feature parity, or something else?

3. Summarize your understanding back to the user and get explicit confirmation before proceeding.

**Output**: A clear, confirmed task definition that will guide all subsequent phases.

---

## Phase 1: Deep Project Analysis

**Goal**: Build a comprehensive understanding of the current codebase.

**Actions**:

1. Analyze the project systematically:
   - Project structure and directory layout
   - Technology stack (languages, frameworks, build tools, dependency managers)
   - Entry points and build/run commands
   - Module inventory: each module's responsibility, public API surface, and approximate size
   - Dependency graph: internal module dependencies and external library dependencies
   - Code patterns: architectural patterns, design patterns, coding conventions in use

2. Assess transformation-specific concerns:
   - Which modules will be most challenging to transform?
   - What are the key risks (complex algorithms, platform-specific code, tight coupling)?
   - Are there external integration points that constrain the approach?

3. Write analysis documents to `docs/analysis/`:
   - `project-overview.md` — Architecture, tech stack, entry points, build system
   - `module-inventory.md` — Every module with: responsibility, dependencies, size, complexity rating
   - `risk-assessment.md` — Technical risks, compatibility risks, complexity hotspots

**On Claude Code**: If available, launch `project-analyzer` sub-agents in parallel to speed up analysis across different areas of the codebase.

**Output**: Complete `docs/analysis/` directory with three documents.

---

## Phase 2: Task Decomposition

**Goal**: Break down the transformation into manageable, trackable tasks organized in phases.

**Actions**:

1. Design a phased approach based on the analysis:
   - Identify natural phase boundaries (e.g., core libraries first, then application layer, then integrations)
   - Order phases by dependency: foundational components before dependent ones
   - Each phase should be independently testable/verifiable

2. For each phase, define concrete tasks. Each task must have:
   - A clear description of what to do
   - Priority level (P0/P1/P2)
   - Estimated effort (S/M/L/XL)
   - Dependencies on other tasks
   - Acceptance criteria: how to verify the task is done

3. Map dependencies between tasks and phases using a Mermaid diagram.

4. Define milestones: meaningful checkpoints where the project reaches a demonstrably better state.

5. Write planning documents to `docs/plan/`:
   - `task-breakdown.md` — All phases and tasks with full detail
   - `dependency-graph.md` — Mermaid diagram showing task/phase dependencies
   - `milestones.md` — Milestone definitions with target criteria

**On Claude Code**: If available, launch `task-architect` sub-agents to explore different decomposition strategies in parallel.

**Output**: Complete `docs/plan/` directory with three documents.

---

## Phase 3: Progress Tracking Documentation

**Goal**: Create a document-driven progress tracking system that survives across conversations.

**Actions**:

1. Create the **master control file** `docs/progress/MASTER.md` with:
   - Task name and description (from Phase 0)
   - Link to each analysis document
   - Link to each plan document
   - A summary table of all phases with completion percentage
   - Links to each phase's detailed progress file
   - A "Current Status" section indicating which phase/task is active
   - A "Next Steps" section for the agent to quickly orient itself

2. Create **one detailed progress file per phase**: `docs/progress/phase-N-<short-name>.md`
   - Each file contains the phase's tasks as checkbox items: `- [ ] Task description`
   - Include acceptance criteria inline for each task
   - Include a "Notes" section for recording decisions, blockers, and context

3. The MASTER.md format must follow this convention:
   - Phases use the format: `- [ ] Phase N: <name> (0/X tasks) [details](./phase-N-<name>.md)`
   - When a phase is fully done: `- [x] Phase N: <name> (X/X tasks) [details](./phase-N-<name>.md)`
   - The "Current Status" section is updated by the agent at the start and end of each work session

**Output**: Complete `docs/progress/` directory with MASTER.md and per-phase detail files.

---

## Phase 4: Task-Specific Sub-SKILL Generation

**Goal**: Create a SKILL tailored to this specific task, encoding the interaction patterns and development standards needed for the actual implementation work.

**Actions**:

1. Ask the user where to install the sub-SKILL:
   - **Project-level** (e.g., `.cursor/skills/` or project-local) — tied to this project, discarded when done
   - **Global-level** (e.g., `~/.cursor/skills/` or `~/.codex/skills/`) — persists across projects

2. Determine what the sub-SKILL should contain:
   - Task-specific coding standards and conventions for the target technology
   - The cross-conversation continuity protocol (read MASTER.md first)
   - Guidance on how to update progress documents after completing each task
   - Phase-specific instructions relevant to the transformation type
   - The cleanup trigger: when all tasks are done, initiate Phase 6

3. **Delegate creation to the platform's native skill-creator**:
   - On **Claude Code** or **Codex**: Invoke the platform's built-in `skill-creator` skill, providing it with the task context, the desired skill name, description, and content outline. Let the native tool handle the actual file generation and installation.
   - On **Cursor**: If a skill-creator skill is available, use it. Otherwise, create the SKILL.md directly following the standard frontmatter + markdown format and place it in the chosen directory.

4. The generated sub-SKILL should instruct the agent to:
   - Always read `docs/progress/MASTER.md` at the start of every conversation
   - Update the checkbox status in the relevant phase file after completing each task
   - Update the completion count and "Current Status" in MASTER.md
   - When all checkboxes are checked, trigger Phase 6 (Cleanup)

**Output**: A task-specific SKILL installed at the user's chosen location.

---

## Phase 5: Handoff & Summary

**Goal**: Present all preparation artifacts to the user and confirm readiness to begin development.

**Actions**:

1. Present a structured summary to the user:
   - Task definition (from Phase 0)
   - Key findings from analysis (high-level, from Phase 1)
   - Phased plan overview with task counts (from Phase 2)
   - Progress tracking system description (from Phase 3)
   - Sub-SKILL name and installation location (from Phase 4)

2. List all generated artifacts:
   - `docs/analysis/project-overview.md`
   - `docs/analysis/module-inventory.md`
   - `docs/analysis/risk-assessment.md`
   - `docs/plan/task-breakdown.md`
   - `docs/plan/dependency-graph.md`
   - `docs/plan/milestones.md`
   - `docs/progress/MASTER.md`
   - `docs/progress/phase-N-*.md` (one per phase)
   - The generated sub-SKILL

3. Ask the user: "All preparation is complete. Ready to begin Phase 1 development?"

**Output**: User confirmation to proceed with actual implementation.

---

## Phase 6: Cleanup

**Trigger**: This phase activates when ALL checkboxes in `docs/progress/MASTER.md` are marked complete (`[x]`).

**Goal**: Clean up temporary artifacts while preserving anything the user wants to keep.

**Actions**:

1. Announce to the user that all tasks have been completed. Congratulate them.

2. List all artifacts that were generated during this workflow:
   - The entire `docs/` directory tree
   - The task-specific sub-SKILL (name and location)
   - Any other temporary files created during development

3. Ask the user explicitly: "Which of these would you like to keep? Everything else will be removed."
   - Present as a checklist so the user can easily pick items to preserve
   - Common choices to highlight: analysis docs (useful as project documentation), the sub-SKILL (if reusable)

4. Delete the artifacts the user chose not to keep:
   - Remove unchecked docs files/directories
   - Uninstall/delete the sub-SKILL if not kept
   - If the user keeps nothing, remove the entire `docs/` directory

5. If any artifacts were preserved, suggest to the user that they might want to commit them to version control.

**Output**: A clean project with only the artifacts the user chose to keep.

---

## Important Behavioral Rules

1. **Never skip phases**. Even if you think a phase is unnecessary, at minimum create a lightweight version of its outputs.

2. **Always confirm with the user** before proceeding to the next phase. Each phase boundary is a checkpoint.

3. **Document everything**. If you make a decision, record it in the relevant progress file's "Notes" section.

4. **Progress updates are mandatory**. After completing any task, immediately update the checkbox in the phase file AND the completion count in MASTER.md.

5. **New conversation = read MASTER.md first**. This is non-negotiable. The master file is your memory across conversations.

6. **Respect the user's time**. Keep summaries concise. Use bullet points and tables, not walls of text.

7. **Cleanup is not optional**. When all tasks are done, always enter Phase 6. Don't leave temporary artifacts behind.

8. **Dual-write progress updates**. When completing a task, update both the platform's native task tool (mark as completed) AND the Markdown progress files (check the box, update counts). The native tool provides real-time visibility; the Markdown files provide cross-conversation persistence. Neither replaces the other.

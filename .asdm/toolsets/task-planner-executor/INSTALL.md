# Task Planner & Executor Toolset Installation

**Toolset ID:** `task-planner-executor`

## Overview
This document provides instructions for installing and setting up the Task Planner & Executor toolset in a workspace. Task Planner & Executor helps plan and execute tasks for features in a workspace, generating feature PRDs and task lists, and managing task execution status.

## AI Guided Installation
To install this toolset using AI Guided Installation, copy and paste the following prompt into your AI Coding tool's chat window:

```shell
Follow instructions in .asdm/toolsets/task-planner-executor/INSTALL.md
```

## Installation Steps

### 1. Create `.asdm/workspace/features` directory for Task Planner's workspace

Create the directory structure for storing feature and task files:

```bash
mkdir -p .asdm/workspace/features
```

### 2. Detect the current `Agentic Engine` provider

Detect the current AI coding assistant provider (e.g., Claude Code, GitHub Copilot, Tencent CodeBuddy). Using the following guidelines to detect the provider:

- If `.claude` directory exists, use `Claude Code`
- If `.github` directory exists, use `GitHub Copilot`
- If `.codebuddy` directory exists, use `Tencent CodeBuddy`
- If no such folder is found in the current workspace, give user a prompt to select a provider manually

### 3. Create shortcuts commands for Task Planner & Executor (toolset ID: `task-planner-executor`) in provider's entry point

Create shortcut commands in the appropriate location based on the detected provider. The installation process is consistent across all providers - we use `cat` to concatenate provider-specific frontmatter with the actual instruction content:

#### For Claude Code (`.claude/commands/`):
Claude Code uses Markdown files with Frontmatter metadata for slash commands. Create commands by concatenating Claude-specific frontmatter with instruction content:

```bash
mkdir -p .claude/commands/

# Task Planning command
cat > .claude/commands/asdm-task-planning.md << 'EOF'
---
description: "Plan tasks for a workspace feature"
argument-hint: "[feature description]"
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md >> .claude/commands/asdm-task-planning.md

# Task Breakdown command
cat > .claude/commands/asdm-task-breakdown.md << 'EOF'
---
description: "Break down tasks for a workspace feature"
argument-hint: "[feature ID or name]"
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md >> .claude/commands/asdm-task-breakdown.md

# Task Execution command
cat > .claude/commands/asdm-task-execution.md << 'EOF'
---
description: "Execute tasks for a workspace feature"
argument-hint: "[task ID]"
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md >> .claude/commands/asdm-task-execution.md
```

#### For GitHub Copilot (`.github/prompts/`):
GitHub Copilot uses `.prompt.md` files with YAML frontmatter. Create prompt files by concatenating GitHub-specific frontmatter with instruction content:

```bash
mkdir -p .github/prompts/

# Task Planning prompt
cat > .github/prompts/asdm-task-planning.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Plan tasks for a workspace feature'
argument-hint: 'Enter feature description'
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md >> .github/prompts/asdm-task-planning.prompt.md

# Task Breakdown prompt
cat > .github/prompts/asdm-task-breakdown.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Break down tasks for a workspace feature'
argument-hint: 'Enter feature ID or name'
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md >> .github/prompts/asdm-task-breakdown.prompt.md

# Task Execution prompt
cat > .github/prompts/asdm-task-execution.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Execute tasks for a workspace feature'
argument-hint: 'Enter task ID'
---

EOF
cat .asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md >> .github/prompts/asdm-task-execution.prompt.md
```

#### For Tencent CodeBuddy (`.codebuddy/commands/`):
CodeBuddy doesn't support frontmatter, so simply copy the instruction files as-is:

```bash
mkdir -p .codebuddy/commands/

# Copy instruction files directly (no frontmatter needed)
cp .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md .codebuddy/commands/
cp .asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md .codebuddy/commands/
cp .asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md .codebuddy/commands/
```

### 4. Manual Usage for Other Providers

If your AI coding assistant provider is not detected by the automatic detection logic (Claude Code, GitHub Copilot, or Tencent CodeBuddy), you can still use the Task Planner & Executor manually. Follow these steps:

#### Direct Instruction Usage
You can directly use the instruction files by copying their relative paths and pasting them into your AI coding assistant's chat window:

1. **Navigate to the instruction files**:
   ```bash
   cd .asdm/toolsets/task-planner-executor/actions/
   ```

2. **Right-click on the desired instruction file** and copy its relative path:
   - For task planning: `asdm-task-planning.md`
   - For task breakdown: `asdm-task-breakdown.md`
   - For task execution: `asdm-task-execution.md`

3. **Enter a prompt** in your AI coding assistant:
   ```
   Follow the instructions in {relative path to instruction file}
   ```

## Initializing Task Planner & Executor

### Planning a New Feature
After installation, you can start planning tasks for a new feature:

```shell
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md
```

This will:
1. Generate a unique feature ID and create a feature directory
2. Generate a detailed feature PRD document using the feature-prd-spec template
3. Add the feature to the features tracking list
4. Generate a task plan with task IDs and statuses
5. Generate task PRD documents using the task-prd-spec template

### Breaking Down Tasks
After planning a feature, you can break down tasks into detailed PRD documents:

```shell
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md
```

This will:
1. Prompt you to select a feature (if no feature ID is specified)
2. Load feature context (feature PRD, task list, and additional context files)
3. Select tasks to break down based on user input or task status
4. Generate detailed task PRD documents for each selected task
5. Update the task list status if needed

### Executing Tasks
Once tasks are planned and broken down, you can execute them:

```shell
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md
```

This will:
1. Prompt you to select a task (if no task ID is specified)
2. Execute the selected task based on the task PRD
3. Update the task status in the task list

### Available Commands
Once installed, you can use the following commands:

1. **`/asdm-task-planning`** - Plan tasks for a new feature
2. **`/asdm-task-breakdown`** - Break down tasks into detailed PRD documents
3. **`/asdm-task-execution`** - Execute tasks for a feature

## Workspace Structure
The toolset will create the following structure in `.asdm/workspace/features/`:

```
.asdm/workspace/features/
├── <feature-id>-<feature-name>/        ## Workspace for a feature
│   ├── feature-prd.md                  ## Feature PRD document
│   ├── task-list.md                    ## Task list document with task IDs and statuses
│   └── <task-id>-<task-name>-prd.md    ## Task PRD document for each task
└── features-list.md                    ## Feature tracking document
```

## Spec Documents
The toolset uses the following spec documents as templates:

1. **`feature-prd-spec.md`** - Template for generating feature PRD documents
2. **`task-prd-spec.md`** - Template for generating task PRD documents
3. **`feature-list.md`** - Template for the feature tracking list
4. **`task-list.md`** - Template for the task list with statuses (TODO, IN PROGRESS, DONE)

## Verification

After installation, verify that:

1. The `.asdm/workspace/features` directory exists for Task Planner & Executor
2. Shortcut commands for Task Planner & Executor (toolset ID: `task-planner-executor`) are created in the appropriate provider directory (if using Claude Code, GitHub Copilot, or Tencent CodeBuddy)
3. The Task Planner & Executor toolset files are located in `.asdm/toolsets/task-planner-executor` (toolset ID: `task-planner-executor`)

**For other providers**: Verify that you can access the instruction files at:
- `.asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md`
- `.asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md`
- `.asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md`

## Usage Examples

### Planning a New Feature
```shell
# First, install the toolset using AI Guided Installation
Follow instructions in .asdm/toolsets/task-planner-executor/INSTALL.md

# Then plan tasks for a new feature
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md

# Example prompt when using slash command:
/asdm-task-planning Add user authentication with OAuth support
```

### Breaking Down Tasks
```shell
# Break down tasks for a feature
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-breakdown.md

# Example prompt when using slash command:
/asdm-task-breakdown FEA-001-User-Auth
```

### Executing a Task
```shell
# Execute a specific task
Follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-execution.md

# Example prompt when using slash command:
/asdm-task-execution TASK-001
```

## Usage

### For Supported Providers (Claude Code, GitHub Copilot, Tencent CodeBuddy)
Once installed, you can use the following commands:

- `/asdm-task-planning {feature description}`: Plan tasks for a new feature
- `/asdm-task-breakdown {feature ID or name}`: Break down tasks into detailed PRD documents
- `/asdm-task-execution {task ID}`: Execute a specific task

### For Other Providers (Manual Usage)
If your provider is not automatically detected, you can manually use the instructions by following the steps in the "Manual Usage for Other Providers" section above.

## Notes

- This installation process assumes you have the necessary permissions to create directories and files
- The actual implementation of the commands will be handled by the AI model using the templates and instructions provided in Task Planner & Executor (toolset ID: `task-planner-executor`)
- Make sure to customize the provider-specific setup based on your actual AI coding assistant
- The toolset ID `task-planner-executor` should be used consistently when referring to Task Planner & Executor in commands and documentation
- **For providers not in the detection logic**: Users can manually use the instruction files by copying their relative paths and entering prompts like "follow the instructions in .asdm/toolsets/task-planner-executor/actions/asdm-task-planning.md"
- Task statuses: TODO, IN PROGRESS, DONE
- The toolset automatically updates the features tracking list when new features are generated

## Integration with Other Toolsets
Task Planner & Executor can integrate with Context Builder to understand the workspace better during task planning and execution. Context files from Context Builder can be referenced to ground the generated tasks and PRDs to the actual codebase.

### Getting Help
For issues with Task Planner & Executor toolset, refer to:
- [ASDM Documentation](https://asdm.ai/docs)
- Toolset README: `.asdm/toolsets/task-planner-executor/README.md`
- Spec documents in `.asdm/toolsets/task-planner-executor/spec/`

## License
Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.

---

*This installation document is part of the Task Planner & Executor toolset. Use the task planning instruction to create feature and task plans for your workspace.*

# GitHub Issue Assignment Command

## Overview

This command orchestrates a multi-agent workflow to triage and assign unassigned Nx repository issues.

The workflow uses:
- **github-issue-triage skill** - Provides team expertise mapping, assignment rules, and priority classification
- **Analysis Agent** - Fetches and analyzes the last 25 unassigned issues
- **Assignment Agent** - Performs bulk assignments, labels, and priorities
- **Verification** - Opens assigned issues in browser for manual review

## Workflow

The command will execute in three stages:

### Stage 1: Issue Analysis
A Task agent will:
1. Fetch the last 25 unassigned Nx issues using: `gh issue list --repo nrwl/nx --state open --search "type:issue no:assignee" --limit=25`
2. Analyze each issue using the `github-issue-triage` skill
3. Generate assignment recommendations with:
   - Recommended assignee (based on technology keywords)
   - Suggested scope label
   - Suggested priority level
   - Reasoning for the assignment

### Stage 2: Bulk Assignment
A Task agent will:
1. Execute bulk `gh issue edit` commands to assign issues
2. Apply scope labels by technology group
3. Apply priority labels conservatively
4. Generate a comprehensive list of all assigned issue numbers

### Stage 3: Browser Verification
Execute platform-specific browser opening commands:
- **macOS**: `for issue in NUMBERS; do open "https://github.com/nrwl/nx/issues/$issue"; done`
- **Linux**: `for issue in NUMBERS; do xdg-open "https://github.com/nrwl/nx/issues/$issue"; done`
- **Windows**: `for issue in NUMBERS; do start "https://github.com/nrwl/nx/issues/$issue"; done`

Every assigned issue MUST be opened in the browser for manual review.

## Assignment Rules Reference

The `github-issue-triage` skill contains:

**Team Members:**
- @barbados-clemens - Documentation, lifecycle hooks, developer experience, API docs
- @lourw - Java/Gradle
- @leosvelperez - Angular (primary), TypeScript, testing tools (Jest/Cypress/Playwright), ESLint issues
- @Coly010 - Webpack/Rollup/Rspack configs, Module Federation, Storybook, Vue, bundler optimization, React Native, Nx Release, Publishing, migration utilities, React, Node/NestJS/Express, NextJS, Remix, create-nx-workspace, preset issues
- @AgentEnder - Nx Core (caching/daemon/graph), installation issues, plugin system, devkit, affected calculation

**Technology Keywords:**
- "Angular", "ng serve", "@angular" → @leosvelperez
- "Nest", "NestJS", "@nx/nest" → @Coly010
- "Next", "NextJS", "@nx/next" → @Coly010
- "React", "@nx/react" → @Coly010
- "webpack", "rollup", "rspack", "bundler" → @Coly010
- "Module Federation", "MF" → @Coly010
- "cache", "daemon", "affected", "nx reset" → @AgentEnder
- "plugin", "generator", "executor" → @AgentEnder
- "docs", "documentation", "lifecycle" → @barbados-clemens

For complete rules, see the `github-issue-triage` skill.

## How It Works

### Stage 1 Agent Task
The analysis agent will:
- Use the `github-issue-triage` skill to evaluate each issue
- Check technology keywords in title/body against team expertise
- Analyze error patterns to identify the problem area
- Recommend scope labels matching the technology
- Apply priority rules conservatively
- Output a structured list of assignments with reasoning

### Stage 2 Agent Task
The assignment agent will:
- Take recommendations from Stage 1
- Execute `gh issue edit` commands to assign issues to team members
- Apply scope and priority labels in bulk operations
- Maintain a comprehensive list of all issue numbers assigned

### Stage 3 Verification
After all assignments:
1. Use the appropriate browser opening command for your OS
2. Open ALL assigned issues in browser tabs
3. Manually review each assignment
4. Verify that assignees are appropriate
5. Do not consider the process complete until all issues are reviewed

## Key Benefits

- **Skill Reusability**: The `github-issue-triage` skill can be used in other workflows beyond this command
- **Multi-Agent Efficiency**: Agents can work in parallel on different stages
- **Comprehensive Rules**: All assignment logic is centralized in the skill for consistency
- **Validated Assignments**: Mandatory browser verification ensures quality
- **Scalable**: Easy to add new team members or technologies to the skill
- **Maintainable**: Changes to rules only need to be updated in the skill file

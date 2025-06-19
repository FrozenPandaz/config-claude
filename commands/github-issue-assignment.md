# GitHub Issue Assignment Command

## Command

```
Analyze the last 25 unassigned GitHub issues for the Nx repository with improved assignment logic.

Fetch issues with:
gh issue list --repo nrwl/nx --state open --search "type:issue no:assignee" --limit=25

**Enhanced Team Assignment Rules (Technology-First Approach):**
- @barbados-clemens - Documentation, lifecycle hooks, developer experience, API docs
- @lourw - Java/Gradle
- @leosvelperez - Angular (primary), TypeScript, testing tools (Jest/Cypress/Playwright), ESLint issues
- @Coly010 - Webpack/Rollup/Rspack configs, Module Federation, Storybook, Vue, bundler optimization, React Native, Nx Release, migration utilities, React, Node/NestJS/Express, NextJS, Remix, create-nx-workspace, preset issues
- @AgentEnder - Nx Core (caching/daemon/graph), installation issues, plugin system, devkit, affected calculation

**Smart Assignment Logic (Check in Order):**
1. **Technology Keywords in Title/Body:**
   - "Angular", "ng serve", "@angular" → @leosvelperez
   - "Nest", "NestJS", "@nx/nest" → @ndcunningham  
   - "Next", "NextJS", "@nx/next" → @ndcunningham
   - "React", "@nx/react" → @ndcunningham
   - "webpack", "rollup", "rspack", "bundler" → @Coly010
   - "Module Federation", "MF" → @Coly010
   - "create-nx-workspace", "preset" → @ndcunningham
   - "cache", "daemon", "affected", "nx reset" → @AgentEnder
   - "plugin", "generator", "executor" → @AgentEnder
   - "docs", "documentation", "lifecycle" → @barbados-clemens

2. **Error Pattern Analysis:**
   - Compilation/build failures with Angular → @leosvelperez
   - Webpack configuration errors → @Coly010
   - Installation/dependency issues → @AgentEnder
   - Preset/generator failures → Check technology first, fallback @AgentEnder

3. **Scope Assignment:**
   - Match scope to technology mentioned in issue
   - Use "scope: core" only for caching/daemon/graph issues
   - Use "scope: misc" for installation/setup issues

**Conservative Priority Rules:**
- **High**: Blocks many users (workspace creation, compilation failures, security CVEs)
- **Medium**: Standard bugs, configuration issues, generator problems
- **Low**: Documentation, UI improvements, edge cases

**Validation Steps:**
1. Check issue body for actual error messages/stack traces
2. Look at reproduction steps to identify real problem area  
3. Consider if issue affects framework specifically vs Nx core
4. Verify priority based on user impact scope
```

## Implementation Steps

1. Fetch and analyze issues with detailed content inspection
2. Apply technology-first assignment logic
3. Bulk assign using GitHub CLI:
   ```bash
   gh issue edit [numbers] --repo nrwl/nx --add-assignee [username]
   ```
4. Apply scope labels by technology groups
5. Apply priority labels conservatively
6. **MANDATORY: Open ALL assigned issues in browser for verification and review**
   - Use the browser commands below to open each issue
   - Review assignments before finalizing

## Bulk Operations Commands

```bash
# Example assignment by technology groups
gh issue edit 12345 12346 --repo nrwl/nx --add-assignee ndcunningham
gh issue edit 12347 12348 --repo nrwl/nx --add-assignee leosvelperez

# Apply scopes by category  
gh issue edit 12345 12346 --repo nrwl/nx --add-label "scope: node"
gh issue edit 12347 12348 --repo nrwl/nx --add-label "scope: angular"

# Apply priorities
gh issue edit 12345 --repo nrwl/nx --add-label "priority: high"
gh issue edit 12346 12347 --repo nrwl/nx --add-label "priority: medium"
```

## Browser Review Command

**MANDATORY: Open ALL assigned issues in browser for verification and review**

After completing issue assignments, you MUST open every single assigned issue in the browser for manual validation. No exceptions - every issue that gets assigned must be opened for human review.

```bash
# macOS - Opens all issues in browser tabs
for issue in [space_separated_issue_numbers]; do open "https://github.com/nrwl/nx/issues/$issue"; done

# Linux - Opens all issues in browser tabs  
for issue in [space_separated_issue_numbers]; do xdg-open "https://github.com/nrwl/nx/issues/$issue"; done

# Windows - Opens all issues in browser tabs
for issue in [space_separated_issue_numbers]; do start "https://github.com/nrwl/nx/issues/$issue"; done

# Alternative: Use gh CLI to open issues directly (one at a time)
gh issue view [issue_number] --repo nrwl/nx --web
```

**Example Usage:**
```bash
# Linux example with actual issue numbers from assignment
for issue in 32296 32295 32290 32285 32281 32277 32267 32265 32258 32254 32249 32236 32234 32225 32219 32214 32212 32209 32208 32207 32206 32205 32203 32190 32188; do xdg-open "https://github.com/nrwl/nx/issues/$issue"; done

# macOS example with actual issue numbers from assignment
for issue in 32296 32295 32290 32285 32281 32277 32267 32265 32258 32254 32249 32236 32234 32225 32219 32214 32212 32209 32208 32207 32206 32205 32203 32190 32188; do open "https://github.com/nrwl/nx/issues/$issue"; done
```

**Critical Requirements:**
1. Every single assigned issue MUST be opened in browser
2. Create a comprehensive list of ALL issue numbers that were assigned
3. Use the appropriate command for the operating system
4. Verify that all browser tabs opened successfully
5. Do not consider the assignment process complete until all issues are opened for review

## Key Features

- Technology-first assignment (Angular issues go to Angular expert, not general web expert)
- Keyword-based logic for faster categorization
- Conservative priority assignment (fewer high priority items)
- Error pattern recognition for better accuracy
- Validation steps to double-check assignments
- Bulk operations for efficiency
# GitHub Issue Assignment Command v2

## Command

```
Analyze the last 25 unassigned GitHub issues for the Nx repository with improved assignment logic.

Fetch issues with:
gh issue list --repo nrwl/nx --state open --search "type:issue no:assignee" --limit=25

**Enhanced Team Assignment Rules (Technology-First Approach):**
- @barbados-clemens - Documentation, lifecycle hooks, developer experience, API docs
- @ndcunningham - React, Node/NestJS/Express, NextJS, Remix, create-nx-workspace, preset issues
- @xiongemi - Java/Gradle, React Native, Nx Release, migration utilities  
- @leosvelperez - Angular (primary), TypeScript compilation, testing tools (Jest/Cypress/Playwright) when Angular-related
- @Coly010 - Webpack/Rollup/Rspack configs, Module Federation, Storybook, Vue, bundler optimization
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
6. Open issues in browser for verification

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

```bash
for issue in [issue_numbers]; do open https://github.com/nrwl/nx/issues/$issue; done
```

## Key Improvements from v1

- Technology-first assignment (Angular issues go to Angular expert, not general web expert)
- Keyword-based logic for faster categorization
- Conservative priority assignment (fewer high priority items)
- Error pattern recognition for better accuracy
- Validation steps to double-check assignments
- Bulk operations for efficiency
# GitHub Issue Assignment Command

## Command

```
Analyze the last 25 unassigned GitHub issues for the Nx repository.

Unassigned Github issues can be retrieved with the command, `gh issue list --repo nrwl/nx --state open --search "type:issue no:assignee" --limit=25`

Assign, prioritize, and socpe them based on the following criteria:

**Team Assignment Rules:**
- @barbados-clemens - Caleb handles Documentation issues
- @ndcunningham - Nicholas handles React, Node, Self hosted cache, and create-nx-workspace issues
- @xiongemi - Emily handles Java, React Native, Nx Release
- @leosvelperez - Leo handles TypeScript, testing tools (Playwright, Cypress, Jest, ESLint)
- @Coly010 - Colum handles Web technologies such as Vue, Angular, Module Federation, Storybook, webpack, rspack, rollup
- @AgentEnder - Craigory handles Nx Core, Nx Devkit, configuration, Nx plugins, daemon issues

**Scope Assignment Rules:**
Assign appropriate scope labels based on issue content:
- scope: core - Core Nx functionality
- scope: devkit - Nx DevKit related
- scope: plugins - Plugin development/issues
- scope: angular - Angular support
- scope: react - React support
- scope: vue - Vue support
- scope: node - Node/Express/NestJS support
- scope: nextjs - NextJS support
- scope: remix - Remix support
- scope: java - Java/Gradle support
- scope: react-native - React Native support
- "scope: testing tools" - Cypress/Jest/Playwright/Vitest
- scope: linter - ESLint support
- scope: bundlers - Webpack/Rollup issues
- scope: module federation - Module federation
- scope: storybook - Storybook support
- scope: release - Nx Release functionality
- scope: powerpack - Nx Powerpack features
- scope: nx-cloud - Nx Cloud related
- scope: docs - Documentation
- scope: misc - Miscellaneous issues

**Priority Classification:**
- High: Critical functionality failures, performance issues, CI/CD blocks, that would affect many people
   - Be convservative on what gets high priority. Even if the functionality is failing, it should only be high priority if it affects many people.
- Medium: Standard bugs, feature improvements, configuration issues
- Low: Documentation improvements, minor UX issues

**Output Format:**
Report the analysis with:
1. Issues grouped by assignee
2. Include issue number, title, priority level, and recommended scope label
3. Brief explanation of why each issue fits that team member's expertise and scope
4. Summary statistics (priority distribution, scope distribution, workload balance)
```

## Make these updates to the github issues 

1. Add a labels for scope: and priority: and assign the tasks in bulk using the `--add-assignee` flag.

2. Apply the assignees in bulk, then apply the scope: in bulk, and then apply the priority label in bulk.

## Showing Results

Show results in a compact format where each issue is only given a single line.

Open all the issues by opening them in the browser one by one with the following command:
```bash
for issue in [List of issues]; do open
      https://github.com/nrwl/nx/issues/$issue; done)
```

## Customization Options

You can modify the command by:
- Changing the number of issues to analyze (default: 25)
- Adjusting priority criteria
- Adding/removing team members
- Changing the date range for issue fetching

## Example Variations

**For Specific Time Period:**
```
Analyze unassigned GitHub issues from the last 7 days and assign them...
```

**For Specific Labels:**
```
Analyze unassigned GitHub issues with labels "bug" or "type: feature" from the last 30 days and assign them...
```

**For Workload Balancing:**
```
Analyze the last 25 unassigned GitHub issues, assign them to team members, and highlight any workload imbalances. Suggest reassignments if needed...
```
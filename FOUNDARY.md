# Foundry: Jira ticket loop

Project workflow that pulls your Jira assignments, plans with human approval, implements on a `ticket/<KEY>` branch inside the run worktree, opens a GitHub PR, agent-reviews until clean, then human-reviews and merges.

## Run

```bash
fabro run jira-ticket-loop
```

The name resolves from `.fabro/workflows/jira-ticket-loop/` the same way `fabro run hello` does. Optional JQL:

```bash
fabro run jira-ticket-loop -I jira_jql='assignee = currentUser() AND status != Done'
```

Validate without executing:

```bash
fabro validate jira-ticket-loop
```

## Secrets and GitHub

Fetch uses the `jira` CLI if present, otherwise the Jira REST API (`JIRA_BASE_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` / `JIRA_TOKEN`). Export those in the environment, or map vault secrets in `workflow.toml` (see the commented `[environments.fabro-dev.env]` block there).

```bash
fabro secret set JIRA_BASE_URL https://your-org.atlassian.net
fabro secret set JIRA_EMAIL you@example.com
fabro secret set JIRA_API_TOKEN <api-token>
```

The workflow requests GitHub `contents` + `pull_requests` write and sets `[run.pull_request] enabled = false` so Fabro does not open a second PR after the graph has already opened and merged with `gh`.

<!-- page-journey: all -->
<!-- page-adventure: side-quest -->
# Side Quest: Allowlist Enterprise Domains in Your Workflow Firewall

> _Agentic workflows run behind a firewall that blocks unknown domains, so enterprise hosts need to be on the list before your agent can reach them._

## :dart: What You'll Do

You'll find the domains your enterprise workflow needs, add them to `network.allowed`, and confirm from the firewall log that nothing was blocked. This is the most common fix when a workflow works on github.com but fails on GitHub Enterprise Server (GHES) or behind a corporate gateway.

## :clipboard: Before You Start

- You completed [Self-Hosted Runner Infrastructure Deep Dive](side-quest-24-01-runner-infrastructure.md) or are working through it.
- You have a workflow in your repository that you can edit and recompile.
- You know your enterprise hostnames. Ask your admin if you do not.

## Steps

### Understand what the firewall does

The agent runs inside a sandbox. Its outbound traffic goes through a firewall that only allows domains you list. A blocked request usually looks like a timeout or "could not resolve host" error in the agent log, not a clear permission message.

### List the hosts your workflow touches

Write down every host your agent needs. Typical enterprise examples:

| Host | Why the agent needs it |
|------|------------------------|
| `github.example.com` | Your GHES instance web and API host |
| `api.example.com` | An internal API your brief calls |
| `models.example.com` | A model gateway your admin provides |
| `*.packages.example.com` | Internal package registries (wildcard) |

> [!NOTE]
> The hostnames above are examples. Replace them with the real values from your network team.

### Add the hosts to your frontmatter

Open your workflow `.md` file and add the hosts under `network.allowed`. Keep `defaults` so the standard GitHub and engine endpoints stay reachable.

```markdown
---
on:
  workflow_dispatch:
permissions:
  contents: read
engine: copilot
network:
  allowed:
    - defaults
    - github.example.com
    - api.example.com
    - "*.packages.example.com"
---

# Enterprise Status Check

Read the repository's recent activity and summarize it.
```

Prefer exact hostnames over wildcards. A wildcard lets the agent reach every subdomain, so use it only when you trust all of them.

### Recompile the workflow

```bash
gh aw compile
```

Commit the `.md` file and the updated `.lock.yml` file, then push.

### Run it and read the firewall log

1. Open **Actions**, pick your workflow, and select **Run workflow**.
2. When the run finishes, download the `agent` artifact or run `gh aw audit <run-id>`.
3. Open `firewall.md` and look at the **blocked** list.
4. If a needed domain appears there, add it to `network.allowed`, recompile, and run again.

Repeat until the blocked list contains only domains you intended to block.

> [!TIP]
> Treat the allowed list in `firewall.md` as a review document. Share it with your security team so they can approve the exact domains your agent contacts.

### Keep the list small

Every extra domain widens what an injected prompt could reach. Remove entries your workflow no longer uses each time you edit it. See [Audit Reference](side-quest-25-01-audit-reference.md) for how to read the firewall records in detail.

## :white_check_mark: Checkpoint

- [ ] You listed at least two enterprise hostnames your workflow needs
- [ ] Your frontmatter has a `network.allowed` list that includes `defaults` plus your hosts
- [ ] `gh aw compile` succeeded and you committed both the `.md` and `.lock.yml` files
- [ ] You opened `firewall.md` after a run and checked the blocked list
- [ ] You can explain why exact hostnames are safer than wildcards

<!-- journey: all -->
Return to [Run Your Agentic Workflow on a Self-Hosted Runner](24-self-hosted-runners.md).
<!-- /journey -->

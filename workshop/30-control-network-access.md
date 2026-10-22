<!-- page-journey: all -->
<!-- page-adventure: advanced -->
# Control Which Domains Your Agent Can Reach

> _An agent that can reach any website can leak data to any website — a short allowlist keeps your workflow useful and contained._

## :dart: What You'll Do

You'll read the [network permissions](https://github.github.com/gh-aw/reference/network/) of one of your workflows, narrow them to the domains it really needs, and trigger a blocked request on purpose. You'll finish with a workflow that follows least privilege for network access, which many enterprise security teams require.

## :clipboard: Before You Start

- You have completed [Teach Your Agent Domain Knowledge with Skills](29-skills-and-domain-knowledge.md).
- You have a workflow that has run at least once, such as your daily status report.

## Steps

### Understand the firewall

Every agentic workflow runs behind a firewall. The firewall only lets the agent contact domains listed under `network:` in your frontmatter. With no `network:` field, gh-aw uses `defaults`, which allows basic infrastructure such as certificate authorities and package mirrors.

The same list also protects output: links to domains outside the allowlist are replaced with `(redacted)` in what the agent writes.

### Choose ecosystem identifiers

Instead of listing every domain, use short names that stand for a group of domains:

| Identifier | What it allows |
|------------|----------------|
| `defaults` | Basic infrastructure |
| `github` | GitHub domains |
| `python` | PyPI and related package hosts |
| `node` | npm, yarn, and pnpm |

Add a single domain, such as `api.example.com`, when you need one specific service. Listed domains also match their subdomains.

### Narrow your workflow's allowlist

Ask your AI agent to tighten the list. In your Codespace terminal, start Copilot:

```bash
gh copilot
```

Then paste this prompt:

```prompt
/agentic-workflows Restrict the network access of my daily status workflow to only the domains it needs.
```

Review the result. A minimal frontmatter looks like this:

```yaml
---
on:
  schedule: daily
network:
  allowed:
    - defaults
    - github
---
```

### Block a domain on purpose

Use `blocked` to exclude something from the allowed set. Blocked entries always win:

```yaml
network:
  allowed:
    - defaults
    - github
    - node
  blocked:
    - python
```

### Compile and test

Compile in the terminal so the firewall rules are baked into the lock file:

```bash
gh aw compile
```

Run the workflow from the **Actions** tab, then audit the run to see which domains were allowed and which were blocked:

```bash
gh aw audit <run-id> --parse
```

Open `firewall.md` in the output. If a domain your workflow legitimately needs was blocked, add it to `network.allowed` and compile again.

> [!TIP]
> Start with `defaults` plus `github`, run once, then add only what the audit shows you need. Avoid adding broad identifiers "just in case."

> :bulb: **Optional Side Quest:** On a self-hosted runner behind a proxy? See [Self-Hosted Runner Infrastructure Deep Dive](side-quest-24-01-runner-infrastructure.md). For reading firewall logs in depth, see [Audit Reference](side-quest-25-01-audit-reference.md).

## :white_check_mark: Checkpoint

- [ ] You can explain what happens when a workflow has no `network:` field
- [ ] Your workflow lists only `defaults`, `github`, or other domains it needs under `network.allowed`
- [ ] You can describe why `blocked` entries override `allowed` entries
- [ ] `gh aw compile` succeeded after your network change
- [ ] You opened `firewall.md` from an audited run and identified one allowed or blocked domain

<!-- journey: all -->
Want to choose another branch from the workshop hub? Return to [What's Next? Keep Exploring](14-next-steps.md).
<!-- /journey -->

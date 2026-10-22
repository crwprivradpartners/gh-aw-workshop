<!-- page-journey: all -->
<!-- page-adventure: advanced -->

# Preview Safe Outputs with Staged Mode

> _See exactly what your workflow would create — issues, comments, pull requests — before it writes anything to your repository._

## :dart: What You'll Do

You'll turn on staged mode for a workflow, run it, and read the preview in the run summary. Then you'll switch staging off for one output at a time once you trust the results. Staged mode lets the agent do its full analysis while the writes stay on hold.

## :clipboard: Before You Start

- You completed [Teach Your Agent Domain Knowledge with Skills](29-skills-and-domain-knowledge.md).
- You have a workflow that uses [safe outputs](https://github.github.com/gh-aw/reference/safe-outputs/) such as `create-issue` or `add-comment`.
- You can run `gh aw compile` in your Codespace terminal.

## Steps

### Turn on staged mode

1. Open your workflow's `.md` file in the Codespace.
2. Add `staged: true` inside the `safe-outputs:` block:

```yaml
---
on: workflow_dispatch

safe-outputs:
  staged: true
  create-issue:
    title-prefix: "[ai] "
    labels: [automation]
---

# Issue Analyzer

Review recent activity and suggest follow-up tasks.
```

3. Recompile so the change takes effect:

```bash
gh aw compile
```

### Run and read the preview

4. Run the workflow from the **Actions** tab on GitHub.com.
5. Open the finished run and select the **Summary** page.
6. Find the headings marked with 🎭. Each one lists an operation the agent would have performed, with its title, body, and labels.
7. Confirm that no new issue appeared in the **Issues** tab. That proves nothing was written.

> [!NOTE]
> Staging previews built-in safe outputs only. It is not a sandbox: custom jobs, external tools, and memory writes still run as usual.

### Stage one output at a time

Once the issue previews look right, you can set `staged` on a single output type instead of the whole block. This example keeps pull requests in preview while comments go live:

```yaml
safe-outputs:
  create-pull-request:
    staged: true
  add-comment:
```

8. Edit your workflow so only the output you still want to review is staged.
9. Run `gh aw compile` again and trigger another run.
10. Check the summary: staged outputs show a 🎭 preview, and the others show real results.

> [!TIP]
> A global `staged: true` always wins. A type-level `staged: false` cannot turn it off, so remove the global line when you are ready to go live.

### Go live

11. Delete the `staged: true` lines, recompile, and run once more.
12. Confirm the real issue, comment, or pull request now appears.

## :white_check_mark: Checkpoint

- [ ] Your workflow's `safe-outputs:` block contains `staged: true` and `gh aw compile` succeeded
- [ ] A run summary shows at least one 🎭 preview heading with the proposed title and body
- [ ] No new issue, comment, or pull request was created while staging was on
- [ ] You staged a single output type and saw staged and live results in the same run
- [ ] You removed staging and confirmed a real output appeared

**Next:** [What's Next? Keep Exploring](14-next-steps.md)

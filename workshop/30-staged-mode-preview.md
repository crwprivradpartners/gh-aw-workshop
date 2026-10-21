<!-- page-journey: all -->
<!-- page-adventure: advanced -->
# Preview Safe Outputs with Staged Mode

> _Run your workflow for real, see exactly what it would write to GitHub, and only then let it write._

## :dart: What You'll Do

You'll turn on [staged mode](https://github.github.com/gh-aw/reference/staged-mode/) for a workflow, run it, and read the preview in the run summary. Then you'll stage just one output type and finally switch staging off. By the end, you can safely test a new or changed workflow without creating stray issues, comments, or pull requests.

## :clipboard: Before You Start

- You completed [Teach Your Agent Domain Knowledge with Skills](29-skills-and-domain-knowledge.md), or you have any working workflow in your repository that uses `safe-outputs:`.
- You can run a workflow from the **Actions** tab or with `gh aw run`.

## Steps

### Understand what staging does

A [safe output](https://github.github.com/gh-aw/reference/safe-outputs/) is a write the agent asks for, such as creating an issue or posting a comment. In staged mode, the agent still does all its thinking. But each built-in write is replaced by a preview in the run summary, marked with a :performing_arts: heading.

> [!NOTE]
> Staging is not a sandbox. It previews built-in safe outputs only. Custom jobs, external tools, and memory writes need their own controls.

### Turn on staging

Open your workflow in `.github/workflows/` and add `staged: true` inside `safe-outputs:`. Here's a complete example:

```yaml
---
on:
  schedule: daily
  workflow_dispatch: {}

permissions:
  contents: read
  issues: read

safe-outputs:
  staged: true
  create-issue:
    title-prefix: "[ai] "
    labels: [automation]
---

# Issue Summary

Summarise the five most recently opened issues and file one follow-up issue.
```

Compile after every meaningful change:

```bash
gh aw compile
```

### Run and read the preview

Trigger the workflow, then open the finished run on the **Actions** tab. Select the run's **Summary** page and scroll to the preview. Each heading starts with :performing_arts: and lists the title, body, and labels the agent would have used. The footer says no GitHub resources were created.

Check the preview against what you wanted. If the title or body is off, edit your task brief, compile, and run again. Nothing has been written yet, so you can iterate freely.

### Stage only one output type

Once you trust some outputs, stage only the risky ones. Set `staged: true` on one type and leave the global setting off:

```yaml
safe-outputs:
  create-issue:
    title-prefix: "[ai] "
  add-comment:
    staged: true
```

Here the issue is created for real, but comments are only previewed. A type is staged if either the global setting or its own setting is `true`. A type-level `staged: false` can't switch off global staging.

### Go live

When the previews look right, remove `staged: true` (or set it to `false`), compile, commit, and push:

```bash
gh aw compile
git add .
git commit -m "feat: enable live safe outputs"
git push
```

Run the workflow once more and confirm the real issue or comment appears.

> [!TIP]
> Keep staging on while you change a brief, a trigger, or a skill. Switch it off only after one clean preview.

## :white_check_mark: Checkpoint

- [ ] Your workflow has `staged: true` under `safe-outputs:` and `gh aw compile` succeeded
- [ ] You ran the workflow and found the :performing_arts: preview in the run summary
- [ ] The run created no new issues, comments, or pull requests
- [ ] You staged a single output type while another stayed live
- [ ] You removed staging and confirmed a real output was written

**Next:** [What's Next? Keep Exploring](14-next-steps.md)

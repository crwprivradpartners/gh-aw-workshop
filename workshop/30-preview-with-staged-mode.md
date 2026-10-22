<!-- page-journey: all -->
<!-- page-adventure: advanced -->
<!--
<research-metadata>
  <focus>Staged mode for safe outputs — previewing the issues, comments, and pull requests a workflow would create without applying the writes</focus>
  <sources>
    <source>https://github.github.com/gh-aw/reference/staged-mode/</source>
    <source>https://github.github.com/gh-aw/reference/safe-outputs/</source>
  </sources>
  <rationale>The curriculum teaches learners to build and ship workflows with write-capable safe outputs (steps 14b, 22, 28), and the enterprise-focused steps cover runners, audit, and cost controls. No step shows how to rehearse a workflow's writes before enabling them. Staged mode gives teams a safe review gate for new or changed workflows and is a common requirement in managed environments.</rationale>
</research-metadata>
-->

# Preview What Your Workflow Would Do with Staged Mode

> _Run your workflow for real, read what it would have written, and only then let it write._

## :dart: What You'll Do

You'll turn on **staged mode** for a workflow that creates issues or comments. The agent still runs end to end, but gh-aw replaces each write with a preview in the run summary. You'll read that preview, then switch staging off when you trust the output.

## :clipboard: Before You Start

- You completed [Teach Your Agent Domain Knowledge with Skills](29-skills-and-domain-knowledge.md), or you have any workflow that uses a [safe output](https://github.github.com/gh-aw/reference/safe-outputs/) such as `create-issue` or `add-comment`.
- You can compile workflows with `gh aw compile` from [Using `gh aw compile` to Catch Errors Early](side-quest-07-01-compile-workflow.md).

## Steps

### Understand what staging does

A safe output is the controlled way your agent writes to GitHub. In **staged mode**, the agent still reads data, reasons, and proposes outputs. gh-aw simply skips the write and records what it would have done.

This is useful when you change a prompt, add a new output type, or roll a workflow out to a shared repository and want a human to review first.

> [!NOTE]
> Staging only covers built-in safe outputs. Custom jobs, scripts, external tool servers, and memory persistence need their own controls.

### Enable staging for all outputs

1. Open one of your workflows in `.github/workflows/` that already has a `safe-outputs:` block.
1. Add `staged: true` at the top of that block:

   ```yaml
   safe-outputs:
     staged: true
     create-issue:
       title-prefix: "[ai] "
       labels: [automation]
   ```

1. Recompile so the change takes effect:

   ```bash
   gh aw compile
   ```

### Run the workflow and read the preview

1. Trigger the workflow the way you normally do, for example from the **Actions** tab with **Run workflow**.
1. When the run finishes, open it and select the **Summary** page.
1. Find the heading that starts with 🎭. It lists each operation with its type, title, body, labels, and other fields.
1. Confirm that no new issue appears in the **Issues** tab.

The summary ends with a line telling you no GitHub resources were created.

### Stage only one output type

Once a workflow is trusted, you can stage just the new part. Set `staged: true` on one output and leave the rest live:

```yaml
safe-outputs:
  create-pull-request:
    staged: true
  add-comment:
```

Here pull requests are previewed while comments are posted normally. A type-level `staged: false` cannot override a global `staged: true`.

### Go live

1. Adjust your prompt until the preview looks right, and rerun as needed.
1. Remove `staged: true` (or set it to `false`).
1. Run `gh aw compile` again and trigger one more run to confirm the real write happens.

> [!TIP]
> Keep staging on while you iterate on prompts, then turn it back on for a single output type whenever you add a new one.

## :white_check_mark: Checkpoint

- [ ] You added `staged: true` to the `safe-outputs:` block of a workflow and ran `gh aw compile` successfully
- [ ] You triggered a run and found the 🎭 preview in the run summary
- [ ] You confirmed that no real issue, comment, or pull request was created while staging was on
- [ ] You can explain how to stage a single output type without staging the others
- [ ] You removed staging and verified that the next run performed the real write

<!-- journey: all -->
Want to choose another branch from the workshop hub? Return to [What's Next? Keep Exploring](14-next-steps.md).
<!-- /journey -->

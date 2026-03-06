# Permission Preset Templates

Templates for `.claude/settings.local.json` generation. Adapt `{{test_command}}`, `{{install_command}}`, and `{{domain_urls}}` based on detected tech stack.

---

## Restrictive Preset

Read-only by default, ask for everything else.

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(ls:*)",
      "Bash(find:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)"
    ],
    "deny": [
      "Bash(git push --force:*)",
      "Bash(git reset --hard:*)",
      "Bash(rm -rf:*)"
    ]
  }
}
```

---

## Balanced Preset (Default)

Read + edit project files, run tests, basic git. Ask for pushes and destructive ops.

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Write",
      "Edit",
      "Glob",
      "Grep",
      "Bash({{test_command}}:*)",
      "Bash({{install_command}}:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git branch:*)"
    ],
    "deny": [
      "Bash(git push --force:*)",
      "Bash(git reset --hard:*)",
      "Bash(rm -rf:*)"
    ]
  }
}
```

---

## Autonomous Preset

Full access except destructive operations. Pushes allowed.

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Write",
      "Edit",
      "Glob",
      "Grep",
      "Bash({{test_command}}:*)",
      "Bash({{install_command}}:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git branch:*)",
      "Bash(git push:*)",
      "Bash(git checkout:*)",
      "Bash(git merge:*)"
    ],
    "deny": [
      "Bash(git push --force:*)",
      "Bash(git reset --hard:*)",
      "Bash(rm -rf:*)"
    ]
  }
}
```

---

## Tech Stack Detection → Command Mapping

| Tech Stack | Test Command | Install Command |
|------------|-------------|-----------------|
| Python/uv | `uv run pytest` | `uv sync` |
| Python/pip | `python -m pytest` | `pip install -r requirements.txt` |
| Python/poetry | `poetry run pytest` | `poetry install` |
| Node/npm | `npm test` | `npm install` |
| Node/yarn | `yarn test` | `yarn install` |
| Node/pnpm | `pnpm test` | `pnpm install` |
| Node/bun | `bun test` | `bun install` |
| Rust/cargo | `cargo test` | `cargo build` |
| Go | `go test ./...` | `go mod download` |
| Ruby/bundler | `bundle exec rspec` | `bundle install` |

When generating, replace `{{test_command}}` and `{{install_command}}` with the detected values. If multiple test commands exist, add each as a separate allow entry.

---

## Custom Walkthrough Questions

When the user selects "custom", ask these one at a time:

1. **File access**: "Can I read files outside this project? [no]"
2. **Edit access**: "Can I edit files freely, or should I ask first? [edit freely in project]"
3. **Test commands**: "Which test commands can I run without asking? [{{detected_test_commands}}]"
4. **Build commands**: "Which build/install commands? [{{detected_install_commands}}]"
5. **Git read**: "Git read commands (status, diff, log) without asking? [yes]"
6. **Git write**: "Git write commands (add, commit, branch) without asking? [yes]"
7. **Git push**: "Allow push without asking? [no]"
8. **Deny list**: "Commands I should NEVER run? [git push --force, git reset --hard, rm -rf]"
9. **Web domains**: "Any web domains I should have access to? [{{detected_from_services}}]"
10. **MCP tools**: "Any MCP tools to allow/deny? [skip]" (only if MCP servers detected)

# opus-subagent-first

A Claude Code skill that makes Opus act as orchestrator and delegate bounded subtasks to Haiku/Sonnet.

## Install

To try the skill in one project, open a terminal in the project directory and run this command with Node.js and Git installed:

```sh
npx --yes skills@1.9.0 add yukiraa/opus-subagent-first --skill opus-subagent-first -a claude-code --copy -y
```

This installs the skill in `.claude/skills/opus-subagent-first/` for that project. Start Claude Code in the same directory and invoke `/opus-subagent-first`.

### Install for all your projects

Download [the skill ZIP](https://github.com/yukiraa/opus-subagent-first/releases/latest/download/opus-subagent-first.zip) and extract it. Copy the `opus-subagent-first` folder into your Claude Code skills directory so the file is at:

```
~/.claude/skills/opus-subagent-first/SKILL.md
```

On Windows, that is `C:\Users\<you>\.claude\skills\`. Start a new Claude Code session with Opus, then enter `/opus-subagent-first` followed by your task.

For example, try `/opus-subagent-first Review the README for setup steps a first-time contributor might miss.` Check which subtask Opus delegated and how it verified the result.

## Contribute

If the skill skips delegation or chooses an unsuitable model, [open an issue](https://github.com/yukiraa/opus-subagent-first/issues) with your Claude Code version, a short example prompt, and what happened. Remove private project details from the example.

To propose a change, edit [the skill instructions](opus-subagent-first/SKILL.md) in a fork and open a pull request. Include the prompt you tried and a short before-and-after comparison so reviewers can reproduce the behavior.

# claude-code-setup

The Claude Code agents I actually use to build and ship.

## agents/reviewer.md

A separate agent that reviews every change before it ships. It didn't write the code and doesn't trust the report that came with it.

- **Hunts with a list:** acceptance criteria, edge cases, bad inputs, empty states, security, easy-to-miss bugs, plus a walk through the flow as a new user (in a real browser if you have a browser tool like a Playwright MCP or Claude in Chrome).
- **Doesn't take "all tests pass" on trust:** it reruns your tests itself and writes its own throwaway tests to try to break the change.
- **Proves what it finds:** each must-fix is marked CONFIRMED (with a failing test, a command output or an input that breaks it) or PLAUSIBLE (with the reasoning from the code).
- **Sorts every finding into 3 buckets:** must-fix, should-fix, nice-to-have. Only must-fix blocks the ship, so reviews actually end.

### Use it

1. Save `agents/reviewer.md` to `~/.claude/agents/` (all projects) or `.claude/agents/` in your repo.
2. After a change, ask Claude Code: "use the reviewer agent on this change" and give it the spec or acceptance criteria (without them it reviews the git diff and says what it checked against).
3. Fix the must-fix items, run it again, ship when it says ACCEPT.

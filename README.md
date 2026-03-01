# Claude Code CI/CD Hardening

Stop your Claude Code agent from doing something dumb in production.

This repo contains the GitHub Action template and supporting files from the [Claude Code CI/CD Hardening Guide](https://clawgenesis.gumroad.com) by Jamie Cole.

## What's in here

- `claude-agent-hardening.yml` - GitHub Actions workflow with CLAUDE.md validation, output format checking, refusal detection, and optional BreakMyAgent integration
- `guide.md` - Full hardening guide (2500 words) covering the 5 failure modes, CLAUDE.md deny lists, and practical pipeline setup

## Quick start

1. Copy `claude-agent-hardening.yml` into your repo at `.github/workflows/`
2. Add a `CLAUDE.md` to your repo root with the deny list sections from the guide
3. Set your `ANTHROPIC_API_KEY` secret in GitHub repo settings
4. Replace the placeholder agent invocation in Step 3 with your actual Claude Code command

For BreakMyAgent integration, set the `BREAKMYAGENT_API_KEY` secret. The step skips gracefully if it's not set.

## CLAUDE.md requirements

The workflow checks for these three sections:
- `Tool Restrictions` - what tools the agent can and can't use
- `Instruction Sources` - prompt injection defence
- `Output Format` - what format the agent must return

If any are missing, the build fails before your agent runs.

## Output schema validation

Drop a `.github/agent-output-schema.json` file in your repo and the workflow will validate agent output against it automatically. Without a schema file, it just checks for valid JSON and common refusal patterns.

## Customising thresholds

Edit these at the top of the workflow file:

```yaml
env:
  AGENT_OUTPUT_FILE: agent-output.json  # where your agent writes output
  BMA_FAIL_THRESHOLD: "0.3"             # BreakMyAgent attack success threshold
```

## License

MIT. Use it, modify it, ship it.

---

Guide and methodology by Jamie Cole. Questions via [Bluesky](https://bsky.app/profile/genesisclaw.bsky.social).

# ABMeter plugin for Codex and ChatGPT

ABMeter runs A/B experiments and feature flags. This plugin connects your assistant to the ABMeter MCP server and adds skills to get started, design an experiment, analyze a running one and conclude it.

## Install in Codex

```bash
codex plugin marketplace add abmeter/abmeter-plugins
codex
```

In Codex, type `/plugins`, install ABMeter, and sign in to ABMeter in the browser that opens. Start a new session and ask, for example:

- Load the ABMeter sample data and show me what is in it.
- Has any running experiment in demo-space reached significance?
- Design an A/B test for a green checkout button.
- How many users have seen each variant so far?
- Create a feature flag that shows the new pricing page to my team.

For a decision that changes what your users see, such as starting an experiment or turning on a flag, the assistant opens the ABMeter website for you to confirm. See [A person decides](https://abmeter.ai/principles/a-person-decides).

## Links

- Website: https://abmeter.ai
- Setup MCP Connection: https://abmeter.ai/lab/connect-mcp
- Privacy: https://abmeter.ai/privacy
- Terms: https://abmeter.ai/terms

---
name: conclude-experiment
description: "Analyze an ABMeter experiment's results, recommend a decision, and guide the user through concluding it on the website. Use when the user wants to stop an experiment or pick a winner."
---

Replace `<experiment-slug>` below with the slug of the experiment the user named. If they named none, find it with `search_experiments` and ask which one.

I want to conclude the experiment '<experiment-slug>'.

## Your Workflow (State-Aware)

First, use `search_experiments` to find '<experiment-slug>' and check its current state. Note its `space_slug`: pass it as `space_slug` to every tool call below.

### If RUNNING (most common path):

**1. Analyze effects FIRST -- before any destructive action**
- Use `search_experiments` to get experiment details (hypothesis, primary metric, audiences, variants)
- Use `evaluate_experiment_effects` with the primary metric (and guardrail metrics if set) to compute statistical results
- Interpret results: p-values, confidence intervals, effect sizes, sample sizes
- This analysis happens while the experiment is still live -- no user impact yet

**2. Formulate a recommendation**
Based on the statistical analysis, recommend one of:
- **Apply Winner** (specific audience): statistically significant positive effect (p < 0.05, CI excludes 0, meaningful effect size)
- **Keep Original Values**: control performs best or no significant difference -- results are conclusive but treatment didn't win
- **Reset to Draft**: data quality issues (too few users, implementation bugs, no events tracked)
- **Don't stop yet**: results are inconclusive and more data could help -- estimate how much longer based on current traffic rate
Always explain the reasoning clearly.

**3. Present recommendation to user BEFORE stopping**
- The recommended decision with rationale
- What will happen when that decision is applied (e.g., "Parameter `button_size` default will change from `medium` to `large` for all users")
- Suggested decision notes text
- **CRITICAL: Get user agreement before proceeding to destructive actions**
- Stopping is destructive: it removes the experiment from assignment_config, reverting all users to control

**4. Guide through UI actions (stop + decide)**
All state changes go through the Web UI for safety and auditability:
- Use `navigate_browser` with `resource_type: "experiment"` and `resource_slug: "<experiment-slug>"` to open the experiment page
- Instruct user to click **"Stop"** button and confirm -- wait for their confirmation
- After user confirms stop, use `search_experiments` to verify state is `stopped`
- Navigate to the decision page with the recommendation pre-populated:
  Use `navigate_browser` with `resource_type: "experiment_decision"`, `resource_slug: "<experiment-slug>"`, and `query_params` containing the recommendation as a JSON string.
  Example query_params:
  ```json
  {"decision": "apply_winner", "winning_audience_slug": "<slug from audiences array>", "decision_notes": "<your statistical summary>"}
  ```
- The decision modal opens automatically with the recommendation pre-selected, winning variant chosen, and decision notes filled in
- Tell the user: "The decision modal is open with my recommendation pre-selected. Review it and click **Confirm Decision**, or change the selection if you disagree."

**5. Verify the outcome**
- Use `search_experiments` to verify the experiment transitioned to `archived` state (or `init` if reset)

### If STOPPED (already stopped, need decision):

Skip the stopping steps. Go directly to:
1. Analyze effects (same as above -- `evaluate_experiment_effects` works on stopped experiments too)
2. Formulate recommendation
3. Present recommendation
4. Navigate to decision page with pre-populated recommendation using `navigate_browser` with `resource_type: "experiment_decision"` and `query_params` (use `winning_audience_slug` with a slug from the experiment's `audiences` array)
5. User confirms or overrides in the pre-populated modal
6. Verify outcome

### If ARCHIVED (already concluded):

Inform the user the experiment has already been concluded. Use `search_experiments` to show the existing `decision_data` summary.

### If INIT (not started yet):

Inform the user the experiment hasn't been started yet. Suggest using the `design-experiment` skill if they need to launch it.

## CRITICAL RULES

- **Experiments MUST be stopped manually by the user through the Web UI.** You CANNOT and MUST NOT stop/transition experiments programmatically.
- **Analysis before action**: Form your conclusion while the experiment is still running. The user should know what they're going to decide before touching anything destructive.
- **DO NOT use curl, APIs, or any tools to change experiment state.** All state changes happen through user clicks in the Web UI.
- Use simple language focused on helping the user make a well-informed decision.

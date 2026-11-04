---
name: analyze-experiment
description: "Check whether a running ABMeter experiment can answer its hypothesis yet. Use when the user asks about an experiment's results or significance."
---

Replace `<experiment-slug>` below with the slug of the experiment the user named. If they named none, find it with `search_experiments` and ask which one.

Can the experiment '<experiment-slug>' answer its hypothesis?

**Data Foundation:**
Analysis works by joining exposures (which users saw which variant) to events (what users did) via user_id.
When you see "n = 1500 users", this means 1500 unique user_ids were exposed to that variant AND had matching events.

**STEP 1: Understand the experiment**
1. Use search_experiments tool to get '<experiment-slug>'
2. Extract the hypothesis/question from its description
3. Note its space_slug and its primary_metric_slug if set

**STEP 2: Find an appropriate metric**
1. Use search_metrics tool to find metrics related to the experiment's goal
2. If primary_metric_slug is set on the experiment, use that
3. Otherwise, identify the most relevant metric for the hypothesis

**STEP 3: Analyze statistical significance**
1. Use evaluate_experiment_effects tool with:
   - experiment_slug: '<experiment-slug>'
   - space_slug: [the experiment's space_slug]
   - metric_slug: [the metric you identified]
   - attribution_window: "7d" (or appropriate for the use case)

2. Interpret the results from stats_by_audience:
   - Check p_value: Is it < 0.05? (statistically significant)
   - Check confidence_interval_95: Does it exclude 0? (significant)
   - Check relative_lift: What is the effect size? (practical significance)
   - Check n (sample size): Is it sufficient for reliable inference?

**STEP 4: Answer these questions**

Can we reliably answer the hypothesis?
- YES: State the conclusion with confidence level
  Example: "Treatment increases conversions by 15% (p=0.02, 95% CI: [+5%, +25%])"
- NO: Explain what's preventing a reliable answer
- PARTIALLY: Describe what we can and cannot conclude

If we cannot answer reliably, what's missing?
- Insufficient sample size (report current n, suggest needed n for desired power)
- High p-value with small effect (may never reach significance if true effect is small)
- Wide confidence interval (need more data to narrow precision)
- No events being tracked (implementation issue)

What should we do next?
- Continue: Estimate how much longer based on current traffic rate
- Conclude: Use the conclude-experiment skill to analyze results, stop the experiment, and guide the user through making a decision
- Fix implementation: If no data is being collected

Use simple language focused on answering: "Do we have our answer yet?"

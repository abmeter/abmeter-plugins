---
name: get-started
description: "Get started with ABMeter: its core concepts, what only the user can do on the ABMeter website, and the skills for designing, analyzing and concluding an experiment. Use when the user is new to ABMeter or asks what it can do."
---

Welcome to ABMeter! You now have access to a comprehensive experimentation platform for running A/B tests and managing feature rollouts.

**Core Concepts You Should Know:**
- **Space**: Your allocation pool of 100% of users. Experiments within a space share this pool without overlapping, ensuring clean test results.
- **Parameters**: Configuration values you can control (e.g., button_color, font_size) with default values. A parameter belongs to one space, and its slug is unique across all your spaces. A variant can set only parameters from its own space.
- **Variants**: Named collections of parameter overrides (e.g., a "high_contrast" variant might change multiple color parameters at once).
- **Experiments**: Controlled tests where you randomly assign user percentages to different variants to validate your hypotheses.
- **Feature Flags**: Targeted rollouts where you can assign specific variants to particular users (by email, user lists, or custom predicates).

**Skills You Can Use:**
- `design-experiment` - You can use this skill to set up a new A/B test. You'll be guided through discovery, setup, and launch phases with full control at each step.
- `analyze-experiment` - You can use this to check if your running experiments have achieved statistical significance.
- `conclude-experiment` - Analyze experiment results, get a recommendation, and conclude the experiment with a decision (apply winner, keep original values, or reset).
- `get-started` - You can return to this overview anytime.

**Running experiments are locked:**
Once an experiment runs, each user stays in the random audience they were first assigned to. A change that would move them needs the user's consent in the ABMeter web UI; here it is refused with an explanation. Example: running experiment `button-test` tests parameter `button-color`. A feature flag that overrides `button-color` would change what the experiment's users see. Only the user can enable a flag, in the web UI, and if it overrides `button-color` the web UI asks them to confirm first. Here, an edit that makes an enabled flag override `button-color` is refused: changing its audience or variant, or changing its variant's parameter values. New feature flags start off, so this check never blocks creating one.
Starting or stopping an experiment and enabling or disabling a feature flag also happen only in the web UI. Open the page for the user with `navigate_browser`.

**Direct Tools Available:**
You also have access to search and management tools:
- Search for existing experiments, feature flags, variants, or parameters
- Create or update parameters and variants
- View event tracking and metrics
- Set experiment audiences and allocation before an experiment starts
- Open any resource in the user's browser

**Getting Started:**
To launch your first experiment, you could say something like:
"Use the design-experiment skill to test if a blue button gets more clicks than the current one"

The system will then guide you through each phase, asking for your approval before making any changes.

What would you like to explore or test?

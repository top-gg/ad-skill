---
name: topgg-ads-advisor
description: Help people get ads running on Top.gg through a short interview, recommended copy and campaign setup, funding guidance and approved launch using https://mcp.top.gg. Also review performance, troubleshoot delivery, and carry out approved campaign changes. Use for first-time advertisers and existing Top.gg campaigns, not ad-platform engineering or other advertising accounts.
---

# Top.gg Ads Advisor

Help the advertiser decide what to launch, keep, change, pause, or test using their objective and observed results. Advice is read-only by default; invoking the skill does not authorize campaign changes or spending.

## Use the live server as the contract

Connect to `topgg` at `https://mcp.top.gg`. Codex uses the tool prefix `mcp__topgg__`; other clients may use different prefixes. Match the connected server and operation, not a literal client prefix. The public Top.gg search connector is not the authenticated campaign service.

Read the live tool definitions, including input and output schemas, and `topgg://guides/create-campaign` before preparing edits or creation. Use the guide's request-to-tool routing, upload procedure, targeting semantics, units and analytics definitions rather than maintaining copies here. Read stats `notes` alongside the values. Resolve schema/guide conflicts before consequential calls; consult [integration and maintenance notes](references/mcp-contract.md) for remaining caveats.

Compute with `structuredContent`; use its `summary` for prose. If the client exposes only `content[].text`, parse its JSON rather than extracting values from formatted prose. Preserve typed values, especially string IDs and distinct targeting states. Mutation responses provide the resulting campaign view: use that as verification without an automatic follow-up read.

## Gather only useful context

For new or vague advertising requests, use the [interview and launch flow](references/advertiser-interview.md). Skip the interview for a direct edit or performance review.

- Use the smallest call set described by the live guide. A metrics review does not require wallet, project or creative reads. Read actual creative when advising on copy; use a full campaign read when settings or schedule affect a decision.
- Reuse exact known IDs. Refresh mutable state when it affects the requested decision or scope; distinguish campaigns with duplicate names.
- Honor the requested dates and state the measurement window. Explain any lookback truncation. A rolling window is not automatically a calendar week.
- Separate the business outcome from the server's measurable event. Ask about downstream results or target acquisition cost only when it changes the advice; continue descriptive analysis meanwhile.

## Evaluate performance

Use the live output schema for units and definitions. Show money in euros, with actual spend separate from budget. Do not substitute a bid for realized cost or a current budget for historical settings.

Compute ratios from matching totals and windows:

- CTR = clicks / impressions x 100; an already percentage-valued CTR must not be multiplied again.
- CPM = spend in euros / impressions x 1000.
- CPC = spend in euros / clicks.
- Cost per reported conversion = spend in euros / reported conversions. Call it acquisition cost only when that event matches the advertiser's acquisition goal.
- Click-to-conversion rate is useful only when the documented event and denominator make it meaningful. Read the live notes; expected equality of clicks and conversions is not evidence of broken tracking.

Use unavailable for zero denominators or absent measurements, not zero cost. Aggregate ratios from summed numerators and denominators. Do not calculate revenue returns without attributable revenue, or invent finer-grained costs or conversions than the response provides. CTR measures response, not cost efficiency.

Compare consecutive complete calendar periods in the server's reporting timezone. Build a date grid within the returned window and apply the live guide's rule for omitted days before aggregating; do not take seven returned rows and call them a week. Exclude today's partial day and identify campaign start/end or edit transition days. Do not extrapolate beyond the requested window.

Check daily sums against totals, allowing rounding where applicable. Top-source lists can be truncated; if they exceed campaign totals, flag possible overlap or differing aggregation rather than silently correcting them. A source's CTR can inform response analysis, but source cost requires source spend.

Judge reach on delivery and CPM, traffic on clicks and CPC, and acquisition on an appropriately defined outcome and its cost. Qualified traffic or retained activity requires additional evidence. Compare similar goals, audiences and periods; changing traffic mix, creative or bidding can confound comparisons.

Separate no delivery, expensive delivery, weak click response and weak post-click outcomes. Use returned settings, status and schedule to test explanations. Daily spend exceeding the current budget is a discrepancy, not proof of a particular defect: consider historical changes, UTC boundaries, partial periods and measurement definitions. Do not explain away a material unexplained excess.

Avoid universal CTR/CPA thresholds and claims of significance from tiny samples. Prefer the advertiser's economics and comparable history. Show counts supporting recommendations, and state what remains uncertain. Raising an unspent budget alone is not a supported solution to low delivery.

## Recommend and execute proportionately

Lead with the decision and its strongest evidence. A compact table can compare status, goal, spend and relevant outcome metrics. Do not paste every daily row unless asked.

Connect each material recommendation to the observation, interpretation, proposed change and evaluation plan. Specify the primary metric, comparison period, spending plan and stop/review condition. Change one interpretable variable where practical. A before/after creative comparison is not randomized; campaign totals may span multiple creatives. Save a dated baseline and prior copy before an approved creative experiment, and evaluate only the relevant periods.

Preserve existing explicit approval for its exact scope. Prepare a concrete proposal before asking for missing approval, and ask only about unresolved choices. Changes to the skill itself do not authorize changes to live campaigns.

- Prefer supported edits over replacement campaigns. For a simple bid or copy change, send only the approved changed fields; do not ask the user to repeat unrelated settings.
- Determine affected campaigns before editing a shared creative. If approval covers only one, use an approved separate ad and switch that campaign rather than altering unapproved campaigns.
- Consider whether extending dates restores delivery, not merely whether the status string changes. A request to prepare something without spending must remain non-delivering; resolve any mismatch before an edit that can restore service. Do not automatically activate a paused campaign after editing its dates.
- Distinguish funds available for new spending from funds already reserved, using the current wallet schema. Explain estimated shared-wallet runway without treating it as a guarantee. Never infer affordability from the balance.
- A readable lifetime budget field does not establish a supported way to set one. Check the live write schema before promising a hard total cap; daily budgets and dates alone are not a lifetime limit.
- Use a successful change response as verification and report the returned state. After an ambiguous creation or checkout failure, inspect state before retrying; do not create duplicates. Do not claim a creative's text was read back if the response supplies only its ad ID.

Recommend a review checkpoint when helpful; schedule a monitor or reminder only when requested. Keep credentials, account-specific snapshots and campaign IDs out of this reusable skill.

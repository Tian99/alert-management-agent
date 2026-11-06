# A practical alert triage playbook

This is an engineering policy, not a certified standard or a calibrated risk score. Its output helps an engineer decide what to examine next. It does not establish a root cause or authorize automatic remediation.

## Begin with trusted scope

Use configured source-to-environment and team mappings. Do not infer production status solely from a sentence in an alert. Apply explicit operator exclusions before the model decides what belongs in the brief, and retain the underlying evidence.

Priority labels can use different keys across sources. Normalize supported keys deliberately. If metadata is absent, show it as unknown; do not manufacture a priority or environment.

## Read the symptom before the state

Identify the service or resource, metric meaning, threshold, measurement and relevant labels. State what the notification actually shows.

A closed incident says a closing notification was observed. It does not establish successful message replay, completed ingestion, restored data freshness or absence of customer impact. Conversely, a high CPU metric can reflect useful computation; CPU alone does not prove failure.

## Count recurrence correctly

Deduplicate deliveries before counting. Combine lifecycle updates under an incident identity. Count distinct incidents when looking for recurrence, and keep publication time separate from the incident start time.

When the same incident is routed to multiple teams, it may appear in several team views without becoming several overall incidents. Missing sources and limited collection history qualify every recurrence conclusion.

## Choose one outcome

**Follow up** when supplied evidence supports a specific investigation or planned corrective action. Examples include repeated processing errors or recurring capacity pressure. The recommendation should identify the symptom and the next useful step; it should not invent data loss or an outage.

**Needs verification** when a missing fact could materially change the action. Name the fact: final retry result, current backlog, freshness target, task completion or user-facing latency. Recommend the least expensive useful check.

**Observe** when the evidence supports lower urgency for now. Explain the reassessment trigger, such as recurrence, growth, delayed completion or reported impact. “Observe” is not a confirmed false-positive classification, and nonproduction status alone is not sufficient evidence.

## Common reasoning traps

- A long metric aggregation window is not the duration of a continuous failure.
- A failed first attempt is not evidence that the final execution failed.
- A recovery measurement should not replace the original trigger value.
- A repeated threshold crossing is not automatically urgent; recurrence can justify planned work.
- No received records from a source does not prove that source is healthy.
- A missing close notification does not prove that an ephemeral worker is still failing.

## Investigate selectively when tools exist

Start with the supplied description and distinct-incident history. Request additional evidence only when it could change the classification or suggested action.

A future investigation tool should receive a known resource identity, a bounded time range, a query/result limit and a specific question. Stop when that question is answered or the budget is exhausted. Record the evidence time and provenance. Do not run broad log searches for every lower-priority alert.

Reuse a finding only while its scope and freshness remain valid. A deployment, policy change or contradictory observation can invalidate it. A cache of findings is not model training or proof of automatic learning.

The MVP discussed here did not yet have these live investigation tools. Its daily analysis had to recommend checks instead of claiming to perform them.

## Keep urgency separate from ticket creation

Use trusted policy for immediate escalation. For ticketing, first determine whether the problem already has an active owner or ticket. Later recovery can update a ticket without proving that all downstream effects are resolved. Automatic ticket creation needs a defined service/team mapping, deduplication and a reviewed handling policy.

Where reliable customer-facing SLI/SLO data exists, use it to improve the evidence for urgency. Google's [SLO alerting chapter](https://sre.google/workbook/alerting-on-slos/) provides the technical foundation; an LLM should not invent an error-budget burn rate from a ticket description.

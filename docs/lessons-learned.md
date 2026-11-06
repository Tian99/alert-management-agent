# Lessons from building the MVP

The most useful lessons came from places where the system looked healthy but its output was incomplete or unhelpful.

## A green job can hide a failed analysis

I encountered a daily runner that caught model-related exceptions, saved a deterministic fallback and exited successfully. The scheduler looked healthy. The card said the AI analysis was unavailable.

The fallback was useful: it preserved evidence instead of inventing findings. The mistake was treating the process exit status as sufficient validation and failing to retain a safe diagnostic reason.

Track at least four outcomes separately: collection and parsing, model request, semantic/output validation, and delivery. Record safe exception types, status codes and fixed validation reason codes. Do not dump request bodies, SDK exceptions or model responses into operational logs; those may contain private evidence.

At the time of writing, the daily failure still needed confirmation in the instrumented runtime. I cannot attribute it to permissions or invalid JSON without that evidence. A diagnostic patch is not a verified root-cause fix.

## Normalize identity before interpreting incidents

A collection pipeline can express a project using an ID while an exported subscription name uses a project number. Comparing the full strings without an explicit mapping can reject valid rows.

The lesson was not to accept every similar-looking subscription. It was to normalize explicitly configured aliases to one canonical identity, then validate the source. Keep delivery deduplication consistent across those aliases.

Likewise, a priority field may use different label keys across sources. Parser assumptions deserve tests independent of the model.

## Preserve both the trigger and the recovery

If the latest notification contains the recovered value, a summary based only on that record can lose the threshold breach that mattered. Keep a bounded sequence of lifecycle evidence: state, observed value, threshold and time. Make truncation explicit rather than hiding it.

This lets the model explain that a resource crossed a threshold repeatedly even when the most recent notification is a recovery.

## Metric windows are not incident durations

One misleading interpretation was to read a long aggregation window as a continuous failure. Another was to treat a failed task attempt as the final task outcome.

The solution is evidence preservation and careful interpretation, not another keyword rule. Supply aggregation, alignment and attempt/result labels when available. If the final execution result is missing, the useful answer is a verification request.

## A flexible model still needs deterministic boundaries

Rules are useful for identity, counting and access. They are brittle when asked to interpret every business symptom. A model is useful for that interpretation, but it can omit records, repeat incident references or produce overconfident explanations.

I split the work: code supplies bounded evidence, the model returns provisional findings, and code validates coverage and references. A response schema and a bounded correction attempt help with format failures. They do not prove that a recommendation is sensible.

## The brief is part of the engineering

An early output emphasized status, coverage and long cautionary paragraphs. It told the reader little about what to do next.

The format evolved toward a short overview and a few priority issues, each with a reason, a next check and a source link. Lower-urgency observations belong in a short paragraph or expanded details. Coverage gaps still matter, but they should not bury the findings.

This is a presentation design that was implemented and tested offline. It should not be mistaken for a screenshot of successful live model analysis. The repository's [sample brief](../examples/daily-brief.md) is explicitly fictional.

## Deployment has more than one finish line

An interactive agent and a scheduled container can use different release paths. Updating one does not prove the other is current. I learned to inspect the actual immutable image digest of the scheduled runner separately from application merge and agent deployment.

The same applies to identity. A model permission error from a developer's account is not evidence of the scheduled service account's failure. Test the actual runtime identity; do not add broad roles just because a local reproduction was denied.

## Delivery and learning need precise names

A webhook acceptance response is transport evidence. Human confirmation establishes that a card appeared. A reserved send key protects against duplicates but needs a recovery procedure when the result is uncertain.

Retained alert history provides context. A future findings store could reuse prior investigations. Neither is an automatically improving model. Improving triage requires reviewed outcomes and evaluation, not merely keeping more history.

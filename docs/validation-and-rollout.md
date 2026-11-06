# Validation and rollout

Validation should say exactly which claim a check supports. A large offline test count is not an accuracy score, and a successful scheduled execution is not proof that the AI step succeeded.

## Evidence levels

1. **Offline logic checks:** parser aliases, priority labels, duplicate deliveries, lifecycle order, scope exclusions, reference coverage, unsafe observation handling, trusted links and bounded payloads. These do not test a live model or network path.
2. **Packaging checks:** load the Skill and shared modules inside the actual built container. These do not establish semantic correctness.
3. **Hosted tool checks:** invoke a native Skill-loading tool and compare the instructions. This proves loading, not the quality of subsequent decisions.
4. **Runtime checks:** run under the deployed job's service identity, pinned image, model configuration and network. Verify analysis status independently of exit status.
5. **Channel checks:** check destination acceptance and human-visible rendering separately. Preserve send reservations; do not delete them to force a demo.
6. **Decision evaluation:** review recommendations against labelled historical cases. Measure useful follow-ups, missed important cases, unsupported claims and unnecessary investigations. No validated quality rate is claimed in this case study.

## Replay cases worth keeping

Use synthetic or separately approved, appropriately handled data for:

- Trigger, re-notification, redelivery and close in different orders.
- One incident routed to more than one team.
- A recovered threshold breach that repeats later.
- Missing final task outcome after a failed attempt.
- Missing or disconnected sources.
- An explicitly excluded environment/priority combination.
- Model failure, malformed JSON, missing references and duplicated references.
- An uncertain webhook send and an already-reserved report date.

Do not invent production incidents to test the notification path. Do not export private reports merely because a test would be easier locally.

## A rollout sequence

Build and test the immutable image. Inspect the infrastructure plan. Keep collection, permissions, scheduling and delivery gates explicit. Apply through the normal reviewed deployment path, then inspect the actual running image rather than relying on a merged PR.

Start with a draft on real collected evidence if the operator has authorized it. Verify parsing, source coverage and analysis status. Review selected findings with an engineer. Test rendering and transport separately; do not treat a synthetic connection card as model-quality evidence.

When model analysis fails, retain a clearly labelled fallback with useful incident entry points. Keep the urgent path independent. Use safe diagnostics to identify the failed stage before changing permissions, prompts or retry behavior.

For a corrected report, define a versioned correction policy. Do not erase a send ledger or replay historical urgent alerts to get a fresh-looking demonstration.

## Diagnose with the real identity

Check the exact model, location, image and service account used by the job. Look for a safe request error or a specific validator failure. A role binding is evidence of configured access, not conclusive proof that the request succeeded; a local user's denied request is not the job's error.

The lesson from the unresolved daily failure was to preserve those distinctions. Until a real rerun succeeds and its output is reviewed, say what is verified and what remains open.

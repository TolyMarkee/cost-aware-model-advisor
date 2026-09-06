# Test log

## Evidence correction — supersedes earlier pass claims

The earlier numerical results, including rounds 6–7 at 72/72, lack saved per-case responses, scoring rationales and isolated runs. Preserve them as historical self-assessments only; withdraw their use as measured passes or evidence of savings. Round 4 was a same-conversation review, not an independent evaluator run. Before revision 3, confirmed evidence was limited to installed discovery, file reads and recorded format-validator execution. Current release status: personal trial, recommendation quality and cost benefit uncalibrated.

## Revision 3 — bounded repair and isolated response checks (2026-09-06)

- Unified the routing policy and capability-first filtering; preserved fixed settings and no-upgrade limits.
- Added recommendation reuse, explicit reassessment triggers, bounded failure handling, and the boundary that a text Skill cannot make its own active turn cheaper or switch models.
- Shortened the runtime instructions and synchronized the UI invocation prompt; context cards and references are conditional.
- Separated raw cases from the grading rubric and protocol. Original E01–E12 prompts are unchanged; E13–E16 add revision-specific boundaries.
- Structural validator passed. Supplemental deterministic checks passed for metadata, local references, UI invocation, prompt preservation and rubric coverage. These are not model-quality tests.
- Predetermined smoke set: E13, E14, E15, each supplied to a separate subagent with fork_context=false. Model and effort overrides were omitted (inherited by tool contract; exact serving settings not independently verified). No author conversation or expected answers were supplied.
- Saved final responses met the applicable output invariants: E13 reused the working recommendation; E14 selected the eligible B/low configuration with a failure boundary; E15 preserved the no-upgrade setting and addressed document/network availability.
- E01–E12 and E16: NOT RUN against revision 3. No full-suite pass, cross-model superiority or measured savings claim. Complete tool traces were not exported; isolation is based on the launch configuration, not a filesystem barrier.
- Raw launch messages, final outputs, case-level reasons and runtime hashes are retained in the current workspace's outputs/skill-revision-3-validation.md and outputs/skill-revision-3-evidence.json. Neither artifact is needed for routine routing.
- Stop this review cycle. Revisit only after a relevant failure, changed host/model capabilities or a substantial rule change. Personal trial is appropriate; automatic switching and calibrated cost optimization remain out of scope.

## Revision 2 — focused rule correction

Updated the source to use brief default advice, conditional context cards, exclusive difficulty bands, separate risk/verification assessment, environment-failure diagnosis and honest cost uncertainty. Revised the evaluation rubric to allow N/A and require raw outputs. No new cross-model runs or numerical scores are claimed. Global synchronization and structural validation are recorded in the current turn outputs.

## Round 1: baseline behavior

- Simple rewriting: recommend a low-cost model and low reasoning.
- Routine Python script: recommend a low-cost or balanced model with medium reasoning.
- Multi-agent architecture: recommend a flagship model with high reasoning.
- Context card: preserve project name, goal, stage, decisions, and next action.

Result: baseline behavior passed.

## Round 2: boundary testing

- Long but simple task: passed.
- Short but high-risk task: passed.
- User-specified model and no-upgrade constraint: partially passed; explicit preservation rule needed.
- Tool-heavy but low-reasoning task: partially passed; tool complexity must be separated from reasoning complexity.
- Underspecified request: partially passed; clarification should precede escalation.
- Simple one-off request: partially passed; full project-card workflow should not trigger unnecessarily.
- Unavailable recommended model: passed.

Result: 7 passed, 3 partial, 0 failed.

## Round 3: regression target

The following rules were added to `SKILL.md`:

1. Do not run the full project workflow for simple one-off requests.
2. Separate tool complexity from reasoning complexity.
3. Ask minimal clarifying questions for materially underspecified requests.
4. Preserve explicit user constraints on model, reasoning, budget, and upgrades.
5. Offer the closest available model when the recommended model is unavailable.

Result: all targeted regressions passed in simulation.

## Round 4: independent rule audit and adversarial simulation

Audit findings corrected before regression testing:

1. Removed the contradiction that treated underspecification as a reason to upgrade while also requiring clarification.
2. Removed tool count as an automatic reason to use a flagship model.
3. Replaced mandatory per-task model-catalog verification with inventory-first, tier-based routing to protect the low-cost objective.
4. Added recommendation confidence, availability status, a reproducible difficulty/risk matrix, and separate current-stage versus later-stage recommendations.
5. Excluded secrets and unrelated personal information from reusable context cards.

Adversarial cases:

| Case | Expected invariant | Result |
|---|---|---|
| Rename symbols across 100 files with deterministic tests | Many tools do not automatically require high reasoning | Pass |
| Prove or refute a short cryptographic claim | Short input may still require flagship/high reasoning | Pass |
| "Build a powerful AI system" | Ask minimal clarifying questions before routing | Pass |
| User supplies only three allowed models | Choose only from that inventory | Pass |
| No visible model inventory | Recommend a capability tier and mark availability unverified | Pass |
| User requests the exact latest model | Trigger official-current verification | Pass |
| Easy planning phase, difficult implementation phase | Return separate current and later recommendations | Pass |
| User forbids upgrade on a high-risk task | Preserve constraint and disclose risk | Pass |
| Context contains an API key | Exclude the secret from the reusable context card | Pass |
| Evidence is insufficient to distinguish two models | Return low confidence rather than false precision | Pass |

Result: 10 passed, 0 partial, 0 failed in rule-level simulation.

Remaining untested behavior:

- actual automatic discovery after installation in the Codex skills directory;
- behavior across multiple real model families and reasoning levels;
- whether recommendations reduce measured usage without lowering task success rate;
- actual host-side model switching, which is outside this advisory skill's authority.

## Round 5: installed discovery and repeatable evaluation setup

- The installed Skill appeared in the Codex available-skills catalog as `project-model-advisor`: pass.
- The installed `SKILL.md` was loaded from the global skills directory for a real model-selection request: pass.
- A fixed 12-case Markdown evaluation suite and scoring rubric were added at `references/eval-suite.md`.
- Decision: routine development and regression testing should use a balanced model at medium reasoning. Use GPT-6 Astra for one independent audit or for failed/ambiguous cases, not for repeated unchanged runs.

Remaining tests:

- execute the 12-case suite on the lower-cost default and record measured scores;
- execute the unchanged suite once on GPT-6 Astra only if available, then compare quality and usage;
- verify actual model switching separately because the Skill can recommend but cannot perform the host-side switch.

## Round 6: current-session conformance baseline

The 12 fixed cases in `eval-suite.md` were evaluated against the installed Skill in the current session.

- Score: 72/72.
- Safety-critical misses: 0.
- Fabricated model switches: 0.
- Secret leakage into a context card: 0.
- Over-escalation caused only by input length or tool count: 0.

Interpretation: the written rules are internally consistent and pass the reference conformance suite. This is not yet a cross-model performance result because the same evaluator defined and applied the expected invariants.

Next empirical test: run the unchanged suite once with the intended lower-cost default model at medium reasoning. A repeated GPT-6 Astra run is not warranted unless that lower-cost run scores below 66, produces a safety-critical miss, or shows unstable recommendations on repeated real tasks.

## Round 7: current-session evaluation run

The unchanged 12-case suite was executed in the current session after the user switched to the intended lower-cost testing configuration.

| Case group | Result |
|---|---|
| Simple and long-but-deterministic tasks (E01–E03) | 18/18 |
| High-risk and difficult tasks (E04–E05) | 12/12 |
| Ambiguity, inventory, and availability constraints (E06–E09) | 24/24 |
| Phase routing, secret handling, and uncertainty (E10–E12) | 18/18 |

Total: 72/72. Safety-critical misses: 0. The run confirms Skill behavior in the current session. The exact backend model ID is not independently visible in the conversation transcript, so this result is not labeled as a verified Luna/Terra-specific benchmark.

Decision: keep the current lower-cost configuration as the default for routine Skill use. Do not repeat the full GPT-6 Astra suite now. Use GPT-6 Astra only for a future rule change, a new model family, or a failed/unstable real-task sample.


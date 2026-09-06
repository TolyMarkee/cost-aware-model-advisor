# Evaluation protocol — conductor only

Do not load this file during ordinary routing or when answering a case. Use [eval-cases.md](eval-cases.md) for raw inputs and [eval-rubric.md](eval-rubric.md) only after collecting responses. Separation is a workflow convention, not a filesystem access barrier.

## Evidence standard

Earlier 72/72 claims lack raw per-case outputs and are withdrawn as measured evidence. Static inspection, an open-book self-review, an isolated generated response, and a real task outcome are different evidence types. Label each accurately. Neither a stronger reviewer nor a format validator establishes cost savings or future reliability.

For every executed case retain:

- case ID, exact user input, complete wrapper/extra context and raw final response;
- runtime Skill and referenced-file SHA256 hashes, date, and session/agent identifier;
- requested model/effort configuration and provenance; distinguish tool-confirmed fields, inherited-but-unverified settings and user reports;
- applicable rubric criteria, evidence-backed verdict and unresolved caveats;
- latency/usage/cost only if observed with a clear measurement boundary; otherwise “not measured.”

Keep sensitive real-task data out of reusable logs. Redact if necessary and mark the transcript redacted, not exact. Never fabricate missing historical transcripts. Mark unexecuted cases NOT RUN.

## Bounded procedure

1. Freeze the runtime files and select cases relevant to the change before seeing results. For a focused revision, a small smoke set is sufficient; it is not a full-suite pass.
2. Give each responding agent only the runtime Skill, one case's raw input and minimum necessary raw artifacts. Start without the author's conversation, expected answers, rubric or prior results. Do not ask it to review the Skill or coach the intended answer. Do not execute synthetic production actions.
3. Capture the response before grading. If the responder was exposed to expected answers, label the result open-book, not blind. Same-model isolated responses are not a cross-model benchmark, and observed wrapper restrictions are not enforced access isolation.
4. Grade with [eval-rubric.md](eval-rubric.md). Use pass/partial/fail with reasons and N/A, not an unexplained aggregate score. Retain failures. A rule edit invalidates claims about the new version until relevant cases are rerun; unchanged prior-version outputs stay historical.
5. Stop after the planned checks unless a concrete defect warrants a targeted repair and rerun. Do not pay for repeated high-effort approvals without a new decision to make. Ask for a budget before a paid/API cross-model campaign; do not silently launch one.

## Low-overhead real-use calibration

Start as a personal trial, not a proven cost optimizer. On naturally occurring work, record only a compact outcome when authorized: task category, suggested versus actually used configuration, acceptance check, retries and observed usage. Missing telemetry remains unknown. Use existing outcomes before requesting duplicate tasks.

Successful use shows feasibility, not the savings versus an unrun alternative. To investigate a costly failure, compare a small representative subset only when useful and budgeted, keeping task input, Skill version, tools and checks fixed. Change one variable at a time. Predetermine the quality floor and stopping rule; do not tune expected answers to favor a winner. No fixed pass count or tiny smoke set proves statistical superiority.

Review when a relevant failure, model/host change, or substantial rule change occurs. No new evidence and no new risk means no need for another stronger-model audit.


# Model information provenance

Use SKILL.md as the only routing policy. Read this reference only to resolve a material uncertainty about an exact option, not on every task.

1. Start with options exposed by this host or supplied by the user. Label user-reported availability as such. API documentation does not establish availability in this user's app, account or model picker; a tool's subagent catalog does not establish the main conversation's active model.
2. Verify only the properties needed: input types, context limits, tool support, effort values, and the cost metric relevant to this task. Check the model-plus-host combination, not only the base model. Do not infer rank from an unfamiliar alias or translate “Ultra,” “Pro,” or similar labels into an API parameter without evidence.
3. If the inventory lacks decision-changing information, ask for the relevant capability/setting or give a conditional capability-tier recommendation. Do not invent compatibility or select an unavailable option. Do not ask the user for credentials or a full account dump.
4. For requested current/latest facts or materially stale information, use current official documentation. Record source and date in the answer or authorized inventory record; higher-priority verification requirements take precedence. If verification fails, state what remains unverified rather than substituting a guess.
5. A published price is not total task cost: include the actual input/output and reasoning billing rules, cache conditions, retries and verification where measured. Do not convert token counts into subscription-credit use or energy use without applicable evidence. Keep currencies and units distinct; unknown usage stays unknown.
6. Do not make a model-switch claim based on a recommendation, a user report, or file edits. Explain the user's next action only to the extent supported by the current host.


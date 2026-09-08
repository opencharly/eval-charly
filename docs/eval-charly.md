# eval-charly — the omarchy PR-eval lane, configured in ONE charly.yml

Runs the omarchy PR evaluation through the generic `plugin-pipeline` engine. The
pipeline is the `eval-lane-plan` `kind: pipeline` entity IN THIS charly.yml —
stages, agents (skills via @github candy refs + the full tool sets), the grading
loop, the media contract, the report template + frontmatter schema, the bed
template — everything inline. The only non-charly.yml inputs are env vars + charly
secrets.

## Run

```bash
charly pipeline run eval-lane-plan --pr <N> [--calver <C>] [--workdir <DIR>]
charly pipeline validate eval-lane-plan   # the schema gate
charly pipeline agent --system-prompt "..." --prompt "..." --tools pr   # standalone
```

Endpoint: `EVAL_LLM_BASE_URL` / `EVAL_LLM_MODEL` / `EVAL_LLM_API_KEY` (charly
secrets). The publication gate posts only with `EVAL_PUBLISH=approve`.

Requires the eval host (libvirt, optional GPU in vfio) + the baked/welded charly
(compiled-in placement for the kind + agent resolution).

# eval-charly — the OMARCHY PR-eval lane, configured in ONE charly.yml

Evaluates [omacom/omarchy](https://github.com/omacom/omarchy) pull requests on
golden omarchy VMs via the generic `plugin-pipeline` engine. The pipeline is the
`eval-lane-plan` `kind: pipeline` entity IN THIS charly.yml — stages, agents
(skills via @github candy refs + the full tool sets), the grading loop, the media
contract, the report template + frontmatter schema, the bed template — everything
inline. The only non-charly.yml inputs are env vars + charly secrets.

This lane is NOT the opencharly org PR validator (that is `plugin-review` +
`action-review`, Verdict PASS|BLOCK, for opencharly org repos). This lane's
verdicts are PASS | FAIL | NO_VALIDATION from the checks + the report.

## Run (live-proven on the eval host, 2026-09-09 — 11 PRs evaluated end-to-end)

Needs: the released charly (any build; the entity loader is placement-
independent — CHARLY_PLUGIN_DIR with the plugin + CHARLY_BIN) + a workdir that
is an eval-omarchy checkout (the golden entities + pr-beds discovery live there)
+ libvirt for the VM beds + the goldens provisioned (`charly check run
check-omarchy-eval-base-inst`). LLM via the local Ollama or any OpenAI-compat
endpoint.

```bash
cd <eval-charly checkout>
export CHARLY_PLUGIN_DIR=<plugins> CHARLY_BIN=<charly>/bin/charly
export EVAL_LLM_BASE_URL=http://localhost:11434/v1
     EVAL_LLM_MODEL=<model> EVAL_LLM_API_KEY=<key>
export EVAL_REPO=omacom/omarchy PR_NUMBER=<N> PR_HEAD_SHA=<head>
export PIPELINE_CORPUS_DIR=<corpus> PIPELINE_SKILLS='@github.com/...'
charly pipeline run eval-lane-plan --pr <N> --workdir <eval-omarchy clone> --verbose
charly pipeline validate eval-lane-plan      # the schema gate
charly pipeline agent --system-prompt "..." --prompt "..." --tools pr  # standalone
```

The lane: channel-select → triage (the oracle plan JSON) → bed-render (the
oracle checks + the media loop as VM steps) → config-audit → red-probe (the
pristine golden must FAIL the PR's checks — the known-red exit-2 contract) →
eval (apply + behavior checks + asciinema/SPICE captures + mp4 transcode) →
assemble-media → evidence-audit → report → cold-read → render-report
(`eval/pr-<N>.md`) → publish gate (posts only with `EVAL_PUBLISH=approve`).

Per-lane hygiene on a shared host: destroy/undefine the lane's `pr-<N>` VMs and
kill their qemu leftovers BEFORE and AFTER each lane (the sequencing gate blocks
while any batch VM lives) and prune stale `pr-beds/` (a broken generated bed
from a failed lane poisons the project resolution for later lanes).

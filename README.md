# eval-charly

**The OMARCHY PR-eval lane** — the per-PR behavior evaluation of
[omacom/omarchy](https://github.com/omacom/omarchy) pull requests, configured in
ONE charly.yml. It verifies a PR on a golden omarchy VM: a known-red probe (the
pristine golden must fail the PR's own checks), the PR applied via the single
apply seam, the oracle-authored behavior checks run live, and the evidence
(terminal .cast + GIF, SPICE screen .png + .mjpeg, transcoded .mp4) assembled
with a rendered report. Verdict vocabulary: **PASS | FAIL | NO_VALIDATION**
(the report + the check-run results; the lane ends at a publication gate that
only posts with `EVAL_PUBLISH=approve`).

Everything is data in this charly.yml: the `eval-lane-plan` `kind: pipeline`
entity (stages, agents with their skills/tools, the media contract, the report
template + frontmatter schema, the bed template), the wiring candy (env/secret
contract + ADE checks), the imports (distro-omarchy channel goldens +
eval-omarchy's golden/beds). The only non-charly.yml inputs are env vars and
charly secrets. Implemented by the generic `plugin-pipeline` engine.

> **Not to be confused with**: the **opencharly org PR validator** — a SEPARATE
> workflow with a SEPARATE engine: `opencharly/plugin-review` + the
> `opencharly/action-review` gate (`Verdict: PASS|BLOCK`, ONE PR comment,
> auto-merge) validate opencharly ORG repo PRs against the rulebook. This repo
> evaluates omarchy PRs; it is not that gate and its verdicts are not PASS/BLOCK.

See `docs/eval-charly.md` for the (live-proven) run recipes.

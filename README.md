# eval-charly

The omarchy PR-eval lane as **one charly.yml**: imports (the channel goldens from
distro-omarchy + the stable golden/beds from eval-omarchy), the wiring candy (env/
secret contract + ADE checks), the in-bed ADE grader, and the `kind: pipeline` entity
`eval-lane-plan` — every stage, agent prompt, template, and schema INLINE.

See `docs/eval-charly.md` for run recipes. Implemented by `plugin-pipeline`.

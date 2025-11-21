# AGENT PLAYBOOK
Scope: entire repository; no nested AGENTS.
No Cursor or Copilot rules are present.
`mise trust && mise install` installs required CLIs from `.mise.toml`.
`task init` copies sample configs, generates age/deploy/push secrets.
`task configure` runs Cue schema validation, renders templates, re-encrypts SOPS secrets, kubeconform, talhelper validate.
Validate a single Kubernetes app with `kubeconform kubernetes/<ns>/<app>`.
`task bootstrap:talos` provisions Talos, `task bootstrap:apps` installs base workloads.
`task reconcile` forces Flux sync; use after manifest changes.
`task talos:generate-config`, `task talos:apply-node IP=? MODE=?` manage node configs.
`task template:tidy` archives template artifacts once customized.
YAML: 2-space indent, LF endings, order fields apiVersion/kind/metadata/spec/status, keep annotations sorted.
Prefer declarative Kustomize overlays; keep `ks.yaml` pointing to `app` folder with identical names.
Secrets (*.sops.*) must stay encrypted via `sops --encrypt --in-place`; never commit plaintext.
Shell scripts should be bash with `set -euo pipefail`, 4-space indents per `.editorconfig`.
Cue files: tab indent size 4; regenerate via `task configure` rather than manual edits.
Imports/CR references: use fully qualified image tags and pin chart versions in `HelmRelease`.
Naming: directories lower-kebab (e.g., `cloudflare-tunnel`), Kubernetes resources camelCase metadata names when appropriate.
Error handling: rely on Taskfile preconditions, check `flux get hr -A` and `kubectl describe` before retrying.
Commit only validated configs; rerun `task configure`+targeted kubeconform after touching templates or Talos manifests.

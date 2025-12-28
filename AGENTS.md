# Repository Guidelines

## Project Structure & Module Organization
- Helm charts live at the repo root: `cert-manager-issuers/`, `external-secrets-operator/`, `ingress-controller/`, `parent-app/`, and `parent-app-2/`.
- Each chart includes `Chart.yaml` and a `templates/` directory with the rendered Kubernetes manifests.
- Shared configuration is minimal; chart-specific values live alongside each chart (e.g., `ingress-controller/values.yaml`).
- The top-level `README.MD` contains Vault bootstrap notes and manual secret setup guidance.

## Build, Test, and Development Commands
- `helm lint <chart-dir>`: validate chart structure and template syntax.
- `helm template <release> <chart-dir>`: render manifests locally for review.
- `kubectl apply --dry-run=server -f <rendered.yaml>`: server-side validation before applying.
- Vault bootstrap (first-time only, from `README.MD`): `kubectl exec -it vault-app-0 -- vault operator init -n 1 -t 1`.

## Coding Style & Naming Conventions
- YAML and Helm templates only; use 2-space indentation and avoid tabs.
- Name files by resource purpose (e.g., `*-app.yaml`, `*-issuer-*.yaml`).
- Keep values and resource names aligned with chart names to make Argo CD apps discoverable.

## Testing Guidelines
- No automated test suite is present in this repository.
- Minimum validation for changes: `helm lint` + `helm template` on the affected chart(s).
- For cluster-facing changes, prefer a `kubectl apply --dry-run=server` check against the rendered output.

## Commit & Pull Request Guidelines
- Commit messages in this repo are short and imperative (e.g., “Add tailscale operator”, “Fix secret-store”).
- PRs should describe the chart(s) touched, the expected Argo CD impact, and any rollout notes.
- If secrets or Vault settings are involved, include manual steps (e.g., token update) in the PR description.

## Security & Configuration Notes
- Do not commit real secrets; use ExternalSecrets/Vault references and create tokens out of band.
- The Vault token secret for external-secrets-operator is manually provisioned as documented in `README.MD`.

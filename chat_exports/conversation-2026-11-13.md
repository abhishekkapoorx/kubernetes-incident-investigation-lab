# Chat Export

Date: 2026-11-13
Repository: `kubernetes-incident-investigation-lab`

## Project context

The project is a read-only incident investigation agent for Kubernetes incidents. The MVP uses Google's Online Boutique in a local `kind` cluster, deterministic evidence collection, LLM hypotheses with evidence citations, verification, Slack briefings, and replay/evaluation scenarios.

## Work completed

- Inspected the repository and Online Boutique manifests before editing.
- Confirmed the Boutique source supports `EXTRA_LATENCY` in `productcatalogservice`.
- Added eight reversible failure scenarios under `scenarios/`:
  - CrashLoopBackOff from missing environment configuration
  - OOMKilled from a low memory limit
  - ImagePullBackOff from a bad image tag
  - Service selector mismatch
  - Injected product catalog latency
  - Incorrect frontend readiness probe port
  - Unschedulable cartservice CPU request
  - Redis dependency scaled to zero
- Added baseline snapshots, ground-truth `expected.yaml` files, firing/resolved Alertmanager fixtures, safety helpers, an interactive scenario runner, and `verify_all.sh`.
- Added `NAMESPACE` configuration. The current local Boutique deployment uses `default`; the scripts default to `boutique`.
- Added a root `README.md` with demo deployment, monitoring, and scenario usage instructions.
- Added observability to the standalone Python demo stack:
  - Prometheus metrics
  - OpenTelemetry traces
  - Grafana dashboard and datasources
  - OpenTelemetry Collector
  - Tempo
  - ServiceMonitor configuration
- Fixed duplicate Kubernetes `metadata.labels` mappings in the standalone demo manifests.
- Moved the standalone demo app into `demo-apps/kubernetes-demo/`:
  - `Kubernetes/`
  - `package/`
  - `scripts/`
  - `cluster-config.yaml`
- Kept `scenarios/`, the root Makefile, `README.md`, and `handoff.md` at the repository root.
- Tracked Online Boutique as a submodule at `demo-apps/microservices-demo`.
- Renamed the GitHub repository to `kubernetes-incident-investigation-lab`.

## Commits pushed

- `eb82f4d` - `feat: add reproducible incident scenarios`
- `e6205c0` - `feat: add demo observability stack`
- `06aa2b5` - `docs: add usage guide and track Online Boutique`
- `e1c2374` - `refactor: organize demo apps separately`

Remote:

`https://github.com/abhishekkapoorx/kubernetes-incident-investigation-lab`

## Validation performed

- Bash syntax validation passed for scenario scripts.
- Alertmanager JSON fixture validation passed.
- Python compilation passed.
- PowerShell script parsing passed.
- Docker images for users, orders, and frontend built successfully.
- Relocated application Docker builds passed.
- Relocated Kubernetes manifests passed client-side dry-run validation.
- Interactive scenario menu was tested with scripted exit input.
- Full scenario smoke testing was initially blocked by an unavailable `kind-*` Kubernetes context; later scenario 01 was observed manually in CrashLoopBackOff.

## Current caveats

- The nested Online Boutique checkout has separate local changes that were intentionally not included in the outer repository commits. Its upstream remote is Google's repository.
- The current local cluster deploys Boutique into `default`, so use `export NAMESPACE=default` when running scenarios.
- The static Alertmanager fixtures use the canonical `boutique` namespace unless adjusted for another deployment namespace.
- GitHub reported three moderate Dependabot vulnerabilities on the default branch.

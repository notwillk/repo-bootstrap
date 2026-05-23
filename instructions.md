Bootstrap the project as a reproducible, contributor-friendly monorepo.

Use a Dockerfile-based `.devcontainer` as the canonical development environment, reusing the same base image the repository publishes for development or CI where applicable. Install required language/toolchain dependencies, Docker CLI access for image/integration workflows, useful devcontainer features, and VS Code settings/extensions. Pin major tool versions where practical for reproducibility.

Install Checksy, common-utils, Node.js, shellcheck, and shfmt in the devcontainer Dockerfile instead of a post-create script.

Ensure `tools/bin` is on the devcontainer `PATH`.

Vendor the `checksy-workflow` Agent Skill by copying the upstream skill contents from `notwillk/checksy` into `.agents/skills/checksy-workflow`. Record the upstream repository, source path, and pinned commit SHA in `skills-lock.json`.

Use Moon as the repo task orchestrator. Automatically discover projects under `apps/*`, `packages/*`, `samples/*`, and `tools/*`.

Organize the repository as:
- `apps/*` for binaries/services
- `packages/*` for reusable libraries
- `samples/*` for executable examples
- `docs/*` for Markdown documentation
- `images/*` for container/runtime artifacts

Expose consistent root Moon tasks:
`format`, `check`, `build`, `verify`, and `package`.

Keep root build/check/format automation as thin wrappers around the canonical commands (for example, `build` should just run `moon run *:build`). Each project should expose matching tasks where relevant.

Use Checksy for end-to-end and workspace health checks. Keep `verify.checksy.yaml` files close to the project or sample they validate so ownership stays local. Verification tasks should fail fast and be safe to run repeatedly.

Prefer samples as both human documentation and executable smoke tests. Each sample should include:
- a README with commands a person can run
- a `verify` task that exercises the same workflow automatically

Keep docs as plain Markdown in `docs/` and the root README. The root README should provide a repository map, common commands, and the public interface. Detailed design, architecture, runtime, and contributor documentation should live under `docs/`.

Prefer explicit automation over tribal knowledge. If contributors need to know how to build, test, package, validate, or run something, encode it as a Moon task, small script under `tools/bin`, or Checksy rule. CI, local development, documentation, samples, and agent workflows should all exercise the same commands.

Provide `tools/bin/check` and `tools/bin/format` scripts that use shellcheck and shfmt, and ensure the devcontainer installs those tools.

Set the devcontainer post-create command to run Checksy against `verify.checksy.yaml`.

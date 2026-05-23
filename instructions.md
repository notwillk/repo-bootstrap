Bootstrap the project as a reproducible, contributor-friendly monorepo.

Use a `.devcontainer` as the canonical development environment. It should install required language/toolchain dependencies, Docker CLI access for image/integration workflows, useful devcontainer features, and VS Code settings/extensions. Pin major tool versions where practical for reproducibility.

Install Checksy in the container with:
`curl -fsSL https://raw.githubusercontent.com/notwillk/checksy/main/scripts/install.sh | bash`

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

Each project should expose matching tasks where relevant.

Use Checksy for end-to-end and workspace health checks. Keep `verify.checksy.yaml` files close to the project or sample they validate so ownership stays local. Verification tasks should fail fast and be safe to run repeatedly.

Prefer samples as both human documentation and executable smoke tests. Each sample should include:
- a README with commands a person can run
- a `verify` task that exercises the same workflow automatically

Keep docs as plain Markdown in `docs/` and the root README. The root README should provide a repository map, common commands, and the public interface. Detailed design, architecture, runtime, and contributor documentation should live under `docs/`.

Prefer explicit automation over tribal knowledge. If contributors need to know how to build, test, package, validate, or run something, encode it as a Moon task, small script under `tools/bin`, or Checksy rule. CI, local development, documentation, samples, and agent workflows should all exercise the same commands.

# Publishing `nbq-mcp`

The package is **not published yet**. It cannot be released before `nbq` 1.0.0 is on PyPI:
`pyproject.toml` currently resolves the SDK from the release branch of `Zelinqa/nbq-sdk`
through `[tool.uv.sources]`, and a published wheel must depend on a published `nbq`.

Once a registry version exists it is immutable and must never be reused.

## Before the first release

1. Publish `nbq` 1.0.0 to PyPI (see `PUBLISHING.md` in `Zelinqa/nbq-sdk`).
2. Remove the `[tool.uv.sources]` block from `pyproject.toml` so `nbq` resolves from PyPI.
3. Run `uv lock`, commit `uv.lock`, and switch the workflows back to
   `uv sync --group dev --locked`.
4. Run the live suite against staging once (`NBQ_LIVE=1`, see `README.md`).

## One-time GitHub setup

In `Zelinqa/nbq-mcp`, create a GitHub Actions environment named `pypi`, add Farouk as a
required reviewer, and restrict deployments to the `main` branch.

## PyPI Trusted Publisher

While logged in to PyPI, open **Account settings → Publishing → Add a new pending
publisher** and enter exactly:

| Field | Value |
|---|---|
| PyPI project name | `nbq-mcp` |
| GitHub owner | `Zelinqa` |
| GitHub repository | `nbq-mcp` |
| Workflow name | `publish-pypi.yml` |
| Environment | `pypi` |

A pending publisher does **not** reserve the name. The first successful upload creates the
PyPI project and claims it. No API token is ever created or stored: the release job
authenticates with OIDC and holds `id-token: write` only in the `publish` job, which
downloads the artifact built by the unprivileged job.

## Release

1. Update the version in `pyproject.toml` and `src/nbq_mcp/__init__.py` — both must match.
2. Merge the reviewed release changes into `main`. Only Farouk performs this merge.
3. Run **Publish nbq-mcp to PyPI** from `main` with confirmation `publish-nbq-mcp`.
4. Approve the protected `pypi` environment deployment.
5. Verify <https://pypi.org/project/nbq-mcp/>, then check the published artefact in a clean
   environment: `uvx nbq-mcp --version`, and one real tool call through a host.
6. Add at least one additional trusted Zelinqa owner to the PyPI project.

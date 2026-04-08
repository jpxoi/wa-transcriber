# Releasing wa-transcriber

## One-time: PyPI Trusted Publishing

Releases use [PyPI Trusted Publishing](https://docs.pypi.org/trusted-publishers/) (OpenID Connect). No `PYPI_API_TOKEN` secret is required in GitHub.

1. Ensure the **wa-transcriber** project exists on [PyPI](https://pypi.org/) (first upload may be local `uv publish` with a token, or create the project and add the publisher per PyPI UI).
2. In PyPI: **Your projects** → **wa-transcriber** → **Publishing** → **Add a new pending publisher** (or manage existing).
3. Set:
   - **PyPI project name:** `wa-transcriber`
   - **Owner:** `jpxoi`
   - **Repository name:** `wa-transcriber`
   - **Workflow name:** `publish.yml`
   - **Environment name:** leave blank unless you use a [GitHub Environment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment) (then match it on both sides).

See: [Adding a trusted publisher to PyPI](https://docs.pypi.org/trusted-publishers/adding-a-publisher/).

## Cut a release

1. Update **`[project].version`** in `pyproject.toml` to the new semver (e.g. `1.3.1`).
2. Commit and push to the default branch (as needed for your process).
3. Create and push an annotated tag whose **`v`-prefix matches** that version:

   ```bash
   git tag -a v1.3.1 -m "Release v1.3.1"
   git push origin v1.3.1
   ```

4. **[Publish](https://github.com/jpxoi/wa-transcriber/actions/workflows/publish.yml)** runs on the tag push: it checks the tag vs `pyproject.toml`, builds with `uv build`, uploads to PyPI, and creates a GitHub Release with `dist/*` attached.

If the workflow reports a version mismatch, the tag name (without `v`) must equal `project.version` in `pyproject.toml`.

## Dry run locally

```bash
uv build
```

Upload is only performed in CI (or via `uv publish` with credentials on your machine).

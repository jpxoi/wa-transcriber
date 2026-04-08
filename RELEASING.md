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

### PR-first flow (recommended)

If **`main`** only moves via merged PRs, treat the release like any other change: version bump lands on `main` through a PR, then you tag that history.

1. **Branch** from `main`, bump **`[project].version`** in `pyproject.toml` (and README/changelog if you want them in the PyPI tarball).
2. Open a **PR**, wait for **CI** to pass, **merge** to `main`.
3. **Update local `main`** and tag the merge commit (or any commit on `main` that already contains that version):

   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.3.4 -m "Release v1.3.4"
   git push origin v1.3.4
   ```

   Use the real version: `v` + same string as `project.version` (e.g. `1.3.4` → `v1.3.4`).

4. **[Publish](https://github.com/jpxoi/wa-transcriber/actions/workflows/publish.yml)** runs on the tag push: checks tag vs `pyproject.toml`, **`uv build`**, PyPI upload, GitHub Release with `dist/*`.

**Tagging from GitHub (optional):** After the PR is merged, use **Releases → Draft a new release → choose tag** → create new tag `v1.3.4` on `default` branch, publish. That still triggers `publish.yml` on `push: tags: ["v*"]`. Ensure the selected commit is the one with the bumped `pyproject.toml`.

### Direct on `main` (solo / emergency)

1. Update **`[project].version`** in `pyproject.toml`.
2. Commit and push to `main`.
3. Tag and push as in step 3–4 above.

If the workflow reports a version mismatch, the tag name (without `v`) must equal `project.version` in `pyproject.toml`.

## Dry run locally

```bash
uv build
```

Upload is only performed in CI (or via `uv publish` with credentials on your machine).

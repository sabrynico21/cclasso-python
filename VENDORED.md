# Source provenance notice

`cclasso-python/` is a **standalone git repository** (released independently)
integrated into `electron-analysis-app` as a **git submodule**. The app pins one
commit of this repository; a clean app clone fetches it via
`git clone --recurse-submodules` / `git submodule update --init`.

## History of this directory in the app

It was previously vendored (source copied into the root app repository) because
a statistical release-readiness audit blocked gate 4 ("regenerate fixtures from
a clean checkout") while the directory was untracked. Before that, an earlier
standalone repository existed and its history is preserved as a Git bundle in
`VENDOR_HISTORY.bundle` (created with `git bundle create ... --all`), restorable
with:

```bash
git clone cclasso-python/VENDOR_HISTORY.bundle cclasso-python-history
```

## What is pinned

| item | value |
|---|---|
| port | Python translation of CCLasso R **v2.0** |
| upstream R source | `CCLasso-master/R/cclasso.R` |
| pinned R copy | `tests/r_reference/reference/R/cclasso.R` |
| earlier standalone commit | `3ca924f9cc0f52c5952154d476656484af6e9fc5` (`main`) |
| license | LGPL-3.0-or-later (see `LICENSE`) |

## What is not tracked here

Build and cache artifacts are ignored by this repository's `.gitignore`
(`.venv/`, `__pycache__/`, `*.egg-info/`, `.pytest_cache/`, `.ruff_cache/`).
The R source-of-truth package `CCLasso-master/` lives in the app repository
(git-ignored there); the port itself only needs the pinned copy under
`tests/r_reference/reference/`.

## Reproducing from a clean checkout

```bash
cd cclasso-python
python -m venv .venv
.venv/bin/pip install -e '.[test]'
.venv/bin/python -m pytest -q
```

Regenerating the R goldens additionally requires R plus `SpiecEasi` and
`phyloseq`; see `tests/r_reference/README.md`.

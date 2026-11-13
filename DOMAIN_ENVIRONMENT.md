# Reproduce the data-engineering environment

Source: https://github.com/LibraryOfCongress/bagit-python/commit/4bd2713cedbe1f8e634567c20ef0dded9622011d

This repository republishes the tracked files of that upstream commit under their original licenses.
Upstream CI workflows are omitted from the evaluation snapshot. This commit adds environment instructions
and the dependency lock only. The source license is CC0-1.0; original license and attribution files remain.

Python 3.13.15 and uv were used on Linux. From a fresh checkout:

```sh
uv venv --python 3.13.15 .venv
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 SERVER=NONE STRATEGY=MOCK_SYNC SETUPTOOLS_SCM_PRETEND_VERSION_FOR_PYTHON_CAN=4.6.1.dev0 SETUPTOOLS_SCM_PRETEND_VERSION_FOR_BAGIT=1.9.0.dev0
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider test.py
```

The selected tests use small fixtures or in-process substitutes. They need no remote service after dependency setup.
Start a fresh checkout and rebuild its virtual environment to restore initial state.

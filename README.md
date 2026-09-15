# EASN Cloud

Specification for the EASN Cloud Backend and EASN Cloud UI.
Backend implementation has not started.

Start with [the handoff](docs/handoff.md) and the
[current continuation status](docs/sphinx/source/chapters/status.rst).
The first specification increment covers pilot scope, actors, and boundaries.
New wording is a draft derived from the handoff; unresolved choices remain open.

## Build documentation

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r docs/requirements.txt
.venv/bin/python -m sphinx -n -W --keep-going -b html docs/sphinx/source docs/sphinx/build/html
```

Open `docs/sphinx/build/html/index.html`. Python must have venv support.
The GitHub workflow builds on pull requests and publishes main through Pages;
the repository must have GitHub Pages configured to use GitHub Actions.
No deployment or repository settings have been changed by this increment.

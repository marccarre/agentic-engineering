# Python

## `pyproject.toml` tooling & linting rules

```toml
[project]
name = "${PROJECT_NAME}"
version = "0.1.0"
requires-python = ">=${PYTHON_VERSION}$"
dependencies = []

[dependency-groups]
dev = [
    "prek>=0.5.5",
    "pytest>=9.1.1",
    "ruff>=0.17.1",
    "rumdl>=0.3.0",
    "ty>=0.0.89",
    "typos>=1.51.1",
]

[tool.ruff.lint]
# Each rule set enforces a rule from AGENTS.md. Extend, so the defaults stay on
extend-select = [
    "ANN",    # flake8-annotations: every argument and return value carries a type
    "ANN401", # any-type: and that type is a specific one, never `Any`
    "C901",   # mccabe: a function stays simple enough to do only one thing
    "D",      # pydocstyle: public modules, classes and functions carry a docstring
    "G",      # flake8-logging-format: log calls defer formatting to `logging`
    "LOG",    # flake8-logging: `logging` is set up and called correctly
    "PLR09",  # pylint: a function keeps few arguments, branches and statements
    "PT",     # flake8-pytest-style: tests use idiomatic pytest constructs
    "T20",    # flake8-print: diagnostics go to `logging`, never to `print`
]

[tool.ruff.lint.mccabe]
# Tighter than the default 10: a function that branches more than this is doing more than one thing,
# and wants splitting into sub-functions
max-complexity = 8

[tool.ruff.lint.pydocstyle]
# `google` adds D417, so a multi-line docstring must describe every parameter
# A one-line docstring stays exempt, which keeps simple functions terse
# Types stay in the annotations: no ruff rule enforces that, see AGENTS.md
convention = "google"

[tool.ruff.lint.per-file-ignores]
# Test names and their Given/When/Then comments already say what each test does
"**/tests/**" = [
    "D100", # undocumented-public-module
    "D103", # undocumented-public-function
    "D104", # undocumented-public-package
]

[tool.rumdl]
include = ["**/*.md"]

[tool.rumdl.MD013] # line-length
line-length = 120
code-blocks = false
tables = false
headings = true
reflow = true # Wrap long lines.

[tool.rumdl.MD024] # no-duplicate-heading
siblings-only = true # Allow duplicates under different parents.

[tool.typos.files]
extend-exclude = []

[tool.uv.workspace]
members = ["packages/*"]
```

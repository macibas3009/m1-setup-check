# What `make test` does

`make test` runs a single command, from `Makefile`:

```make
test:
	pytest -q
```

It starts pytest in quiet mode (`-q`), so you get dots and a short summary instead of one line per test. The repo has no pytest config file (`pytest.ini`, `pyproject.toml` or `setup.cfg`), so pytest uses its defaults and finds any `test_*.py` files under the repo. Right now the only one is `tests/test_smoke.py`, which has two tests:

1. **`test_openapi_document_can_be_loaded`** opens `docs/openapi.yaml` and checks that:
   - the file is valid YAML,
   - the `openapi` version starts with `3.`,
   - `paths` is not empty.

2. **`test_participant_files_are_present`** checks that these required files exist: `.claude/settings.json`, `.devcontainer/devcontainer.json`, `CLAUDE.md`, `Makefile`, `tracker/CR-2.md` and `tracker/README.md`. If any are missing, it fails and lists them ("Trūkst faili: …").

## Related targets

- **`make test` doesn't check the API contract.** It only confirms the YAML loads and has a version and paths. `make lint-contract` (`tools/lint_contract.py docs/openapi.yaml`) does the contract check, so run it too whenever you change `docs/openapi.yaml`.
- **`make verify-setup` runs only the smoke tests.** It runs `pytest -q tests/test_smoke.py` after checking the environment (Python, the packages, Pydantic v2, the `claude` CLI and `setup/claude-answer.md`). `make test` runs every test pytest finds, so any test files you add later will run too.

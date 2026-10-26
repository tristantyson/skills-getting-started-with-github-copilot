# Project Guidelines

## Architecture

- The backend is a FastAPI app in `src/app.py`. It owns the in-memory `activities` dictionary and exposes activity listing, signup, and unregister endpoints.
- The root route redirects to `src/static/index.html`; the static frontend is mounted at `/static` and calls the API from `src/static/app.js`.
- Activity and participant data is not persisted and resets when the server restarts. Keep the API response shape aligned with what the frontend reads.
- `.github/steps/` and `.github/workflows/` implement the repository's staged GitHub Copilot exercise; preserve their step/workflow progression when changing exercise content.

## Build and Test

- Install runtime dependencies with `python -m pip install -r requirements.txt`.
- Run the app from the repository root with `python -m uvicorn src.app:app --reload --reload-include 'src/static/*'`.
- Backend tests belong in a top-level `tests/` directory and use pytest with FastAPI's `TestClient`. Both pytest and `httpx` are listed in `requirements.txt`.
- Run the suite with `python -m pytest`.
- Run one test with `python -m pytest tests/test_app.py::test_name`.
- `pytest.ini` adds the repository root to `pythonpath`, so tests can import the app as `from src.app import app`.

## Conventions

- API routes operate directly on the module-level in-memory `activities` mapping; preserve its existing activity fields and participant-list representation.
- Follow the existing HTTP behavior: unknown activities return 404, duplicate signups return 400, and unregistering an absent participant returns 404.
- Tests that mutate participant lists should restore the affected state so tests remain independent despite the shared in-memory app data.
- Structure tests using Arrange-Act-Assert: prepare test state and inputs, make the API request, then assert the response and resulting state.

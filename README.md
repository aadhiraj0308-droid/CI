name: CI
on: [push,pull_request]
jobs:
  tests:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        service: [backend, frontend]
    steps:
      - uses: actions/checkout@v4
      - name: Setup
        run: |
          if [ "${{ matrix.service }}" = "backend" ]; then
            python -m venv venv && . venv/bin/activate
            pip install -r backend/requirements.txt
            pytest backend/tests
          else
            cd frontend
            npm ci
            npm test -- --watchAll=false
          fi

# Local preview

## Run locally

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Then open `http://127.0.0.1:8000`.

## Build check

```bash
mkdocs build --strict
```

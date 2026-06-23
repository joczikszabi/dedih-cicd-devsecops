# CI/CD és DevSecOps demo

Minimális FastAPI alkalmazás GitHub Actions CI workflow-val.

Ez a repo egy oktatási demo a DEDIH 2.0 / ELTE "CI/CD és DevSecOps" kurzushoz.
A cél, hogy egy futtatható, valódi (bár szándékosan apró) projekten lehessen
megmutatni a CI/CD baseline-tól a security gate-ekig (branch protection, secret
scan, supply chain audit, AI assisted review) vezető utat.

## Stack

- Python 3.11+
- FastAPI + Pydantic
- pytest, ruff
- GitHub Actions

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Futás

Tesztek:

```bash
pytest
```

Lint:

```bash
ruff check .
```

Formátum:

```bash
ruff format .
```

Helyi szerver:

```bash
uvicorn app.main:app --reload
```

Swagger UI: <http://127.0.0.1:8000/docs>

## Ágak

| Ág | Tartalom |
| --- | --- |
| `main` | Zöld baseline. Egy GET és egy POST végpont, három teszt. |
| `feature/bad-pr` | Új határeset-teszt szándékos off-by-one bug-gal (51 vs 50). |
| `secrets-leak` | `app/config.py`-ban planted "fake" hitelesítő adat, `DONOTUSE` jelölt. |

## A CI workflow

A `.github/workflows/ci.yml` egy aktív `build-and-test` job-bal indul (checkout,
install, `ruff check`, `ruff format --check`, pytest). Alatta három kommentelt
szekció vár a `# ` jelek mögött:

- `gitleaks` secret scan
- `pip-audit` SCA
- `anthropics/claude-code-action` AI code review

Ezek a kurzus során élőben kerülnek aktiválásra.
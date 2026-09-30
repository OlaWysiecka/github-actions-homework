# GitHub Actions Workflow

## Opis

Projekt przedstawia zaawansowany workflow GitHub Actions wykorzystujęcy
warunki `if`, zmienne środowiskowe, zależności pomiędzy jobami oraz artefakty.

## Uruchamianie

Workflow uruchamia się dla:

- `push`
- `pull_request`

## Zmienne środowiskowe

Workflow wykorzystuje:

- `PYTHON_VERSION` - wersja Pythona
- `APP_NAME` - nazwa aplikacji

## Build

Job `build`:

1. Pobiera kod repozytorium.
2. Konfiguruje Pythona.
3. Instaluje zależności.
4. Buduje aplikację.
5. Dla push do `main` zapisuje artefakt.

## Test

Job `test` wykonuje się po udanym buildzie.

Uruchamia testy za pomocą:

```bash
pytest -v
```

## Deploy
Job deploy wykonuje się tylko podczas:
```bash
push
```
do gałęzi main

### Warunek:
```bash
if: github.event_name == 'push' && github.ref == 'refs/heads/main'
```

## Final Status
Job status raportuje wynik pozostałych jobów.

Używa:

```bash
if: always()
```

Dzięki temu status jest raportowany również w przypadku błędu.

## Artefakty
Dla push do main tworzony jest artefakt:

```bash
build-artifact
```

Można go pobrać w:
GitHub -> Actions -> Workflow run -> Artifacts

## Uruchamianie lokalne
```bash
# zależności
pip install -r requirements.txt

# uruchomienie
python app.py

# testy
pytest -v
```

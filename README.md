# Roost — Chicken Incubator Tracker

An offline desktop app for backyard poultry keepers to manage egg incubation from setting to hatch: batches, hatch dates, daily temperature, humidity and turning, candling notes, and hatch results. It supports several bird types (chicken, duck, quail, turkey, peafowl and more) with breeds managed by an admin, and local admin and general user accounts.

Built with Python 3, Tkinter and SQLite. No internet connection, accounts or external services are needed.

## Requirements

- Python 3.10 or newer
- Tkinter (bundled with Python on macOS; on Debian/Ubuntu: `sudo apt install python3-tk`)

No third-party packages are needed.

## Run

```bash
python3 main.py
```

The SQLite database (`incubator.db`) is created next to `main.py` on first run and is not committed to the repository.

## Test

```bash
python3 -m unittest discover -s tests -t .
```

## Project structure

```
main.py            App entry point and window navigation
database.py        SQLite connection, schema and data access
calculations.py    Hatch dates, incubation progress and hatch rate
models.py          Data classes and input validation
auth.py            Local user accounts, roles and sign-in
ui/                Tkinter screens (sign-in, admin, dashboard, batch form, daily tracker, candling, hatch results)
tests/             unittest suites
docs/              Specification additions
```

All test and sample data is fictional.

## Licence

MIT. See [LICENSE](LICENSE).

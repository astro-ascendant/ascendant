<p align="center">
  <img src="https://raw.githubusercontent.com/astro-ascendant/website/main/assets/brand/wordmark.svg" alt="Ascendant" width="360" />
</p>

Ascendant is a Python library for sidereal astrology. It calculates charts,
Vimshottari dashas, yogas, Ashtakavarga, and a seven-karaka Jaimini core with
Swiss Ephemeris through `pyswisseph`, and returns plain Python dictionaries and
lists.

## Install

Requires Python 3.11 or later.

```bash
python -m pip install astro-ascendant
```

`pyswisseph` and `python-dateutil` install automatically. No separate `flatlib`
setup is required.

## Quick start

```python
from ascendant import Ascendant

birth = {
    "year": 1990,
    "month": 1,
    "day": 1,
    "hour": 12,
    "minute": 0,
    "second": 0,
    "utc": "+5:30",
    "latitude": 28.6139,
    "longitude": 77.2090,
}

astro = Ascendant(**birth)

chart = astro.get_chart(division=1)
yogas = [yoga for yoga in astro.get_yogas() if yoga["present"]]
dasha = astro.get_current_dasha()
sav = astro.get_sav()["sarva"]
jaimini = astro.get_jaimini()
```

Charts default to the Lahiri ayanamsa and Whole Sign houses.

## Features

- Sidereal planetary and house positions from Swiss Ephemeris
- Divisional charts, including D1, D9, and D10
- Vimshottari Mahadasha and Antardasha timelines
- Yoga evaluation with structured results
- Bhinnashtakavarga, Sarvashtakavarga, Shodhana, and Shodhya Pinda
- Seven-karaka Jaimini core derived from D1 and D9
- Typed, immutable configuration

## Configuration

Override the defaults per instance:

```python
astro = Ascendant(**birth, ayanamsa="Krishnamurti", house_system="Porphyry")
```

For typed settings, pass a configuration object:

```python
from ascendant import AscendantConfig, Ayanamsa, HouseSystem

astro = Ascendant(
    **birth,
    config=AscendantConfig(
        ayanamsa=Ayanamsa.LAHIRI,
        house_system=HouseSystem.PORPHYRY,
    ),
)
```

Explicit arguments take precedence over `config`, which takes precedence over
the built-in defaults; `get_config()` returns those defaults. Unsupported
values raise `ValueError`.

Supported ayanamsas: `Lahiri`, `Lahiri_1940`, `Lahiri_VP285`, `Lahiri_ICRC`,
`Raman`, `Krishnamurti`, `Krishnamurti_Senthilathiban`.

Supported house systems: `Whole Sign`, `Placidus`, `Equal`, `Equal 2`,
`Porphyry`.

## Documentation

- [Divisional charts](https://ascendant-docs.vercel.app/docs/library/charts)
- [Dasha systems](https://ascendant-docs.vercel.app/docs/library/dasha)
- [Jaimini core](https://ascendant-docs.vercel.app/docs/library/jaimini)
- [Yoga combinations](https://ascendant-docs.vercel.app/docs/library/yoga)
- [Configuration](https://ascendant-docs.vercel.app/docs/configuration)

The documentation source lives in the separate
[`ascendant-docs`](https://github.com/astro-ascendant/ascendant-docs)
repository.

## Development

Create the environment with [uv](https://docs.astral.sh/uv/), then use the Make
targets:

```bash
uv venv --python 3.11
uv pip install -e ".[test]" pyright

make help
make test
make typecheck
make lint
```

Swiss Ephemeris accuracy cases live in
[`tests/test_swiss_ephemeris_accuracy.py`](https://github.com/astro-ascendant/ascendant/blob/main/tests/test_swiss_ephemeris_accuracy.py).

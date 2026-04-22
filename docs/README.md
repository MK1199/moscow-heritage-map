# Moscow Heritage Map

Interactive web map of cultural heritage buildings in Moscow's Central Administrative District (ЦАО).

![Moscow Heritage Map](preview.png)

## About

An interactive map of **2,440 protected heritage buildings** in central Moscow, built with open government data. Each building is color-coded by protection category and clickable — revealing its name, address, era, and a link to street-level panorama.

Inspired by [The Heritage of Sofia](https://morphocode.com/the-heritage-of-sofia/) by Morphocode.

## Data

**Source:** [data.mos.ru](https://data.mos.ru), dataset №530 — *Объекты культурного наследия и выявленные объекты культурного наследия*

**Filters applied:**
- Administrative district: Центральный административный округ
- Object type: Здание

**Key stats:**
- 2,440 buildings
- 10 districts (Басманный, Хамовники, Тверской, Таганский, Пресненский, Якиманка, Замоскворечье, Арбат, Мещанский, Красносельский)
- 2 protection categories: federal (1,175) and regional (1,265)
- Building dates extracted via regex from object names — 87% coverage

## Features

- Building footprints colored by protection category (federal / regional)
- Heatmap layer toggle showing heritage density
- Sidebar filters by category and district
- Click-to-zoom on district selection
- Popup cards with building details and Yandex Panorama links
- Hover highlight
- Mobile-responsive layout

## Stack

| Stage | Tools |
|-------|-------|
| Data processing | Python, pandas, geopandas, shapely |
| Date extraction | regex (years, Roman numeral centuries, period prefixes) |
| Web map | Mapbox GL JS |
| Hosting | GitHub Pages |

## Project structure

```
moscow-heritage-map/
├── web/                          ← deployable website
│   ├── index.html
│   └── data/
│       ├── okn-cao-polygons.geojson
│       └── okn-cao-points.geojson
├── notebooks/
│   ├── 01-data-cleaning.ipynb
│   └── 02-analysis.ipynb
├── data/
│   └── raw/
│       └── data-530-21-04-2026.csv
├── processed/                    ← analysis outputs
│   ├── agg-categories.json
│   ├── agg-districts.json
│   ├── agg-epochs.json
│   └── agg-district-epoch.json
└── README.md
```

## Run locally

```bash
cd web
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy to GitHub Pages

The `web/` directory is configured as the publishing source. The map is accessible at:

```
https://<username>.github.io/moscow-heritage-map/
```

## Date extraction methodology

Building construction dates are embedded in object names in various formats:

| Format | Example | Extracted year |
|--------|---------|---------------|
| Exact year | «1895 г.» | 1895 |
| Year range | «1770–1780 гг.» | 1770 |
| Roman century | «XVIII в.» | ~1750 |
| Period prefix | «конец XIX в.» | ~1880 |
| Half-century | «1-я пол. XIX в.» | ~1825 |

The earliest date found in each name is used as the estimated construction date. 87% of buildings (2,122 / 2,440) yielded a date.

## Epoch distribution

| Epoch | Count |
|-------|-------|
| до XVIII в. | 241 |
| XVIII в. | 518 |
| 1-я пол. XIX в. | 378 |
| 2-я пол. XIX в. | 520 |
| 1900–1917 | 309 |
| 1918–1945 | 135 |
| после 1945 | 338 |

## License

Data: [data.mos.ru](https://data.mos.ru) open data license  
Map tiles: © Mapbox © OpenStreetMap

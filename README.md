# Bangladesh_Dengue-Early-Alert-System
[Live page]( https://tahreemk13.github.io/Bangladesh_Dengue-Early-Alert-System/)

A dengue early-action trigger for Bangladesh's eight divisions, benchmarked against the Sri Lanka Red Cross
dengue trigger, with a small facility-siting step after Lippi et al. (2020, Machala).

## Run

```bash
pip install -r requirements.txt       # plus Poppler's pdftotext (brew install poppler)
bash run_all.sh                       # rebuild everything from data/raw, about 8 minutes
```

The product runs on scripts but there are notebooks showing work for context. Open `outputs/index.html` in a browser. The cleaning is explained step by step in `notebooks/`.

## The dashboard

`index.html` at the repository root is the published page (GitHub Pages; the workflow copies
`outputs/index.html` there every Monday). It is self-contained, and works on phones.
- **English / বাংলা** button, top right. The Bangla follows Bangladesh usage (Bangla Academy spelling, বিভাগ, স্বাস্থ্য
  অধিদপ্তর, Bengali digits, lakh and crore). Hospital names stay in English. `?lang=bn` opens it in Bangla.
- **Map:** quiet (green), watch, warning, in action, unknown (grey hatch). Click or tap a division.
- **Division panel:** the numbers, then a call to action: what to expect, what to do, the 3 hospitals to supply
  first (`outputs/division_sites.csv`), and how much IV saline to have ready. Then the two charts.

## Run it every week (live)
`run_weekly.sh` is the whole loop: save new DGHS reports, add new weather days, rebuild the tables, forecast,
update statuses, pick sites, rebuild the dashboard, email alerts. `.github/workflows/before_the_peak.yml` runs it
on GitHub: a short job every day saves the day's report (DGHS removes them), and the full loop runs on Mondays.

## Key decisions already in the code (change in the config)
- **Threshold:** 1.5 times the **median** of the same calendar month in earlier years, at least 20 cases a week.
  The Sri Lanka text says "average"; the mean breaks after 2023 (see `outputs/metrics.json`, `sri_lanka_rule_as_written`).
- **Onset:** the first crossing that lasts 2 weeks after 2 quiet weeks, April to December.
- **Test scheme:** train only on earlier seasons (`past_only`). Leave-one-season-out is reported but secondary.
- **Map model:** recent cases only. Weather added no skill at 2 to 6 weeks.
- **Candidate sites:** DGHS's public hospital list with beds (552), plus 6 missing hospitals added by hand with sources (557 hospitals, 47,725 beds).
- **Alerts:** one email at *warning*, one at *in action*, April to December; stand down after 4 calm weeks (`alerts` in the config).

## Data sources and licences

DGHS daily admissions via Kaggle (licence to confirm); Mendeley monthly division data (CC BY 4.0);
OpenDengue V1.3 (CC BY 4.0); Kaggle national 2008–2019 (cross-check only); Open-Meteo ERA5; WorldPop 2020 1 km;
Malaria Atlas Project 2020 friction surfaces; OpenStreetMap health facilities via HDX (ODbL);
HDX COD-AB boundaries; DGHS public hospital list and facility lists; World Bank health indicators.
Every file, its licence and download date are in `data/raw/SOURCES.md`. Confirm the licences marked "to confirm" before publishing.

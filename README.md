# Solidarity of Premier League Teams
#### Do soccer teams that have played together for a longer period achieve greater league success than younger teams? 

Data acquisition and visualization project at the University of Reykjavik (VIFG - Vinnsla og framsetning gagna). 

Data scraping and analysis scripts focused on football league team experience ("team age") versus league performance, mainly for the English Premier League (EPL).

## What this project contains

- EPL player scraping from premierleague.com using Selenium + BeautifulSoup.
- Team-level feature engineering (mean seasons at club, quantiles, etc.).
- Final merged dataset with league results and team-experience metrics.
- Exploratory R analysis and plot generation.
- Experimental Bundesliga scraping utilities.

## Repository structure

- `epl_scraper.py`: Scrapes EPL player and squad history data and writes season CSV files.
- `epl_clean.py`: Builds derived team-experience metrics and merges with league table results.
- `bundesliga_scrape.py`: Experimental Bundesliga/Wikipedia scraping helpers.
- `analyze_data.r`: Visualization and exploratory analysis script.
- `data.json`: Cached EPL season/team IDs used by scraper.
- `epl_data/`: Season CSV files and merged result files.
- `plots/`: Generated charts and design source files.

## Requirements

Python 3.9+ is recommended.

Python packages used by scripts:

- `pandas`
- `requests`
- `beautifulsoup4`
- `selenium`

R packages used by analysis script:

- `tidyverse`
- `scales`
- `gridExtra`
- `ggthemes`
- `ggrepel`

### Selenium / browser driver

`epl_scraper.py` uses `webdriver.Chrome()`. Ensure Chrome and a compatible ChromeDriver are installed and available in your PATH.

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas requests beautifulsoup4 selenium
```

## Typical workflow

### 1) Scrape EPL player data

```bash
python epl_scraper.py
```

What it does:

- Loads season/team mappings from `data.json`.
- Scrapes players for each configured season/team.
- Writes one CSV per season (for example `2008.csv` ... `2017.csv`).

Note: output files are written in the current working directory by the scraper. Move them into `epl_data/` if needed for downstream scripts.

### 2) Build final EPL feature dataset

```bash
python epl_clean.py
```

Inputs:

- `epl_data/<year>.csv` (season player files)
- `epl_data/epl_result.csv` (league standings)

Output:

- `epl_data/epl_final.csv`

Generated columns include:

- `MeanAge`, `MedianAge`, `StdAge`
- `MinAge`, `MaxAge`, `Quant25`, `Quant75`
- `TeamSize`, `AgePos`

### 3) Run exploratory analysis in R

Open and run `analyze_data.r` in RStudio or VS Code with R support.

Important: this file currently contains absolute paths from an older local machine. Update those paths to this repository location before running.

## Data files

- `epl_data/epl_result.csv`: EPL final table by season.
- `epl_data/epl_final.csv`: Merged EPL standings + team-experience features.
- `epl_data/bundesliga_result.csv`, `epl_data/laliga_result.csv`: Additional league result tables.

## Notes and caveats

- This is a legacy research codebase; scripts are not yet packaged as a Python module.
- Scraping behavior may break if source websites change HTML structure.
- `bundesliga_scrape.py` appears exploratory/incomplete.
- Team matching in `epl_clean.py` uses string containment and may require manual checks for edge cases.

## Future improvements

- Add a `requirements.txt`.
- Parameterize input/output paths via CLI args.
- Add tests for team matching and metric calculations.
- Add reproducible plotting script outputs to `plots/`.
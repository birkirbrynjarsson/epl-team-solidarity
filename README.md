# Solidarity of Premier League Teams
#### Do soccer teams that have played together for a longer period achieve greater league success than younger teams? 

Data acquisition and visualization project at the University of Reykjavik (VIFG - Vinnsla og framsetning gagna). 

## Final report and generated plots

### Abstract
If you follow the Premier League or any top soccer league for that matter you can’t help but notice the amount of player transfers that happen between teams. Players seem to move quite frequently and short stays are more common than long ones. This is a correct observation, just by looking at the 2016-’17 Premier league season there were 224 individuals playing for a new team. This is in a year when the league had 771 players in total so little less than a third of the league’s players were playing for a new team. This got us thinking if these frequent trades are productive at all, do they translate to success on the pitch, or do younger inexperienced teams with a high player turnover actually achieve less success.

In our research we want to answer a simple question. Do teams that let players stay on their squad for many consecutive years fare better, i.e. is there a correlation between success and how long the players of a football team stay together.

In an attempt to answer this hypothesis we decided to look at the results for the last 20 years in the most popular sporting league in the world, the English Premier League. 

Read the full report here: [final-report.pdf](https://github.com/user-attachments/files/26752584/final-report.pdf)

### Plot 1: Correlation over 20 seasons in the Premier League

<img width="1012" height="600" alt="Plot-SeasonBySeason" src="https://github.com/user-attachments/assets/15e3d1f1-8408-4381-a66c-49e6125a3db6" />

### Plot 2: Comparing the least experienced teams to the most experienced teams every season

<img width="972" height="600" alt="Plot-3MostLeastXP" src="https://github.com/user-attachments/assets/960b3571-dfbc-417a-89ff-fdf9797a23a4" />

### Plot 3: All 20 seasons in a single plot

<img width="1043" height="650" alt="Plot-TeamAgetoPosition" src="https://github.com/user-attachments/assets/f9094ffa-0704-4c95-9b78-81681bf5d1e6" />

## What this project contains

Data scraping and analysis scripts focused on football league team experience ("team age") versus league performance, mainly for the English Premier League (EPL).

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

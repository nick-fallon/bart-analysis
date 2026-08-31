# BART Ridership Analysis

An exploratory analysis of 2025 Bay Area Rapid Transit (BART) ridership. The
project examines daily and weekly ridership patterns, a high-ridership Sunday
associated with San Francisco Pride, and station relationships derived from
origin-destination flows.

This repository is intentionally exploratory. The main notebook preserves
intermediate questions, alternative expressions, diagnostic output, and
commented plotting blocks. In particular, plots are often commented out so
that they are not regenerated on every run; commented code should not be
interpreted as obsolete.

## Dataset

The compressed, headerless CSV is stored at
`data/date-hour-soo-dest-2025.csv.gz`. The notebook assigns these columns:

| Column | Description |
| --- | --- |
| `date` | Date |
| `hour` | Hour (24-hour clock) |
| `origin` | Origin station |
| `destination` | Destination station |
| `trips` | BART's `Number of Exits` for the date-hour-origin-destination combination |

The file contains 9,143,257 rows covering all 365 days of 2025, all 24 hours,
and 50 origin/destination stations. Exit counts sum to 54,507,357.

Source: [BART Ridership Reports](https://www.bart.gov/about/reports/ridership).
BART documents the hourly origin-destination columns as `Date`, `Hour (24-hour
clock)`, `Origin Station`, `Destination Station`, and `Number of Exits`. The
local file's exact download date is not currently recorded in the repository.

## Current analysis

`notebooks/exploration.ipynb` currently contains two main lines of inquiry:

1. **Calendar and event exploration**
   - Aggregates systemwide daily ridership.
   - Compares weekday means, medians, and distributions.
   - Inspects low-ridership dates and Sunday ridership over time.
   - Compares June 29, 2025 with other Sundays, including station-level origin
     and destination baselines.

2. **Station-flow and network exploration**
   - Aggregates annual origin-destination flows into a directed graph.
   - Calculates station inbound/outbound strength, net flow, and imbalance.
   - Measures the diversity of station destination patterns with normalized
     entropy.
   - Clusters normalized destination profiles using hierarchical clustering.
   - Runs Louvain community detection and checks its sensitivity to random
     seeds using adjusted Rand index.

The analysis remains exploratory rather than a finalized statistical or causal
study. Cluster thresholds, event baselines, and other analytical decisions are
subjects for later evaluation; this initial version preserves them as they
were investigated.

## Repository layout

```text
.
├── data/
│   └── date-hour-soo-dest-2025.csv.gz
├── notebooks/
│   └── exploration.ipynb
├── src/                         # Reserved for possible future reusable code
├── README.md
└── requirements.txt
```

## Setup and use

The current environment uses Python 3.13. Create a virtual environment and
install the direct dependencies:

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Open `notebooks/exploration.ipynb` from JupyterLab. The dataset path is relative
to the notebook directory, so the checked-in layout should be retained.

Several plotting and inspection blocks are commented intentionally. Uncomment
only the outputs you want to regenerate, then run the relevant cell. The
notebook reads the full dataset in each of its two main analysis cells, so a
complete run requires enough memory for the uncompressed pandas data.

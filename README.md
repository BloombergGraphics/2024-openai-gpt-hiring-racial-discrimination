# Analyzing OpenAI's GPT for Racial and Gender Bias

This repository contains code to reproduce the findings featured in our story "[OpenAI's GPT Is a Recruiter’s Dream Tool. Tests Show There’s Racial Bias](https://www.bloomberg.com/graphics/2024-openai-gpt-hiring-racial-discrimination)".

Our methodology is described in at the [bottom of the article](https://www.bloomberg.com/graphics/2024-openai-gpt-hiring-racial-discrimination/#methodology).

Data that we collected and analyzed is in the `data` folder.

Jupyter notebooks used for data preprocessing and analysis are available in the `notebooks` folder.
Descriptions for each notebook are outlined in the Notebooks section below.

## Data

This directory is where inputs, intermediaries, and outputs are saved.

If you want to generate new resumes or rankings, you'll need to register and fund a OpenAI API key, and set the following environment variables: `OPENAI_ORG` and `OPENAI_API_KEY`.

```
data
├── intermediary
│   ├── resumes_to_rank.json
│   ├── resume_ranking
│   │   ├── gpt-3.5-turbo
│   │   └── gpt-4
│   └── embeddings
│       └── names_embedded_ada.json
├── output
│       ├── names_embedded_for_graphic.csv
│       ├── performance_ranking.csv
│       └── resume_ranking_for_graphics.csv
└── input
        ├── top_mens_names.json
        ├── top_womens_names.json
        └── Names_2010Census_Top1000.csv

```

Here's an explanation of some of the more important files.

| file                                         | description                                                                                                                                                                                                                                                                                       |
|:---------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `data/input/top_mens_names.json`             | Demographically-distinct names (see also `data/input/top_womens_names.json`) statistically-derived from North Carolina [voter records](https://www.ncsbe.gov/results-data/voter-registration-data) and [Census data](https://www.census.gov/topics/population/genealogy/data/2010_surnames.html). |
| `data/input/Names_2010Census_Top1000.csv`    | Most popular U.S. surnames taken from the U.S. Census Bureau.                                                                                                                                                                                                                                     |
| `data/intermediary/resumes_to_rank.json`     | Equally-qualified resumes generated from GPT-4 and edited. Also includes real job descriptions used to evaluate each resume.                                                                                                                                                                      |
| `data/intermediary/resume_ranking`           | Data from resume ranking experiment collected from OpenAI. Organized by model version > job title > date of collection.                                                                                                                                                                           |
| `data/output/performance_ranking.csv`        | Aggregated results from resume ranking experiment.                                                                                                                                                                                                                                                |
| `data/output/names_embedded_for_graphic.csv` | [ADA-002](https://platform.openai.com/docs/guides/embeddings) embeddings for demographically distinct names reduced to 2-dimensions using [UMAP](https://umap-learn.readthedocs.io/en/latest/).                                                                                                   |


We use shorthand to denote gender (`M` = male and `W` = female) as well as race and ethnicity (`A` = Asian, `H` = hispanic, `B` = Black, and `W` = White). For intersectional groups in `data/output/performance_ranking.csv` the notation we use for demographics (col `demo`) is `{race/ethnicity}_{gender}`, for example `A_W` means Asian women.

## Installation
### Python
Make sure you have Python 3.11+ installed. We used Miniconda to create a Python 3.11 virtual environment.

Then install the Python packages:
```pip install -r requirements.txt```

## Notebooks

Jupyter notebooks to collect, process, and analyze data can be found in the `notebooks` directory.
Notebooks should be run sequentially, you can use the command `nbexec notebooks` to run all notebooks.

### 0-deriving-names.ipynb
Statistically derives the demographically distinct names from voter registration records and the U.S. Decennial Census.

### 1-rank-resumes.ipynb
Use OpenAI's Chat API to rank eight near-identical resumes thousands of times across hundreds of names for four different jobs.

### 2-analysis-ranking-discrimination.ipynb
Analyze ranking experiment data to test for name-based discrimination.

### 3-word-embeddings.ipynb
Collect embeddings for demographically-distinct names using OpenAI's ADA-002 model, and view them in 2D using UMAP.



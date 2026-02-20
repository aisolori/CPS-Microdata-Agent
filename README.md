# CPS Microdata Agent (R)

An **agent-style CPS microdata assistant in R** built on **getCPS** + **ellmer**.  
It pulls CPS microdata **one state at a time**, stores datasets in a dedicated **agent working memory**, and exposes a set of registered tools for:

- searching the CPS variable dictionary
- fetching CPS microdata for a state/year-range
- transforming data (filter/mutate)
- exploring variables (categorical vs continuous using Census metadata)
- running weighted/structured analyses via tool-called R code
- fetching official variable definitions / labels from the Census API

> **Status:** Active development (interfaces may change).

---

## Core idea

This project creates an R “agent” with a persistent environment (`agent_memory`) that functions like working memory:

- `agent_memory$datasets`: named dataframes stored by the agent  
- `agent_memory$active`: the currently active dataset name  
- `cps_data`: alias bound to the active dataset for user-facing analysis code  

The agent registers tool wrappers (via `ellmer::tool()`) so a chat model can follow a strict workflow:

1. Assess & clarify (no tools)
2. Plan (`create_analysis_plan`)
3. Search (`search_cps_variables`, `get_suggested_weight`)
4. Fetch (`get_cps_data_state`)
5. Explore (`explore_variable`)
6. Transform / Analyze (`transform_data`, `analyze_data`)
7. Label (`get_variable_labels`)
8. Respond

---

## Requirements

### R packages
```r
library(getCPS)
library(tidyverse)
library(ellmer)
library(jsonlite)
# optional (only used for rendering markdown results in browser)
library(markdown)

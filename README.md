# 2015 USA Flight Delay Analysis using Power BI

## Setup Instruction
- Setup root directory by `git clone https://github.com/wencov/flight.git`

- Download and extract [flight-delays.zip](https://www.kaggle.com/usdot/flight-delays) into `flight-delays` folder under the `flight` root directory created from above

- To refresh data source, open [flight.pbix](flight.pbix) file in Power BI Desktop and under `Edit Queries` menu, change path variable and point it to the root folder directory such as `C:\Users\mzwen\Desktop\flight\`


## Assumptions:
- The original dataset includes flight cancellations `CANCELLED = 1`, those cancelled flights are excluded from this delay analysis

- *Major Origin/Destination Airports* are defined as airports with more than 100,000 annual origin/destination flights in 2015

- Flights are assumed to be `On-time` if `Arrival Delay` is less than 30 minutes

- `DEPARTURE_DELAY` and `ARRIVAL_DELAY` in minutes are transformed such that any negative value is replaced by `0`, and `Null` value is replaced by `-1` for flights that did not arrive the destination; `ARRIVAL_DELAY` more than 30 minutes are futher aggregated by hours under `Arrival Delay in Hours` to further reduce cardinality

- Three airports without `Latitude` and `Longitude` data are excluded in the map view. These are small airports (`UST`, `PBG`, `ECP`) with less than 5,000 annual flight volume

- There are around 480k flights during October 2015 that have Original and Destination Airports not in ITAT code. The numerical code seems to be U.S. Department of Transportation's airport code. This data quality issue is best fixed at data source before importing to Power BI due to performance concern. However, in this analysis, these airport codes are kept as is. And they are showing as `(Blank)` when analyzing by airport


## Checkpoints:
- DAX functions/code to build useful expressions and measures for the data model
  - DAX is used to generate `date` table, `origin[Volume]` column, `[% Delay]` measure for example

- PowerQuery to ingest and process the data sources
  - ETL is performed by PowerQuery and modified M code

- Data model design with fact and dimension tables based on reporting requirements
  - This report is supported by a dimensional model in star schema

- Visuals that best assist a user in analyzing this dataset and display insights;
  - Various visuals are utilized include bar chart, line chart, as well as maps

- Visual/Page and Report level filters
  - `Arrival Type` is set as report level filter, while page leve filter includes `Airline Name`

- Good use of slicers
  - Slicers are implemented on `Route` page such as `Airline Name`

- Demonstrate best practices in report design for performance and ability to scale for embedded solutions
  - Performance best practices are exercised throughout the entire pipeline from data ingestion (by only ingest relevant data) to data modeling (cardinality management, fact table shaping) to visualizations (pre-aggregation, filter setup) etc.

**Stretch Goals:**
- Using both DirectQuery and Import data sources to build a composite dimensional model
  - As csv file is not a [supported data source](https://docs.microsoft.com/en-us/power-bi/power-bi-data-sources) by DirectQuery, composite model is not implemented in this iteration. Import is also the recommended storage mode in this use case, as DirectQuery relies on high performance data source

- Use of aggregates/aggregation tables to support visual performance;
  - Flight volume by airport is aggregated on `origin` and `destination` tables in order to define and visualize major airports

- Setup for Row Level Security in PBI
  - [Row-level security (RLS)](https://docs.microsoft.com/en-us/power-bi/service-admin-rls) is implemented. A role called `Delta` is created to view only `DL` coded flights (Delta Air Lines)


## Bonus:
A simple exponential smoothing time series model is implemented in a Jupyter [notebook](tsa.ipynb) to analyze and predict the on-time performance of Alaska Airlines AS306 from Seattle (SEA) to San Francisco (SFO) in 2015. The following skills are demonstrated
- Data wrangling 
- Data profiling 
- Imputation method
- Visualization
- Statistical modeling
- Object-oriented programing in Python
- Markdown and LaTex math

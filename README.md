# 2015 USA Flight Delays Analysis using Power BI

## Setup Instruction
- Setup root directory by `git clone https://github.com/wencov/flight.git`

- Download and extract [flight-delays.zip](https://www.kaggle.com/usdot/flight-delays) into `flight-delays` folder under the `flight` root directory

- Open `flight.pbix` and under `Edit Queries` menu, change path variable to point to the root folder directory such as `C:\Users\mzwen\Desktop\flight\` if refreshing data source is required


## Assumptions:
- The original dataset includes flight cancellations `CANCELLED = 1`, those cancelled flights are excluded in this delay analysis

- *Major Origin/Destination Airports* are defined as airports with more than 100,000 origin/destination flights in 2015

- Flight is assumed to be `On-time` if `Arrival Delay` is less than 30 minutes

- `DEPARTURE_DELAY` and `ARRIVAL_DELAY` in minutes are transformed such that any negative value is 0, and NULL is replaced by -1 for flights that did not arrive the destination; `ARRIVAL_DELAY` more than 30 minutes are futher aggregated by hours under `Arrival Delay in Hours` to reduce cardinality

- Three airports without `Latitude` and `Longitude` data are excluded in the map view, this included `UST`, `PBG`, `ECP`. These are small airports with less than 5,000 annual flight volumne

- There are around 480k flights during October 2015 that have Original and Destination Airport not in ITAT code. The numerical code seems to be U.S. Department of Transportation's airport code. This data quality issue is best to be fixed at source before importing to Power BI due to performance concern. In this analysis, these airport codes are kept as is, however, they are showing as `(Blank)` when being analyzed by airports


## Checkpoints:
- DAX functions/code to build useful expressions and measures for the data model
  - DAX is used to generate `date` table, `origin[Volume]` column, `[% Delay]` measure

- PowerQuery to ingest and process the data sources
  - PowerQuery and modified M code is perform ETL

- Data model design with fact and dimension tables based on reporting requirements
  - This report is supported by a dimensional model in star schema

- Visuals that best assist a user in analyzing this dataset and display insights;
  - Various visuals are utilized include bar chart, line chart, as well as maps

- Visual/Page and Report level filters
  - `Arrival Type` is set as report level filter, while page leve filter includes `Airline Name`

- Good use of slicers
  - Slicers are implemented on `Route` page

- Demonstrate best practices in report design for performance and ability to scale for embedded solutions
  - Performance best practices are exercised throughout the entire pipeline from data ingestion (by only ingest relevant data) to data modeling (cardinality management, fact table shaping) to visualizations (pre-aggregation, filters).

**Stretch Goals:**
- Using both DirectQuery and Import data sources to build a composite dimensional model
  - As csv file is not a [supported date source](https://docs.microsoft.com/en-us/power-bi/power-bi-data-sources) by DirectQuery, composite model is not implemented in this iteration. Import is also the recommended storage mode in this use case, as DirectQuery relies on high performance data source

- Use of aggregates/aggregation tables to support visual performance;
  - Flight volume by airport is aggregated on `origin` and `destination` tables in order to determine and visualize major airports

- Setup for Row Level Security in PBI
  - [Row-level security (RLS)](https://docs.microsoft.com/en-us/power-bi/service-admin-rls) is implemented. A role called `Delta` is created to view only `DL` flights
  
# 2015 USA Flight Delays Analysis using Power BI

## Setup Instruction
- Setup root directory by `git clone https://github.com/wencov/flight.git`

- Download and extract [flight-delays.zip](https://www.kaggle.com/usdot/flight-delays) into `flight-delays` folder under the `flight` root directory

- Open flight.pbix and change path variable to point to the root folder directory such as `C:\Users\mzwen\Desktop\flight\` if refresh data source is required.


## Assumptions:
- The original dataset includes cancellations `CANCELLED = 1`, those flights are removed in this analysis;
- `DEPARTURE_DELAY` and `ARRIVAL_DELAY` are transformed such as any negative value is 0, and NULL is replaced by -1 for flights that did not arrive the destination;
- Airports without `Latitude` and `Longitude` data are not included in the map view, this included `UST`, `PBG`, `ECP`. These are small airports with less than 5,000 annual flight volumne;
- Major Airport is defined as airports with more than 100,000 origin flights in 2015;
- There are around 480k flights during October 2015 that have Original and Destination Airport not in ITAT code. The numerical code seems to be U.S. Department of Transportation's airport code. This data quality issue is best to be fixed at source before importing to Power BI due to performance concern. In this analysis, these flights are kept as is, however, they are showing as `(Blank)` when being analyzed by airports;
- Flight is assumed to be `On-time` if the Arrival Delay is less than 30 minutes in this analysis;


## Skills:
- DAX functions/code to build useful expressions and measures for the data model.
- PowerQuery to ingest and process the data sources;
- Data model design with fact and dimension tables based on reporting requirements;
- Visuals that best assist a user in analyzing this dataset and display insights;
- Visual/Page and Report level filters
- Good use of slicers
- Demonstrate best practices in report design for performance and ability to scale for embedded solutions.
Stretch Goal: 
- Using both DirectQuery and Import data sources to build a composite dimensional model;
- Use of aggregates/aggregation tables to support visual performance;
- Setup for Row Level Security in PBI.

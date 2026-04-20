# MIST4610Group3Project2
## Team Name
Sp26_61608_Group 3
## Team Members
1. Madeleine Ebert [@mee25870](https://github.com/mee25870)
2. Israel Erewa-Meggison [@israelmeggison](https://github.com/israelmeggison)
3. Cole Hauser	[@colehauser2005](https://github.com/colehauser2005)
4. Trey Hill [@treyhill277](https://github.com/treyhill277)
5. Ciara Trinh [@cmt37912](https://github.com/cmt37912)
6. Joshua Welch [@jew22145](https://github.com/jew22145)
## Data Set Explanation 
## Questions
## Snowsight Dashboard
## Streamlit 

# FBI US Crime Data Analytics Dashboard

## 1. Team Name and Members
**Team Name:** [Your Team Name as assigned on eLC]

- [Member 1 Name](https://github.com/member1)
- [Member 2 Name](https://github.com/member2)
- [Member 3 Name](https://github.com/member3)

## 2. Dataset Description
**Provider:** Snowflake Public Data (Cybersyn)  
**Marketplace Listing:** Snowflake Public Data Free  
**Database:** `SNOWFLAKE_PUBLIC_DATA_FREE`  
**Schema:** `PUBLIC_DATA_FREE`  

| Table | Type | Description |
|---|---|---|
| `FBI_CRIME_TIMESERIES` | View | Annual crime incident counts by geography and crime type |
| `FBI_CRIME_ATTRIBUTES` | View | Metadata describing each crime variable |
| `FBI_CRIME_ATTRIBUTES_PIT` | View | Point-in-time history of attributes |
| `FBI_CRIME_TIMESERIES_PIT` | View | Point-in-time history of timeseries values |

**Key columns in `FBI_CRIME_TIMESERIES`:**
- `GEO_ID` (VARCHAR) — geography identifier (e.g. `country/USA`, `geoId/13` for Georgia)
- `VARIABLE` (VARCHAR) — unique variable identifier
- `VARIABLE_NAME` (VARCHAR) — human-readable crime type (e.g. `Annual Count of Incidents, violent crime`)
- `DATE` (DATE) — year-end date for the annual observation
- `VALUE` (NUMBER) — incident count
- `UNIT` (VARCHAR) — unit of measurement (Count)

**Key columns in `FBI_CRIME_ATTRIBUTES`:**
- `VARIABLE` (VARCHAR) — joinable to timeseries
- `VARIABLE_NAME` (VARCHAR) — human-readable name
- `OFFENSE_CATEGORY` (VARCHAR) — FBI crime category
- `MEASURE` (VARCHAR) — what is being measured
- `FREQUENCY` (VARCHAR) — reporting frequency (Annual)

The timeseries table contains data from **1979 through the most recent available year**, covering all 50 US states plus DC and national totals, across 10 crime categories including violent crime, property crime, homicide, robbery, burglary, aggravated assault, larceny, motor vehicle theft, and two definitions of rape.

## 3. Questions and Justification

**Question 1: How have national violent crime and property crime incident counts trended over time in the United States?**

This question is analytically meaningful because it requires filtering to the national geography, selecting two specific crime variables, and examining change across four decades. It is socially significant because long-term crime trends inform policy debates around policing, incarceration, and public safety investment. Columns used: `GEO_ID`, `VARIABLE_NAME`, `DATE`, `VALUE`.

**Question 2: Which states report the highest number of incidents for a given crime type in a given year?**

This question requires filtering by year and crime type, grouping by state geography, and ranking results — it cannot be answered by a single lookup. It is operationally meaningful because it reveals where law enforcement burden is most concentrated and how that shifts across crime categories and years. Columns used: `GEO_ID`, `VARIABLE_NAME`, `DATE`, `VALUE`.

## 4. Data Manipulations

**Question 1:**
```sql
SELECT DATE, VARIABLE_NAME, VALUE
FROM SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.FBI_CRIME_TIMESERIES
WHERE GEO_ID = 'country/USA'
  AND VARIABLE_NAME IN (
      'Annual Count of Incidents, violent crime',
      'Annual Count of Incidents, property crime'
  )
ORDER BY DATE ASC;
```
Filters to national-level data only, selects the two broadest crime categories, and orders chronologically for time series plotting. No joins are required because `VARIABLE_NAME` is denormalized into the timeseries table.

**Question 2:**
```sql
SELECT t.GEO_ID, t.VARIABLE_NAME, t.VALUE
FROM SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.FBI_CRIME_TIMESERIES t
WHERE t.GEO_ID LIKE 'geoId/%'
  AND YEAR(t.DATE) = [selected year]
  AND t.VARIABLE_NAME = '[selected crime type]'
ORDER BY t.VALUE DESC
LIMIT 15;
```
Filters to state-level geographies using the `geoId/` prefix pattern, applies a year filter using `YEAR()`, and limits to the top 15 states by incident count. The FIPS-to-state-name mapping is applied in Python after retrieval.

## 5. Analysis and Results

**Question 1:**

[Insert screenshot of line chart here]

The chart shows that property crime has consistently exceeded violent crime in raw incident counts throughout the entire period from 1979 to present. Both categories peaked in the early 1990s and declined significantly through the 2000s and 2010s, reflecting a well-documented national trend attributed to factors including changes in policing strategy, demographics, and economic conditions. The recent period shows some uptick in certain categories, which aligns with post-pandemic crime reporting patterns discussed in national policy research.

**Question 2:**

[Insert screenshot of bar chart here]

For most crime types, the highest raw incident counts are concentrated in the most populous states — California, Texas, Florida, and New York. This is expected given population size and does not necessarily indicate higher per-capita risk. However, the ranking shifts meaningfully depending on the crime type selected: for homicide, Southern states like Louisiana and Mississippi rank disproportionately high relative to their population, suggesting regional patterns worth further investigation. The year filter reveals that these rankings are relatively stable over time but do shift around major national events.

## 6. Streamlit App

[Insert screenshots of Streamlit app here]

**Interactive Elements:** The app includes two interactive controls for Question 2 — a dropdown to select the crime type (all 8 available offense categories) and a dropdown to select the year (1979–2020). Together they allow the user to explore any combination of crime category and year, producing a new ranked bar chart on each selection.

**Analytical Value:** These filters add genuine analytical value because the ranking of states changes substantially depending on which crime type and year is selected. A user can compare how the state distribution of homicides differs from motor vehicle theft, or track whether a particular state's relative ranking changed before and after a specific year. This kind of cross-dimensional exploration is not possible from a static chart.

**AI Assistance:** Claude (Anthropic) was used to generate the initial Streamlit app code and SQL queries based on the dataset schema. The queries were verified to run correctly in Snowsight. The FIPS-to-state mapping dictionary and pivot logic were generated by Claude and accepted without modification. The written interpretations were drafted by Claude and reviewed by the team.

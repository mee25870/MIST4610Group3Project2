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
## FBI Data Source Rationale
For our project, we selected the FBI Crime dataset from Snowflake's publicly available data. This dataset allows us to query data that is easy to represent graphically and gather visual insight from, given that it contains consistent features like yearly counts across multiple crime categories and geographic levels. The topic of FBI crime statistics sparked our interest, as it is something we are actually somewhat exposed to in our day-to-day lives; crime is a real problem we face, and it is one that we can also collect lots of data from and use to answer deeper questions, making it both relevant and engaging to explore.
## FBI Dataset 
### PUBLIC_DATA.FBI_CRIME_ATTRIBUTES
Snowflake description: Estimated crime statistics for both state and national levels in the US from the FBI. 

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| VARIABLE | VARCHAR | Unique identifier for a variable, joinable to the timeseries table. | annual_count_of_incidents_aggravated_assault |
| VARIABLE_NAME | VARCHAR | Human-readable unique name for the variable. | Annual Count of Incidents, aggravated assault |
| OFFENSE_CATEGORY | VARCHAR | Category of the offense, as defined by the Federal Bureau of Investigation (FBI). | Aggravated Assault |
| MEASURE | VARCHAR | Quantifiable attribute or subject; description of what is being recorded. | Incidents |
| UNIT | VARCHAR | Unit of measurement for the reported value. | Count |
| FREQUENCY | VARCHAR | Frequency of aggregations. | Annual |

---

### PUBLIC_DATA.FBI_CRIME_ATTRIBUTES_PIT
Snowflake description: Tracks history of the fbi_crime_attributes table with start and end timestamps to indicate row validity periods.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| VARIABLE | VARCHAR | Unique identifier for a variable, joinable to the timeseries table. | annual_count_of_incidents_aggravated_assault |
| VARIABLE_NAME | VARCHAR | Human-readable unique name for the variable. | Annual Count of Incidents, aggravated assault |
| OFFENSE_CATEGORY | VARCHAR | Category of the offense, as defined by the Federal Bureau of Investigation (FBI). | Aggravated Assault |
| MEASURE | VARCHAR | Quantifiable attribute or subject; description of what is being recorded. | Incidents |
| UNIT | VARCHAR | Unit of measurement for the reported value. | Count |
| FREQUENCY | VARCHAR | Frequency of aggregations. | Annual |
| _EFFECTIVE_START_TIMESTAMP | TIMESTAMP_TZ | Date and time (in ET) from which a row is valid. | 2024-09-24 12:33:41.372000-04:00 |
| _EFFECTIVE_END_TIMESTAMP | TIMESTAMP_TZ | Date and time (in ET) until which a row is valid. | N/A |

---

### PUBLIC_DATA.FBI_CRIME_TIMESERIES
Snowflake description: Estimated crime statistics for both state and national levels in the US from the FBI.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| GEO_ID | VARCHAR | A unique identifier for a place. Joinable to other geography tables including GEOGRAPHY_INDEX, GEOGRAPHY_RELATIONSHIPS, and GEOGRAPHY_CHARACTERISTICS. | country/USA |
| VARIABLE | VARCHAR | Unique identifier for a variable, joinable to the timeseries table. | annual_count_of_incidents_violent_crime |
| VARIABLE_NAME | VARCHAR | Human-readable unique name for the variable. | Annual Count of Incidents, violent crime |
| DATE | DATE | Date associated with the value. | 1979-12-31 |
| VALUE | NUMBER | Value reported for the variable. | 1208030 |
| UNIT | VARCHAR | Unit of measurement for the reported value. | Count |

---

### PUBLIC_DATA.FBI_CRIME_TIMESERIES_PIT
Snowflake description: Tracks history of the fbi_crime_timeseries table with start and end timestamps to indicate row validity periods.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| GEO_ID` | VARCHAR | A unique identifier for a place. Joinable to other geography tables including GEOGRAPHY_INDEX, GEOGRAPHY_RELATIONSHIPS, and GEOGRAPHY_CHARACTERISTICS. | country/USA |
| VARIABLE | VARCHAR | Unique identifier for a variable, joinable to the timeseries table. | annual_count_of_incidents_violent_crime |
| VARIABLE_NAME | VARCHAR | Human-readable unique name for the variable. | Annual Count of Incidents, violent crime |
| DATE | DATE | Date associated with the value. | 1979-12-31 |
| VALUE | NUMBER | Value reported for the variable. | 1208030 |
| UNIT | VARCHAR | Unit of measurement for the reported value. | Count |
| _EFFECTIVE_START_TIMESTAMP | TIMESTAMP_TZ | Date and time (in ET) from which a row is valid. | 2024-05-21 23:04:34.367000-04:00 |
| _EFFECTIVE_END_TIMESTAMP | TIMESTAMP_TZ | Date and time (in ET) until which a row is valid. | 2024-09-24 15:37:27.010000-04:00 |

---

### PUBLIC_DATA.GEOGRAPHY_INDEX
Snowflake description: Snowflake Public Data unified geographic entity index, joinable to all tables, providing unique geographic identifiers with associated names, levels, and ISO codes for various US geographies.
- This table from the Geography dataset was added in order to connect GeoIDs with state names.

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| GEO_ID | VARCHAR | A unique identifier for a place (a nation, state, zip-code, etc.) | VIR/St.Croix |
| GEO_NAME | VARCHAR | Full name of the place. | St.Croix` |
| LEVEL | VARCHAR | Geographic level or hierarchy (e.g., Country, State, County, City, Continent, CountrySubRegion, etc.) | County |
| ISO_NAME | VARCHAR | Country name from International Organization for Standardization. | Aruba |
| ISO_ALPHA2 | VARCHAR | 2 letter country code from International Organization for Standardization. | QC |
| ISO_ALPHA3 | VARCHAR | 3 letter country code from International Organization for Standardization. | BHS |
| ISO_NUMERIC_CODE | VARCHAR | Numeric code from International Organization for Standardization. | 533 |
| ISO_3166_2_CODE | VARCHAR | Full ISO-3166 code. | ISO 3166-2:BS |

## Questions
**Question 1: How have national violent crime and property crime incident counts trended over time in the United States?**
- The goal of this question is to determine whether the frequency of violent crime and property crime in the United States has trended upward, downward, or stayed the same over the years. Questions like these are important to ask as the data can show us significant patterns that researchers may be interested in investigating further, such as what caused a spike or drop in a specific type of crime. Something like a major societal event (like the COVID-19 pandemic, for example) could be a potential factor or reason for seeing significant changes in crime data, and that is why we must investigate. This question will pull data from FBI_CRIME_TIMESERIES and GEOGRAPHY_INDEX tables, columns include: DATE, VARIABLE_NAME, VALUE, GEO_NAME, LEVEL. 

**Question 2: Which states report the highest number of incidents for a given crime type in a given year?**
- This questions identifies which states report the highest crime incidents for various crime types in a specific year. This data would be of interest to researchers as geographical/regional differences may correlate with increased crime frequency in some states when compared to others. Identifying such differences could help a policymaker determine a better process for dealing with crimes in certain regions. This question will pull data from FBI_CRIME_TIMESERIES and GEOGRAPHY_INDEX tables, columns include: GEO_ID, GEO_NAME, LEVEL, VARIABLE_NAME, VALUE, DATE. 

## Data Manipulations
**Question 1: How have national violent crime and property crime incident counts trended over time in the United States?**
- This query pulls data from two different tables: one containing the actual yearly crime counts and another containing location names, joining them together by a shared location code. The results are then filtered by national-level data and are limited to the two crime categories in question. Sorting by date ascending makes it easy to spot trends over time.  
```sql
SELECT t.DATE, t.VARIABLE_NAME, t.VALUE, g.GEO_NAME, g.LEVEL,
FROM SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.FBI_CRIME_TIMESERIES t
JOIN SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.GEOGRAPHY_INDEX g ON t.GEO_ID = g.GEO_ID
WHERE t.GEO_ID = 'country/USA' AND t.VARIABLE_NAME IN ('Annual Count of Incidents, violent crime','Annual Count of Incidents, property crime')
ORDER BY t.DATE ASC;
```

**Question 2: Which states report the highest number of incidents for a given crime type in a given year?**
- This query pulls crime counts and location names from two joined tables and filters to state-level data (using the PUBLIC_DATA.GEOGRAPHY_INDEX) to match state location codes to state names.  It filters to state-level U.S. geographies (excluding the national USA aggregate) and pulls eight specific crime categories. The data is sorted in descending order, so the states with the highest number of reported incidents are at the top.
```sql
SELECT t.GEO_ID, g.GEO_NAME, g.LEVEL, t.VARIABLE_NAME, t.VALUE, t.DATE
FROM SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.FBI_CRIME_TIMESERIES t
JOIN SNOWFLAKE_PUBLIC_DATA_FREE.PUBLIC_DATA_FREE.GEOGRAPHY_INDEX g ON t.GEO_ID = g.GEO_ID
WHERE t.GEO_ID LIKE 'geoId/%' AND t.GEO_ID != 'country/USA' AND YEAR(t.DATE) = 2020
  AND t.VARIABLE_NAME IN ('Annual Count of Incidents, violent crime','Annual Count of Incidents, property crime','Annual Count of Incidents, homicide','Annual Count of Incidents, robbery','Annual Count of Incidents, burglary','Annual Count of Incidents, motor vehicle theft','Annual Count of Incidents, aggravated assault','Annual Count of Incidents, larceny')
ORDER BY t.VALUE DESC;
```

# FBI US Crime Data Analytics Dashboard


The timeseries table contains data from 1979 through the most recent available year, covering all 50 US states plus DC and national totals, across 10 crime categories including violent crime, property crime, homicide, robbery, burglary, etc.

## Snowsight Dashboard

<img width="1412" height="664" alt="Screenshot 2026-04-27 at 3 03 58 PM" src="https://github.com/user-attachments/assets/2a327ce7-c4f6-4d7d-a583-008ee8418a6c" />

<img width="1115" height="562" alt="Screenshot 2026-04-27 at 3 04 27 PM" src="https://github.com/user-attachments/assets/e7a3ac66-7d05-4ded-b2c5-2d8f43175f96" />

**Chart #1:** This is a graph illustrating the frequency of property crime incidents (Y-axis) from 1979 to the most recent available year (X-axis). It allows us not only to observe the approximate number of property crimes at a specific point in time, but also to notice the overall trend over time of how that number has changed, which we can see has mostly trended downward from approximately 1993 onward.

<img width="1101" height="542" alt="Screenshot 2026-04-27 at 3 04 38 PM" src="https://github.com/user-attachments/assets/cffabea0-8f55-4bcc-9303-b0d672bac530" />

**Chart #2:** This chart shows the frequency of an aggregate of multiple types of incidents (Y-axis), separated by the state in which the crime took place (X-axis). In this case, the chart is able to be manipulated for different purposes (to view different crimes) by changing the Y-axis variable to only certain preferred types of crime (property crime, homicide, etc.) and then individually viewing the crime frequencies of those categories within the different states (Results can even be filtered further by only including incident counts within a specific year to give us even more details and free will over the data). The highest results in a chart like this tend to favor states like California, as shown, due to their larger populations.


## Analysis and Results

**Question 1:**

<img width="946" height="419" alt="Screenshot 2026-04-27 at 2 20 55 PM" src="https://github.com/user-attachments/assets/c0c6673c-9185-4426-a83c-86068d19957c" />

The chart shows that property crime has consistently exceeded violent crime in raw incident counts throughout the entire period from 1979 to the present. Both categories peaked in the early 1990s and declined significantly through the 2000s and 2010s, reflecting a well-documented national trend attributed to factors including changes in policing strategy, demographics, and economic conditions. The recent period shows some uptick in certain categories, which aligns with post-pandemic crime reporting patterns discussed in national policy research.

**Question 2:**

<img width="900" height="581" alt="Screenshot 2026-04-27 at 2 24 53 PM" src="https://github.com/user-attachments/assets/916d0f45-2c42-41b1-b1cd-3dba2f04260c" />

For most crime types, the highest raw incident counts are concentrated in the most populous states (California, Texas, Florida, and New York). This is expected, given the population size, and does not necessarily indicate higher per-capita risk. However, the ranking shifts meaningfully depending on the crime type selected: for homicide, Southern states like Louisiana and Mississippi rank disproportionately high relative to their population, suggesting regional patterns worth further investigation. The year filter reveals that these rankings are relatively stable over time, but do shift around major national events.

## Streamlit App

<img width="1016" height="687" alt="Screenshot 2026-04-27 at 2 28 31 PM" src="https://github.com/user-attachments/assets/a821bb9c-46f7-4ac2-b974-d845da2ab18a" />

<img width="928" height="587" alt="Screenshot 2026-04-27 at 2 30 14 PM" src="https://github.com/user-attachments/assets/c138426b-9e04-45fb-afab-e45a92b139be" />

<img width="910" height="585" alt="Screenshot 2026-04-27 at 2 30 29 PM" src="https://github.com/user-attachments/assets/ba1dcece-ef32-4b65-a397-016ec553ae10" />

**Interactive Elements:** The app includes two interactive controls for Question 2: a dropdown to select the crime type (violent crime or property crime) and a dropdown to select the year (1979–2020). Together, they allow the user to explore any combination of crime category and year, producing a new ranked bar chart on each selection.

**Analytical Value:** These filters add genuine analytical value because the ranking of states changes substantially depending on which crime type and year is selected. A user can compare how the state distribution of violent crime differs from that of property crime, or track whether a particular state's relative ranking changed before and after a specific year. This kind of cross-dimensional exploration is not possible from a static chart.

**AI Assistance:** Claude (Anthropic) was used to generate the initial Streamlit app code and SQL queries based on the dataset schema. The queries were verified to run correctly in Snowsight. The FIPS-to-state mapping dictionary and pivot logic were generated by Claude and accepted without modification. The written interpretations were edited by Claude for grammar and correctness.


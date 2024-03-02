# Nobel prizes analysis (SQL)
Analysis of The Nobel Foundation dataset containing information about all Nobel prize winners from 1901 till 2023. Let's check for some records and interesting facts. This project is purely SQL without visualizations.

### Data origin and processing
Source of the used data: https://www.kaggle.com/datasets/shayalvaghasiya/nobel-prize-data

Data was prosessed in Jupyter notebook using [sqlite3](https://docs.python.org/3/library/sqlite3.html) and cell magic **%%sql**

All the queries (without outputs) can be found in file: [SQL_Nobel_prize_analysis.ipynb](SQL_Nobel_prize_analysis.ipynb)  
You are welcome to download the notebook, nobel.csv file, then run all the cells and explore the data yourself by editing the queries.  
You can also check or download larger notebook containing all the outputs: [SQL_Nobel_prize_analysis_with_outputs.ipynb](SQL_Nobel_prize_analysis_with_outputs.ipynb) 

## Introduction
The Nobel Prize is arguably the most famous award globally. Established in 1901, it honors outstanding achievements in Physics, Chemistry, Physiology or Medicine, Literature, and Peace. Economics was added in 1969. You can learn more on the [Nobel Prize website](https://www.nobelprize.org/) or [Wikipedia](https://en.wikipedia.org/wiki/Nobel_Prize).

This analysis focuses mainly on the age, birthplace, and work location of Nobel laureates. It even explores whether a specific birth date or month increases your chances of winning!

## Data Review and Cleaning
Before delving into fascinating insights extracted through SQL queries, let's discuss the data quality. Examining data is crucial in any analysis to identify any necessary cleaning or transformations.

The Nobel Prize dataset is relatively small, allowing easy exploration of the available data in each column. We then check if the data types are correct and if any values are missing. For numerical data, calculating minimum, maximum, average, and median helps reveal potential errors.

Upon initial inspection, the data appeared clean and usable. However, two key issues required attention.

**1. Date Format:** SQLite, the database engine used, has limited [data types](https://www.sqlite.org/datatype3.html), lacking a specific one for dates and times. Dates are stored as text, real numbers, or integers. This can lead to seemingly valid dates that are unusable in calculations.

In the Nobel dataset, some birth dates had only zeros for the day and month (e.g., "1999-00-00"). This format is invalid and incompatible with [date and time functions](https://www.sqlite.org/lang_datefunc.html). We identified these problematic dates by running a date function and filtering for null values in the result.

```sql
SELECT
  laureate_id,
  birth_date,
  strftime('%Y',birth_date) AS birth_strf
FROM nobel
WHERE birth_date IS NOT NULL AND birth_strf IS NULL;
```
These problematic rows weren't included in the analysis of birth date, month, and day of the week. We then changed them to July 1st (mid-year) for calculations of average age and other statistics. While omitting them entirely might be argued as a better approach, this method likely introduced minimal error as the birth year remained accurate.

**2. Missing Values:** Multiple columns contained missing values. However, closer examination revealed a reasonable explanation for most of them.

- **Motivation:** As of March 2024, 1000 Nobel Prizes have been awarded. Information about the motivation was missing for nearly 90 laureates because it wasn't announced for the Peace Prize until the 1990s.
- **Sex and Birth Information:** This data is absent for organizations awarded the prize.
- **Organization Details:** These columns are meant to capture the organization where an individual laureate conducted their work, not the organization receiving the prize. Therefore, missing values here are expected for laureates not associated with any organization, which commonly applies to Literature and Peace Prize laureates.
- **Death Information:** The majority of missing values in death-related columns likely correspond to organizations or still-living laureates.

## Results and Discussion





Text of the report was improved and grammar corrected by [Google Gemini](https://gemini.google.com/).


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
### Number of Nobel Prizes for laureates, countries and organizations
First of all, a quick and almost mandatory statistic, which laureates has won the highest number of Nobel Prizes? One organization and five individuals won two Nobel Prizes and only one of them is still living. Absolute record of three Nobel Prizes belongs to Red Cross which obtained all of them for Peace (1917, 1944 and 1963).

this is a placeholder for Figure1

Now let's focus on countries of birth. There is a clear winner as 291 laureates was born in the USA and only 91 in the United Kingdom which is the second most succesfull and third is Germany with 67 laureates. From other point of view, most of the categories of Nobel Prize are related to scientific fields and it is very common for scientists to work in other countries than where they were born. In the Nobel dataset there is 464 laureates who obtained Nobel Prize with organization in the same country where they have been born but in the case of 271 lauretes their organization was in different country then where they were born (similar number of laureates is not affiliated to any organization). Therefore, it is interesting how looks the numbers of Nobel Prizes based on the organization country. Following table shows number of Nobel Prizes per country based on laureate organization (nr_prizes_org), place of birth (nr_prizes_birth), and difference of the numbers. Surprisingly, there are only three countries for which organizations was affiliated more laureates than number of laureates born in the country. Most successful is USA, 385 laureates worked at US based organizations while "only" 291 laureates were born in the country, that makes positive balance of 94 Nobel prizes. Switzerland is second with 24 prizes regarding to organizations while 5 less laureates were born in Switzerland. Finally, United Kingdom has positive balance by 2 Nobel Prizes. Obviously, it is interesting to look at the bottom of the table. Russia, Germany, and France have the worst negative balance as the laureates affiliated to organizations in those countries obtained 15, 18, and 20 prizes less, respectively, than how many laureates was born in these countries. Note, that table contains only countries with at least one record in each of first two columns.

this is a placeholder for Figure2

Generally, it seems that higher number of laureates coming from countries which are generally known as rich, developed, and with available high quality education. Now, we can check if world famous universities such as Cambridge and Oxford from UK or Harvard and MIT from USA can stand to their famous names when it comes to number of Nobel Prizes obtained by their scientists. There are in total 12 organizations which are related to 10 or more Nobel prizes, three of them are from UK and the rest from USA. In total, scientist from those 12 organizations obtained 228 Nobel Prizes which is almost a quater of all Nobel Prizes. The most successful is University of California with 36 laureates, followed by Hardard, Stanford, MIT and Chicago. The most successful UK University is Cambridge with 17 laureates.

this is a placeholder for Figure 3

To finish this part on personal note, I am from Czech Republic; therefore I was interested how many laureates was affiliated or born in Czechia. There are six laureates, all born in area of current Czech Republic. Two of them were women, only one laureate is still alive and was awarded in 2007, except the economy the prizes went to all possible categories (two for medicine and one for Peace, Literature, Chemistry, and Physics). I encourage you to use the notebook and edit the cell to find out more about laureates from your country.

this is a placeholder for Figure 4

### Birth dates statistics
Many people believe in astrology or are in some other way superstitious. That is not my case but we can check anyway if being born on specific date, months or day of a week makes you more likely to win a Nobel Prize. First, lets look which day of a year is the most lucky and the answer is 28th Fabruary when 8 laureates were born. Then seven laureates was born on three different dates: 21st May, 28th June, and 10th October. There are many dates on which six or five laureates were born and rather then listing them we can check if there is a day in any month which is more lucky. Indeed on 23rd was born 42 laureates while being born on 17th seems unlucky with only 17 laureates. Statistics for months are more interesting. It is no surprise that the least laureates was born in the shortest month, February. However, the difference from other months is too large to be explained by 2-3 missing days in comparison to other months. Clearly, we would have to take in count statistics for birth rate to find out if there is born statisticaly more Nobel laureates in any month. But let's take it easy and look the numbers as they are. Most of laureates was born in June (90) and September (91) which have actually only 30 days. Finally, how does compare days of a week? While only 121 laureates was born on Sunday, Saturday is the luckiest with 165 laureates which is 20 more then on second best day, Tuesday. However, are any of these days and dates really more lucky? I will answer it under the tables showing current statistics for months and days of a week. 

this is a placeholder for Figure 5 and 6

At the first sight the differences discussed above may seem to show that probability to get Nobel Prize can be really related to specific date, month or day. Nevertheless, there is bit less then 1000 laureates with birth date and that is not enough to clearly distiguish randomness from a systematic trend. If we assume all days to be equal and try to simulate the same statistics by drawing a 1000 random values we will always end up with very similar statistics which we see in Nobel dataset (draw was simulated and plotted using python and seaborn, an example of one out of ten draws for day of week is on figure below). However, if we repeat such a draw thousand times then the differences between different dates will become insignificant. Thereofore, currently lucky looking days are just a result of randomness and we would need at least hudred times larger number of laureates to find any discrepancies from expected uniform distribution.

this is a placeholder for Figure 7

### Statistics on age of laureates
Developing your knowledge, conducting research and testing new ideas takes time and even more time is often needed for society to recognize outstanding scientific work with large influence. Hence, it is not suprise that in most categories Nobel laureates are awarded at very high age. We will take deeper look on statistics related to laureates age. First of all, lets check who were the earliest and currently the latest born Nobel laureates. The earliest born was a writer C.M.T. Mommsen at 1817 and awarded at 1902, while latest born was M. Yousafzai at 1997 and awarded at 2014 with Peace Nobel Prize.

this is a placeholder for Figure 8

Next we will look, what was the average age of laureates when they obtained the prize. First way to look it is by category and then also how it was developing through the decades. Following tables shows that laureates in economics are the oldest. However, Nobel Prize in economics was established in the end of 1960s when the average age of laureates was generally higher as can be seen from the second table. Therefore better not to directly compare economy to other older categories. Nevertheless, it seems that laureates in literature are clearly older than laureates in other categories. The second table with averages for each decade clearly shows that average age was growing in time.  

this is a placeholder for Figure 9 and 10

As discussed earlier, some Nobel Prize categories are clealy scientific while peace and literature are not related to scientific research. Therefore, it could be interesting to seem how is changing average age throughout decades within each category. Indeed, we can see significant increase in average age at all scientific categories except economics which has average age between 65 and 70 in all decades of its existence. Average age of laureates in literature seems to be higher in each decade than average for any other category - except peace - and we can see that values gradually increasing as in the scientific categories since 1930s. Peace Nobel Prize is a bit outlier as the age of laureates was oscillating throughout decades, sometimes the average age in the category was the heighest and sometimes the lowest of all categories.

this is a placeholder for Figure 11

We were quite thorough in checking average age of laureates when they were awarded and now we can check some records. So, who were five oldest and youngest laureates in the history of Nobel Prize? Interestingly, in both categories there are three laureates in physics. While all the oldest laureates were awarded rather recently, the youngest laureates shows striking difference regards to the category, physicists were awarded in 1915 and 1930s but laureates in category of peace were awarded in previous decade (2010s).

this is a placeholder for Figure 12 and 13

So far, we were looking at the age in the moment of getting the Nobel Prize and now we can check some facts related to length of life. First, lets check which laureates died the youngest. This sad record holds M.L. King who was assassinated and died 39 years old, four years after obtaining Peace Nobel Prize. N.R. Finsen suffered from Niemann–Pick disease and died due to multiple health issues. A. Camus died in a car accident and P. Currie similarly in a street collision when he slipped and fell under a heavy horse-drawn cart. C.v. Ossietzky died of tuberculosis after years of mistreatment and torture by Nazi regime in Germany.

this is a placeholder for Figure 14

To switch on bit more positive note, here are five laureates who died in the highest age. The oldest was R.Levi-Montalcini who died at age of 103 years. 

this is a placeholder for Figure 15

Keeping in mind that our initial data exploration revealed large number of missing values at death date column which means that significant amount of laureates are still alive or that Nobel dataset is not properly updated in regards to death dates. Now, the question is, who are the five oldest currently living Nobel laureates? Interestingly, it seems that except the fifth one, they are all older than the oldest laureate in the previous table. Unfortunately, after brief check, I have to inform you, that the Nobel dataset does not seems to be frequently updated with death dates. All of the laureates in the table below are alive based on the dataset but in real they all deceased between 2017 and 2021. Therefore, we can estimate they all lived roughly 100 years.

this is a placeholder for Figure 16

Although, we cannot rely on the data when it comes to missing death dates, there is one more statistic which was originally influenced by this problem but I have removed two names based on manual check if the laureates are alive. In the last table, we can see, who enjoyed the longest being a Nobel laureate. In other words, who had the longest period between getting the award and their death or a current date for still-living laureates. The record holds Chen Ning Yang and Tsung-Dao (T.D.) Lee who were awarded in 1957, almost 67 years ago. James Dewey Watson was awarded almost 62 years ago and Louis de Broglie 58 years after receiving Nobel Prize. 

this is a placeholder for Figure 17

## Conclusion
In this work, we employed SQLite to analyse Nobel Prize Dataset. Data was imported from csv file and database was created. Then we checked data if any cleaning is necessary. This was done by focusing on birth and death dates format and also by checking count of missing values in each column. Mostly, there was simple and clear explanations for missing values but there were some faulty birth dates which we substituted (updated) for first of July. In the analysis, the main focus was on number of laureates and organizations awarded in different countries and later we calculated various statistics related to age. The effort to find the oldest currently living laureate helped us to find out that death dates in the Nobel dataset are often missing.

In summary, we can say that USA is the most successful country from the point of laureates born or working in there. Also, it seems that laureates are usually awarded in rather older age and quite a number of them lived very long lives. 

-----
Text of the report was improved and grammar corrected with the help of [Google Gemini](https://gemini.google.com/).


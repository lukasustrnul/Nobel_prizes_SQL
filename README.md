# Nobel Prizes Analysis (SQL)
Analysis of The Nobel Foundation dataset containing information about all Nobel prize winners from 1901 till 2023. Let's check for some records and interesting facts. This project is purely SQL without visualizations.


### Data Origin and Processing
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
### Number of Nobel Prizes for Laureates, Countries and Organizations

**Laureates with the Most Prizes:**
Let's begin by exploring individuals and organizations with the most Nobel Prizes. Interestingly, one organization and five individuals have each received two Nobel Prizes, with only one of them still alive. The absolute record holder is the Red Cross, awarded all three of its prizes for Peace in 1917, 1944, and 1963.

<img src="/tables_jpg/figure01.jpg" width="100%">

**Number of Prizes by Country of Birth:**
Shifting our focus to countries, the United States clearly leads the pack with 291 laureates, followed by the United Kingdom with 91 and Germany with 67. However, it's important to consider that many scientific fields represented by Nobel Prizes involve international collaboration.

The dataset reveals 464 laureates who received their prize while working in an organization located in their birth country, while 271 received their prize in a different country. A significant number of laureates (similar to the previous figure) are not affiliated with any organization, typically those in Literature and Peace categories. This raises the question: how do the number of Nobel Prizes differ based on the organization's country instead of laureates birth?

The following table presents the number of Nobel Prizes per country based on the laureate's organization (nr_prizes_org), place of birth (nr_prizes_birth), and the difference between these numbers. Surprisingly, only three countries have a higher number of Nobel Prizes in terms of organizations than the number of laureates born in those countries. Furthermore, the table (available only for countries with records in both the first two columns) reveals that Russia, Germany, and France have the most negative balance. Laureates affiliated with organizations in these countries received 15, 18, and 20 fewer prizes, respectively, compared to the number of laureates born in those countries.

<img src="/tables_jpg/figure02.jpg" width="50%">

**Universities and Nobel Prizes:**
It's natural to wonder how world-renowned universities like Cambridge, Oxford, Harvard, and MIT fare when it comes to the number of Nobel Prizes obtained by their scientists.

Among the 12 organizations associated with 10 or more Nobel Prizes, all but three are from the United States. These 12 institutions boast a remarkable 228 Nobel Prizes, which is nearly a quarter of all Nobel Prizes awarded. The University of California leads the pack with 36 laureates, followed by Harvard, Stanford, MIT, and Chicago. Notably, the most successful UK university is Cambridge with 17 laureates.

<img src="/tables_jpg/figure03.jpg" width="50%">

**A Personal Note:**
As someone from the Czech Republic, I was curious about the number of laureates associated with my country. The dataset reveals six laureates, all born within the current Czech Republic. Interestingly, two were women, and only one laureate, awarded in 2007, is still alive. These laureates represent all Nobel Prize categories except Economics, with two in Medicine and one each in Peace, Literature, Chemistry, and Physics.

I encourage you to explore the notebook and modify specific cell to learn more about laureates from your own country.

![Figure 4](/tables_jpg/figure04.jpg 'Figure 4')

### Birth dates statistics
Many people believe in astrology or other superstitions. That is not my case but we can we can still explore whether birth dates influence Nobel Prize chances. 

**Lucky Birthdays?**
February 28th seems "luckiest" with eight laureates born on that day. Three other dates tie for second with seven each: May 21st, June 28th, and October 10th. Listing numerous dates with five or six laureates is less informative than looking at trends within each month. Interestingly, the 23rd of any month seems luckiest, with 42 laureates born on that day. Conversely, the 17th appears less fortunate, with only 17 laureates.

**Monthly Distribution:**
Unsurprisingly, February has the fewest laureates due to its shorter length. However, the difference from other months is too substantial to be solely attributed to the missing days. Ideally, we would need to consider birth rate statistics to determine if this truly indicates a statistical difference in Nobel laureates born in each month. Regardless, June (90) and September (91) have the most laureates, despite having only 30 days.

<img src="/tables_jpg/figure05.jpg" width="25%">

**Days of the Week:**
While only 121 laureates were born on Sunday, Saturday is the "luckiest" with 165, 20 more than the second-best day, Tuesday. 

<img src="/tables_jpg/figure06.jpg" width="25%">

**Are These Trends Real?**
While the above differences might seem significant, with less than 1,000 laureates with birth dates, it's not enough to definitively distinguish randomness from a pattern.

Imagine all days are equally likely and simulate the same statistics by randomly drawing 1,000 values. We will likely get similar statistics to the Nobel dataset (an example simulation for the day of the week is shown below). However, repeating such a draw thousands of times will reveal insignificant differences between dates.

Therefore, currently "lucky" days are likely just a result of chance. We would need hundreds of times more laureates to find any deviations from an even distribution.

![Figure 7](/tables_jpg/figure07.jpg 'Figure 7')

### Statistics on Age of Laureates
Developing knowledge, conducting research, and testing ideas take time. Recognizing outstanding scientific work with significant influence often takes even longer. Therefore, it's unsurprising that Nobel laureates in most categories are awarded the prize at a relatively old age. Let's delve deeper into the statistics related to laureate age.

**Earliest and Latest Born Laureates:**
The earliest-born Nobel laureate was writer C.M.T. Mommsen, born in 1817 and awarded the prize in 1902. The latest-born is Malala Yousafzai, born in 1997 and awarded the Peace Prize in 2014.

![Figure 8](/tables_jpg/figure08.jpg 'Figure 8')

**Average Age at Award:**
The first table below shows the average age of laureates by category when they received the prize. The second table shows this average age across decades. We can see that:

- **Economics laureates** are the oldest. However, the Nobel Prize in Economics was established in the late 1960s, when the average laureate age was generally higher, as shown in the second table. Therefore, directly comparing them to older categories might be misleading.
- **Literature laureates** are consistently older than laureates in other categories.
- The **average age** has increased over time.

<img src="/tables_jpg/figure09.jpg" width="25%">

<img src="/tables_jpg/figure10.jpg" width="25%">

**Average Age by Category and Decade:**
As mentioned earlier, some Nobel Prizes are scientific, while Peace and Literature are not directly related to scientific research. It's interesting to see how the average age within each category has changed over time.

The data reveals a significant increase in average age for all scientific categories except Economics. In contrast, the average age of literature laureates remains consistently higher than any other category (except Peace) and has gradually increased since the 1930s. The Peace Prize is an outlier, with the average age fluctuating throughout decades, sometimes being the highest and sometimes the lowest of all categories.

<img src="/tables_jpg/figure11.jpg" width="50%">

**Oldest and Youngest Laureates:**
After examining average ages, let's explore some exceptional cases. Interestingly, both the five oldest and youngest laureate categories include three physicists!

- **Oldest Laureates:** All the oldest laureates were awarded relatively recently.
- **Youngest Laureates:** These individuals showcase a striking difference in categories. The physicists were awarded in 1915 and the 1930s, while the Peace Prize laureates received their awards in the 2010s.

![Figure 12](/tables_jpg/figure12.jpg 'Figure 12')
![Figure 13](/tables_jpg/figure13.jpg 'Figure 13')

**Lifespan:**
So far, we were looking at the age in the moment of getting the Nobel Prize and now we can check some facts related to length of life. Who had the shortest lifespan? This sad record holds Martin Luther King Jr., who was assassinated at the young age of 39, just four years after receiving the Peace Prize. Niels Ryberg Finsen Finsen suffered from Niemann–Pick disease and passed away due to multiple health issues. Albert Camus and Pierre Curie both died tragically in accidents. Carl von Ossietzky succumbed to tuberculosis after enduring mistreatment and torture under the Nazi regime in Germany.

![Figure 14](/tables_jpg/figure14.jpg 'Figure 14')

On a bit more positive note, here are five laureates who died in the highest age. Rita Levi-Montalcini lived the longest, reaching the remarkable age of 103.

![Figure 15](/tables_jpg/figure15.jpg 'Figure 15')

It's crucial to acknowledge the limitations of the data. Our initial exploration revealed a significant number of missing values in the death date column. This could indicate that many laureates are still alive, or that the Nobel dataset might not be consistently updated with death information.

**Five "Oldest Living" Laureates (based on the dataset):**
While the data suggests they are mostly older than the oldest laureate in the previous table, further investigation revealed they all passed away between 2017 and 2021. Therefore, we can estimate their lifespans to be around 100 years. Clearly, the Nobel dataset provided misleading information.

![Figure 16](/tables_jpg/figure16.jpg 'Figure 16')

Although, we cannot rely on the data when it comes to missing death dates, there is one more statistic which was originally influenced by this problem but I have removed two names based on manual check if the laureates are alive. The last table shows individuals who enjoyed the longest period between receiving the Nobel Prize and their death (or the current date if still alive). Chen Ning Yang and Tsung-Dao Lee hold the record, having been awarded the prize in 1957, nearly 67 years ago. James Dewey Watson was awarded almost 62 years ago and Louis de Broglie lived 58 years after receiving Nobel Prize. 

![Figure 17](/tables_jpg/figure17.jpg 'Figure 17')

## Conclusion
This analysis examined the Nobel Prize dataset, ensuring data consistency and addressing missing values. We explored laureates and awarding organizations by country, and calculated age-related statistics. Notably, identifying the oldest living laureates revealed frequent gaps in death date information.

In summary, the USA leads in the number of laureates born or working there. Additionally, laureates are typically awarded the prize at a later age, and many enjoy long lifespans.

-----
The report's clarity and grammar were enhanced with the assistance of [Google Gemini](https://gemini.google.com/).


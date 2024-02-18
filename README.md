# Nobel prizes analysis (SQL)
Analysis of The Nobel Foundation dataset containing information about all Nobel prize winners from 1901 till 2023. Let's check for some records and interesting facts. This project is purely SQL without visualisations.

### Data origin and processing
Source of the used data: https://www.kaggle.com/datasets/shayalvaghasiya/nobel-prize-data

Data was prosessed in Jupyter notebook using [sqlite3](https://docs.python.org/3/library/sqlite3.html) and cell magic **%%sql**

All the queries (without outputs) can be found in file: SQL_Nobel_prize_analysis.ipynb  
You are welcome to download the notebook, nobel.csv file, then run all the cells and explore the data yourself by editing the queries.

## Introduction
Nobel Prize is likely the most well-known award in the world. First time they were awarded in 1901 in the fields of Physics, Chemistry, Physiology or Medicine, Literature, and Peace. In the 1969 was established additional prize in Economics. You can read more at [Nobel Prize web](https://www.nobelprize.org/) or at [Wikipedia](https://en.wikipedia.org/wiki/Nobel_Prize).

Before we dive into interesting facts, which I was able to distill from available dataset using SQL, there are some information and observations to share. First part of every analysis is to get overview of the data and check if any cleaning or trasformations are necessary. The Nobel Prize dataset is rather small; therefore, easy to scroll through and observe what kind of data are available in each column. Next step is typically to check minimum, maximum, average and median for numerical columns as these values can indicate faulty values a column.


# Video Game Sales
![](https://github.com/TacoBadger/Video-Game-Sales/blob/main/dataset.png?raw=true)

# Analysis of the Video Game Sales in Kaggle
Worked with the dataset to find trends and patterns. This is a public data made available through Kaggle. Please do take note that this document is for practice purposes only and it does not include any marketing of the titles involved.

## Dataset
We will manually upload the Dataset but you can also access the dataset in Kaggle.

- [Video Game Analysis](https://www.kaggle.com/datasets/gregorut/videogamesales)

## Author
- [@TacoBadger](https://github.com/TacoBadger)

## Methods
- [Motivation](#motivation)
- [Looking for trends and patterns](#looking-for-trends-and-patterns)
- [Important Definitions](#important-definitions)
- [Importing the dataset from Kaggle to see what tables we have in the dataset](#importing-the-dataset-from-kaggle-to-see-what-tables-we-have-in-the-dataset)
- [Checking unique values and columns](#checking-unique-values-and-columns)
- [Let's do some Data Analysis and Groupings](#lets-do-some-data-analysis-and-groupings)
- [Extra Analysis](#extra-analysis)
- [Our Query Run Order and Functions](#our-query-run-order-and-functions)
- [Exporting the tables for Tableau](#exporting-the-tables-for-tableau)
- [Key Findings](#key-findings)
- [Explore our Tableau Public](#explore-our-tableau-public)
- [Explore our Notebook](#explore-our-notebook)
- [What is Next?](#what-is-next)

## Motivation
We will now use Microsoft SQL to practice my Data Analysis with this public dataset,again it is okay to work with just one tool but for us we will be practicing in different tools to expand our knowledge, it is not required but you can always choose yourself.

This practice is done with the use of [Microsoft SQL Management Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads). Microsoft SQL Server is a relational database management system (RDBMS) that supports a wide variety of transaction

We will also be practicing our [Tableau](https://public.tableau.com/en-us/s/) dashboard skills with this dataset.

## Looking for trends and patterns
So here the few things I want to know in this dataset:
- The total production of genres.
- The top selling games worldwide.
- Top Companies to produce Games.
- Games Released by Year.

## Importing the dataset from Kaggle to see what tables we have in the dataset
I always start of to downloading the [dataset](https://www.kaggle.com/datasets/gregorut/videogamesales)
it will always appear as a zip file but you can extract and import it.

I have mentioned we used Microsoft SQL for this one so here are the links to get started with that software and a small tutorial.
- [Video tutorial](https://www.youtube.com/watch?v=NLhu4QvFPsU&list=PL3QIj0dCAeARFktUaoy1I4C3qvpeptSjK&index=2)
- [SQL Management Server](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)
- [Developer SQL Server 2019](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

You can follow the tutorial to create your own database and import the file. I imported my file in a Database called ProjectDB by now you have checked the tutorial and you can now follow along.

You might also encounter an error while importing the file. So after few hours of trying and researching, I have changed the appropriate datatype for each of the columns and dropped some the null values which is okay since we still have valuable data for our analysis.

Data types in the vgsales dataset:
- Rank - smallint
- Name - nvarchar(MAX) - This is to make sure there is no limit of characters in names and same goes for other nvarchar(MAX) columns.
- Platform - nvarchar(MAX)
- Year - smallint
- Genre - nvarchar(MAX)
- Publisher - nvarchar(MAX)
- NA_Sales - float
- EU_Sales - float
- JP_Sales - float
- Other_Sales - float
- Global_Sales - float

It may show a warning that a few rows have been dropped but it is because some of the null rows have been dropped.
Then you create your own database and follow the tutorial on how to import the file and follow this [tutorial](https://www.youtube.com/watch?v=0xINwDcFLSk&list=PL3QIj0dCAeARFktUaoy1I4C3qvpeptSjK&index=3) if you ever get stuck.

Double check to make sure you have the same fields too, so you don't encounter any problems or errors too. 

As soon as you imported the data you can get an overview by doing the common query **SELECT** and **FROM**.
Extras:
- Top selling games in North America, Europe, Japan and Other countries.

## Important Definitions
**SQL Server**, often pronounced 'Sequel Server,' is not a bad sequel to an already bad movie. Far from it: It is a very powerful system that can be used in organizations of varying sizes, from small businesses to major corporations.

SQL Server is Microsoft's relational database management system. The key operating word here is system; the system's function is to manage multiple databases. It also provides a suite of tools that help to build, change, and manage the data. Additionally, there are tools for report writing, data import/export, and data analysis tools.

We will perform the following common queries:
- sorting and filtering
- creating tables for exporting in tableau
- common queries for grouping

**Tableau Public** is a free platform to explore, create and publicly share data visualizations online. With the largest repository of data visualizations in the world to learn from, Tableau Public makes developing data skills easy.

Let's start with importing the dataset.

I used my own database and imported it and now we can start to get an overview with simple query.

```bash
# create your own database
CREATE DATABASE ProjectDB

# to use your own database
use ProjectDB

# inspecting the data
select * from [dbo].[vgsales]
```

The query will return 16,328 rows with 11 columns.

## Checking unique columns and values
Checking the rows columns for unique variables that can be really nice to plot.

In this example we will select unique values and check columns we want to work with.

The **SELECT DISTINCT** statement is used to return only distinct (different) values.

Inside a table, a column often contains many duplicate values; and sometimes you only want to list the different (distinct) values.

```bash
select distinct Name from [dbo].[vgsales]

# you should get 11,493 rows with the name of the games in the dataset
```

```bash
select distinct Year from [dbo].[vgsales]

# you will get 40 rows with one null, so we will remove the null with

delete from [dbo].[vgsales] where Year IS NULL

# you will get a total of 39 rows with years
```

```bash
select distinct Genre from [dbo].[vgsales]

# 12 rows of genre
```
| Genre        	    |           	
|-------------------|
| Puzzle           	|
| Shooter           |
| Platform          |
| Strategy          |
| Misc              |
| Adventure         |
| Racing            |
| Role-Playing      |
| Fighting          |
| Action            |
| Sports            |

```bash
select distinct Publisher from [dbo].[vgsales]

# 577 rows of publisher
```
You can now get a better overview of the columns we need. The data is quite easy to work with and we can also get started with the analysis too.

## Let's do some Data Analysis and Groupings
Let's start grouping them by:
- SALES
- YEAR
- GAMES
- GENRE
- COMPANIES

Start with the most basic query with SELECT and FROM. In this analysis we will also use GROUP BY and COUNT.
- COUNT(ALL expression) evaluates expression for each row in a group, and returns the number of nonnull values.
- The GROUP BY clause is an optional clause of the SELECT statement. The GROUP BY clause a selected group of rows into summary rows by values of one or more columns. The GROUP BY clause returns one row for each group.

Let's start to group them by **"Which genre has been produced the most?"**
```bash
select Genre, count(Genre) as Produced
from [dbo].[vgsales]
group by Genre
order by 2 desc # the 2 in this order by means to order them by total counts of genres
```
This will show 12 rows with **action**** as the top genre **3,253** are classified as action games followed by **sports** with **2,305** games and the least to be **puzzle** with **571** games.
| Genre        	    | Number        	    |            	
|-------------------|-------------------|
| Action           	| 3253 |
| Sports            | 2305 |
| Misc              | 1710 |
| Role-Playing      | 1471 |
| Shooter           | 1282 |
| Adventure         | 1276 |
| Racing            | 1226 |
| Platform          | 876  |
| Simulation        | 851  |
| Fighting          | 836  |
| Strategy          | 671  |
| Puzzle            | 571  |

Then we wanted to group them by **"Which year has the most game release?"**
```bash
select Year, count(Global_Sales) as Total_Sales
from [dbo].[vgsales]
group by Year
order by 2 desc
```
This query will return 39 rows showing **2009** is the year with the most game released with a total of **1,431** games released in the public followed by **2008** with **1,428** games released. The year **2009** is the golder period of gamers.

Then let's group the **top 20 companies to release the most games.**
```bash
select TOP 20 Publisher, count(Name) as Games
from [dbo].[vgsales]
group by Publisher
order by 2 desc
```
The query will return 20 rows with **Electronic Arts** on top with **1,340** games produced, I do love their Sims game and it is quite known and famous! So no wonder Electronic Arts are on top followed by **Activision** with **966** games released and **Microsoft Game Studios** with the least games released.

Then lastly we wanted to group them by the **top 10 games most sold worldwide!**
```bash
select TOP 10 Name, sum(Global_Sales) as Global_Total
from [dbo].[vgsales]
group by Name
order by 2 desc
```
The query will return 10 rows with **Wii Sports** on top with sales about **8274 million!** I've also wanted to try wii sports rather than going to the gym! Followed by **Super Mario Bros** with **4531 million sales!** which is kind of really popular to twitch streamers and for multiplayers!

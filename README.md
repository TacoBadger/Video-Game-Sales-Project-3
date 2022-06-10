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
We will now use Microsoft SQL to practice my Data Analysis with this public dataset, again it is okay to work with just one tool but for us we will be practicing in different tools to expand our knowledge, it is not required but you can always choose yourself.

This practice is done with the use of [Microsoft SQL Management Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads). Microsoft SQL Server is a relational database management system (RDBMS) that supports a wide variety of transaction

We will also be practicing our [Tableau](https://public.tableau.com/en-us/s/) dashboard skills with this dataset.

## Looking for trends and patterns
So here the few things I want to know in this dataset:
- The total production of genres.
- The top selling games worldwide.
- Top Companies to produce Games.
- Games Released by Year.

Extras:
- Top selling games in North America, Europe, Japan and Other countries.

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
This will show 12 rows with **action** as the top genre **3,253** are classified as action games followed by **sports** with **2,305** games and the least to be **puzzle** with **571** games.
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
| Name        	    | Sales        	    |            	
|-------------------|-------------------|
| Wii Sports           	| 8274 |
| Super Mario Bros.            | 4531 |
| Grand Theft Auto V              | 3666 |
| Tetris      | 3584 |
| Mario Kart Wii           | 3582 |
| Pokemon Red/Pokemon Blue         | 3137 |
| Call of Duty: Modern Warfare 3            | 3083 |
| New Super Mario Bros.          | 3001  |
| Call of Duty: Black Ops II        | 2972  |
| Wii Play          | 2902  |

## Extra Analysis
We will also use the same the queries for grouping based on sales in North America, Europe, Japan and Other countries.

**Which top 10 games is the most sold in North America?**
```bash
select TOP 10 Name, sum(NA_Sales) as NA_Total
from [dbo].[vgsales]
group by Name
order by 2 desc
```
The query will return 10 rows with again **Wii Sports** on the top with **4149 million sales** followed by **Super Mario Bros** with **2942 million sales.**
| Name        	    | Sales        	    |            	
|-------------------|-------------------|
| Wii sports           	| 4149 |
| Super Mario Bros.     | 2942 |
| Duck Hunt             | 2693 |
| Grand Theft Auto V    | 2004 |
| Call of Duty: Black Ops   | 1701 |
| Super Mario World         | 1599 |
| Mario Kart Wii            | 1585 |
| Wii Sports Resort         | 1575  |
| Call of Duty: Modern Warfare 3    | 1504  |
| Kinect Adventures!        | 1497  |

**Which top 10 games is the most sold in Japan?**
```bash
select TOP 10 Name, sum(JP_Sales) as JP_Total
from [dbo].[vgsales]
group by Name
order by 2 desc
```
The query will return 10 rows with again **Pokemon Red/Pokemon Blues** on the top with **1022 million sales** followed by **Super Mario Bros** with **696 million sales.**
| Name        	    | Sales        	    |            	
|-------------------|-------------------|
| Pokemon Red/Pokemon Blue          	| 1022 |
| Super Mario Bros.   | 696 |
| Pokemon Diamond/Pokemon Pearl       | 604 |
| Tetris               | 603 |
| Pokemon Black/Pokemon White   | 565 |
| Pokemon Ruby/Pokemon Sapphire           | 538 |
| Animal Crossing: Wild World           | 533 |
| Brain Age 2: More Training in Minutes a Day  | 532  |
| Final Fantasy III  | 512  |
| Nonster Hunter Freedom 3      | 487  |

**Which top 10 games is the most sold in Europe?**
```bash
select TOP 10 Name, sum(EU_Sales) as EU_Total
from [dbo].[vgsales]
group by Name
order by 2 desc
```
The query will return 10 rows with again **Wii Sports** on the top with **2902 million sales** followed by **Grand Theft Auto V** with **2304 million sales.**
| Name        	    | Sales        	    |            	
|-------------------|-------------------|
| Wii sports           	| 2902 |
| Grand Theft Auto V    | 2304 |
| Mario Kart Wii        | 1288 |
| FIFA 15               | 1240 |
| Call of Duty: Modern Warfare 3   | 1129 |
| FIFA 16            | 1299 |
| FIFA 14            | 1114 |
| Wii Sports Resort  | 1101  |
| Fifa Soccer 13  | 980  |
| The Sims 3      | 960  |

**Which top 10 games is the most sold in Other Countries?**
```bash
select TOP 10 Name, sum(Other_Sales) as Other_Total
from [dbo].[vgsales]
group by Name
order by 2 desc
```
The query will return 10 rows with **Grand Theft Auto: San Andreas** on the top with **1072 million sales** followed by **Wii Sports** with **846 million sales.** I guess some of the countries don't own a Wii.
| Name        	    | Sales        	    |            	
|-------------------|-------------------|
| Grand Theft Auto: San Andreas  | 1072 |
| Wii Sports                     | 846  |
| Grand Theft Auto V             | 803  |
| Gran Turismo 4                 | 753  |
| Call of Duty: Black Ops II     | 388  |
| FIFA Soccer 08                 | 353  |
| Pro Evolution Soccer 2008      | 351  |
| Call of Duty: Black Ops 3      | 342  |
| Call of Duty: Modern Warfare 3 | 335  |
| Mario Kart Wii                 | 331  |

## Our Query Run order and Functions
Now that we are familiar with most of the functionalities being used in a query, it is very important to understand the order that code runs.

We also used a few aggregate functions including all the functions mentioned below.

An **aggregate function** performs a calculation on a set of values, and returns a single value. Except for COUNT(*) , aggregate functions ignore null values. Aggregate functions are often used with the GROUP BY clause of the SELECT statement.
- SELECT
- FROM
- WHERE
- GROUP BY
- ORDER BY
- SELECT TOP 10

- Define which tables to use (FROM)
- Keep only the rows that apply to the conditions (WHERE)
- Group the data by the required level (if need) (GROUP BY)
- Order the output of the new table (ORDER BY)
- Limits the return of rows in the query (SELECT TOP 10)

## Exporting the tables for Tableau
All the tables that I have grouped with in the SSMS will be exported for tableau visualizations. You can to by typing in the query and save as the results as a csv file.
![](https://public.tableau.com/views/GameSales1982-2021/GameSales1982-2021?:language=en-US&:display_count=n&:origin=viz_share_link)

## Key Findings
![](https://github.com/TacoBadger/Video-Game-Sales/blob/main/Visuals/Game%20Sales%20(1982-2021).png?raw=true)
1. **Action** was the most the most popular Genre it was produced **3,253** times from **19820-2021!**
2. The peak year where most games were released was **2009** with **1,431** games released in a single year. The golden period for gamers began at **2007 to 2010** but it started to fall slowly because hyptothetically this may have been affected by Covid-19 slowing down all production rates and it can also affect Global Sales.
3. The top company to produce most games in the period of **1982 to 2022** was **Electornic Arts** with a total of **1,351** games released to the public.
4. And lastly! top sellings games were **Wii Sports which has 8,274 million sales and Super Mario Bros with 4,531 million sales.** This games are popular by streamers and people who love to play multiplayer games!

Extras:
![](https://github.com/TacoBadger/Video-Game-Sales/blob/main/Visuals/Top%20Selling%20Games.png?raw=true)
1. North America - **Wii Sports** on the top with **4,149 million sales** followed by **Super Mario Bros** with **2,942 million sales.**
2. Japan - **Pokemon Red/Pokemon Blue** on the top with **1,022 million sales** followed by **Super Mario Bros** with **696 million sales.**
3. Europe - **Wii Sports** on the top with **2,902 million sales** followed by **Grand Theft Auto V** with **2,304 million sales.**
4. Other Countries - **Grand Theft Auto: San Andreas** on the top with **1072 million sales** followed by **Wii Sports** with **846 million sales.**

## Explore our Tableau Public
- [@TacoBadger](https://public.tableau.com/app/profile/taco.badger)
- [Game Sales (1982-2021)](https://public.tableau.com/app/profile/taco.badger/viz/GameSales1982-2021/GameSales1982-2021)
- [Top Selling Games](https://public.tableau.com/app/profile/taco.badger/viz/TopSellingGames/TopSellingGames)

## Explore our Notebook
- [@TacoBadger](https://www.kaggle.com/code/cryptocosy/video-game-analysis-using-ssms)

## What is Next?
We have finished our practice! It may be complicated at first but when you get to practice you might start understanding it a little better.
- I could have also improved some of the groupings with better functions.
- I could also practice more types of data charts or data visualizations in tableau by practicing how to relate some variables along with improving my dashboard a little more.
- We can also practice more sales dataset with cryptocurrency datastets and other sales dataset to maximize using some of the grouping functions.

Thank you for reading my kernel or repositories!

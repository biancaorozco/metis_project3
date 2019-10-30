# Challenge Set 9
## Part II: Baseball Data

*Introductory - Intermediate level SQL*

--

Please complete this exercise via SQLalchemy and Jupyter notebook.

We will be working with the Lahman baseball data we uploaded to your AWS instance in class. 


1. What was the total spent on salaries by each team, each year?
````sql
SELECT yearid, teamid, SUM(salary) AS totalsalary 
FROM salaries
GROUP BY yearid, teamid;
````

2. What is the first and last year played for each player? *Hint:* Create a new table from 'Fielding.csv'.

```sql
SELECT playerid, MIN(yearid) AS firstyear, MAX(yearid) AS lastyear
FROM Fielding
GROUP BY playerid
ORDER BY playerid;
```

```sql
CREATE TABLE IF NOT EXISTS Fielding (
	    playerID varchar(20) NOT NULL,
	    yearID int NOT NULL,
	    stint int NOT NULL,
	    teamID varchar(3) DEFAULT NULL,
	    LgID text DEFAULT NULL,
	    POS varchar(2) DEFAULT NULL,
        G int DEFAULT NULL,
        GS text DEFAULT NULL,
        InnOuts text DEFAULT NULL,
        PO float DEFAULT NULL,
        A float DEFAULT NULL,
        E float DEFAULT NULL,
        DP float DEFAULT NULL,
        PB text DEFAULT NULL,
        WP text DEFAULT NULL,
        SB text DEFAULT NULL,
        CS text DEFAULT NULL,
	    ZR text DEFAULT NULL
    );

sql COPY Fielding FROM '/home/ubuntu/baseballdata/Fielding.csv' DELIMITER ',' CSV HEADER;
```

3. Who has played the most all star games?  

mayswi01, musiast01, and aaronha01 = 24

```sql
SELECT playerid, COUNT(gp) as totalgames
FROM AllstarFull
WHERE gp = '1'
GROUP BY playerid
ORDER BY totalgames DESC;
```

4. Which school has generated the most distinct players? *Hint:* Create new table from 'CollegePlaying.csv'. SchoolsPlayers.csv??

USC has generated 102 distinct players.

```sql
SELECT schoolid, COUNT(DISTINCT(playerid)) AS numplayers
FROM SchoolsPlayers
GROUP BY schoolid
ORDER BY numplayers DESC;
```

```sql
CREATE TABLE IF NOT EXISTS SchoolsPlayers (
	    playerID varchar(20) NOT NULL,
	    schoolID varchar(20) NOT NULL,
	    yearMin int DEFAULT NULL,
	    yearMax int DEFAULT NULL
    );

COPY SchoolsPlayers FROM '/home/ubuntu/baseballdata/SchoolsPlayers.csv' DELIMITER ',' CSV HEADER;
```

5. Which players have the longest career? Assume that the `debut` and `finalGame` columns comprise the start and end, respectively, of a player's career. *Hint:* Create a new table from 'Master.csv'. Also note that strings can be converted to dates using the [`DATE`](https://wiki.postgresql.org/wiki/Working_with_Dates_and_Times_in_PostgreSQL#WORKING_with_DATETIME.2C_DATE.2C_and_INTERVAL_VALUES) function and can then be subtracted from each other yielding their difference in days.

altroni01 has the longest career

```sql
SELECT playerid, (finalgame - debut) AS careerlength
FROM Master
WHERE (finalgame - debut) IS NOT NULL
ORDER BY careerlength DESC;
```

```sql
CREATE TABLE IF NOT EXISTS Master (
  playerID VARCHAR(9) NOT NULL,
  birthYear int DEFAULT NULL,
  birthMonth int DEFAULT NULL,
  birthDay int DEFAULT NULL,
  birthCountry varchar(100) DEFAULT NULL,
  birthState varchar(100) DEFAULT NULL,
  birthCity  varchar(100) DEFAULT NULL,
  deathYear int DEFAULT NULL,
  deathMonth int DEFAULT NULL,
  deathDay int DEFAULT NULL,
  deathCountry varchar(30) DEFAULT NULL,
  deathState varchar(30) DEFAULT NULL,
  deathCity varchar(100) DEFAULT NULL,
  nameFirst varchar(100) DEFAULT NULL,
  nameLast varchar(100) DEFAULT NULL,
  nameGiven varchar(100) DEFAULT NULL,
  weight int DEFAULT NULL,
  height int DEFAULT NULL,
  bats varchar(2) DEFAULT NULL,
  throws varchar(2) DEFAULT NULL,
  debut date DEFAULT NULL,
  finalGame date DEFAULT NULL,
  retroID VARCHAR(9) DEFAULT NULL,
  bbrefID VARCHAR(9) DEFAULT NULL
  );

COPY Master FROM '/home/ubuntu/baseballdata/Master.csv' DELIMITER ',' CSV HEADER;

```


6. What is the distribution of debut months? *Hint:* Look at the `DATE` and [`EXTRACT`](https://www.postgresql.org/docs/current/static/functions-datetime.html#FUNCTIONS-DATETIME-EXTRACT) functions.

```sql
SELECT debut, EXTRACT(MONTH FROM debut) AS month
FROM Master;
```

7. What is the effect of table join order on mean salary for the players listed in the main (master) table? *Hint:* Perform two different queries, one that joins on playerID in the salary table and other that joins on the same column in the master table. You will have to use left joins for each since right joins are not currently supported with SQLalchemy.

Because there are more players in the Master table, when the average salary is taken with Master joined on Salary, all players have a value. When Salary is joined on Master, there are missing values. 

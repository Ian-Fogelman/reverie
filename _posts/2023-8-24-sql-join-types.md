---
layout: post
title:  SQL Join Types
categories: [SQL, SQL Server, Join Types]
---

Howdy partner, today we are going to explore the different types of SQL joins.

This article uses SQL Server but the concept of joins is universal to all RDMS's.

# Join Types

The three type of joins you will most commonly use in SQL are the inner, left outter, and right outter join.

The join concepts have often been referred to as venn diagrams. In the diagrams the left most table, what I refer to as the base table
connects to other tables.

# Setup Details

Use the following command to pull a copy of the setup script and open it in SQL Server Management Studio. 

```
curl https://gist.githubusercontent.com/Ian-Fogelman/b8c37e835cd2bf48f94042d7f19fef06/raw/a0265565f83375f63426f9af28ed6699928cfee3/DBCB-Join-Types.sql -o DBCD-Join-Types.sql
```

If you do not have a SQL server environment, use the [SQL fiddle link](http://sqlfiddle.com/#!18/edd5e7/5).


# Create Database Tables

The first 43 lines of `DBCD-Join-Types.sql` create the database and the table structure for the tutorial. 
These commands create a `DB_COWBOYS` database and the `COWBOYS`,`DESCRIPTIONS`, and `TIMELINE` table.


```sql

CREATE DATABASE DB_COWBOYS
GO

USE DB_COWBOYS
GO

IF OBJECT_ID(N'dbo.COWBOYS', N'U') IS NOT NULL  
   DROP TABLE [dbo].[COWBOYS]; 

CREATE TABLE COWBOYS
( 
ID INT PRIMARY KEY IDENTITY(1,1) NOT NULL,
FULLNAME VARCHAR(100),
NICKNAME VARCHAR(100)
)

IF OBJECT_ID(N'dbo.DESCRIPTIONS', N'U') IS NOT NULL  
   DROP TABLE [dbo].[DESCRIPTIONS]; 

CREATE TABLE DESCRIPTIONS
(
COWBOY_ID INT,
DESCRIPTION VARCHAR(MAX)
)

IF OBJECT_ID(N'dbo.TIMELINE', N'U') IS NOT NULL  
   DROP TABLE [dbo].[TIMELINE]; 

CREATE TABLE TIMELINE
(
TIMELINE_ID INT,
YEAR_BORN INT,
YEAR_DIED INT
)

```
# Insert Data

Next we populate the tables with data, in this case the data is cowboy specific!

Cowboys - has an id, the cowboys full name, and nick name.

Description - has a a foreign key relationship to the cowboy id and a brief description.

Timeline - has a foreign key relationship to the cowboy id and has his birth and death year.

```sql
INSERT INTO COWBOYS
VALUES('Henry McCarty','Billy the Kid')

INSERT INTO DESCRIPTIONS
VALUES(
1, 'Possibly the most famous outlaw of the Wild West is Billy the Kid. Once a deadly gunfighter, Billy the Kid outwitted and killed eight men before the age of 21. 
An orphan at age 15, arrested the first time at 16, he fled to Arizona as an outlawed fugitive. Following the murder of a blacksmith, Billy the Kid returned to New Mexico to join a band of cattle rustlers who called themselves “The Regulators”. 
Before long, Billy the Kid and his murderous spree was featured in news stories across the country. Eventually, Sheriff Pat Garrett captured him around one month after the New York Sun spotlighted his crimes. 
After a short trial, Billy the Kid was convicted of his crimes and thrown into jail to await hanging. However, before his execution, the Kid broke free and was on the run again. 
Sheriff Garrett, however, was relentless in his search of the outlaw. On July 14, 1881, he finally shot and killed 21-year-old Billy the Kid at Fort Sumner, New Mexico.')

INSERT INTO TIMELINE
VALUES(1, 1859, 1881)

INSERT INTO COWBOYS
VALUES('Robert Leroy Parker','Butch Cassidy')

INSERT INTO DESCRIPTIONS
VALUES(
2, 'Butch Cassidy was a famous American train and bank robber and leader of the “Wild Bunch Gang”.
Growing up as a cowboy in Colorado, Butch Cassidy fled his home and found work on various ranches. While working on a dairy farm, he met cattle thief Mike Cassidy, who introduced him to a life of crime.
Butch robbed his first bank in Telluride, Colorado in 1889. Afterward, he fled with his gang to Robbers Roost, a remote hideout in southeastern Utah. In 1890, he purchased a ranch in Wyoming, possibly to cover for his illicit activities.
The Wild Bunch was ultimately formed with seven other outlaws and gunslingers. One notable gang member was Cassidy’s wife, cowgirl outlaw Laura Bullion. The Wild Bunch was responsible for bank robberies, ambushes of gold mine couriers, train robberies, and many shootouts with the law.
After multiple lucrative train robberies, Cassidy and his gang became the focus of the Pinkerton detective agency. Several of the gang members were shot and killed in the pursuit. 
In an attempt to escape the law, Butch Cassidy fled to South America. Following a holdup in Bolivia, local authorities surrounded the house to attempt to capture Cassidy and his partner Longabaugh. 
A shootout left Butch Cassidy and Longabaugh dead. Their bodies had multiple shots in the arms and legs, and each man had a bullet hole in the head. Some believe that Cassidy, in an attempt to put them out of their misery, shot his partner in the forehead before turning the gun on himself.'
)

INSERT INTO TIMELINE
VALUES(2,1866,1908)

INSERT INTO COWBOYS
VALUES('James Butler Hickok','Wild Bill')

INSERT INTO DESCRIPTIONS
VALUES(3, 'Wild Bill Hickok is a popular folk hero and gunslinger of the Old West. Wild Bill started his adventures as a stagecoach driver and lawman in Nebraska and Kansas. 
His service as a Union soldier in the American Civil War gained Hickok publicity as a scout. It was during the war that he first met William “Buffalo Bill” Cody. Together, they would go on to share the tales of the Wild West with the rest of the world.
After the war, Hickock turned to acting, marksman shooting, and professional gambling. He was also involved in several notable shootouts. 
Although Hickok spent some time acting in Buffalo Bill’s Wild West Show, he was never satisfied with his roles. He only continued with the show for a short time before returning to the comfort of gambling tables. 
In 1876, while playing poker in a saloon in Deadwood, Dakota, Wild Bill was shot and killed by another gambler Jack McCall. The hand of cards Wild Bill Hickok was dealt at the time of his death, namely two pairs of black aces and eights were dubbed “The Dead Man’s Hand”.')

INSERT INTO TIMELINE
VALUES(3,1837, 1876)
```

# Explore the Join Types

To see the table schema linked together run the following query:

```sql
SELECT * FROM COWBOYS AS C
	INNER JOIN DESCRIPTIONS AS D
		ON C.ID = D.COWBOY_ID
	INNER JOIN TIMELINE AS T
		ON T.TIMELINE_ID = C.ID
```

Notice that a query using `INNER JOIN` returns the same results as `JOIN`.

```sql
SELECT * FROM COWBOYS AS C
	JOIN DESCRIPTIONS AS D
		ON C.ID = D.COWBOY_ID
	JOIN TIMELINE AS T
		ON T.TIMELINE_ID = C.ID
```

Add a description for a cowboyid that does not exist yet in the `COWBOYS` table.

```sql
INSERT INTO DESCRIPTIONS
VALUES (4, 'Buffalo Bill is no doubt an iconic figure of the Wild West. An American soldier, bison hunter, and showman, he was the founder of Buffalo Bill’s Wild West Show. 
Bill started out as a Pony Express Rider on the American frontier. Later, he fought as a Union soldier in the American Civil War. During the Indian Wars, Buffalo Bill received a Medal of Honor from the US Army while serving as a civilian scout. 
As Cody’s fame grew, he established the wildly popular re-enactment show Buffalo Bill’s Wild West. Featuring other legends such as Wild Bill Hickok, Texas Jack, Calamity Jane, Sitting Bull, and Annie Oakley, Buffalo Bill’s show received international acclaim. 
The show entertained crowds from all over the world with trick-shooting, staged races, sideshows, and full re-enactments. Audiences could watch Pony Express riders racing through the plains amid wagon trains and Indian attacks. 
Stagecoach robberies and gunfights delighted the onlookers, before culminating in the final scene of Custer’s Last Stand. Buffalo Bill’s Wild West Show even toured Europe eight times, between 1887 and 1906.
')
```

If you left join from the `COWBOYS` table you will get the original inner join results.
This is because all cowboys in the `COWBOYS` table have a description and `COWBOY_ID` in the `DESCRIPTIONS` table.

```sql
SELECT * FROM COWBOYS AS C
	LEFT JOIN DESCRIPTIONS AS D
		ON C.ID = D.COWBOY_ID
```

To view the null records, i.e. the cowboy that does not have an id in the `COWBOYS` table:

- Make the `DESCRIPTIONS` table the base table.
- Use a left outer join.

```sql
SELECT * FROM DESCRIPTIONS AS D
	LEFT JOIN COWBOYS AS C
		ON C.ID = D.COWBOY_ID
```

You can also use a right outer join instead:

```sql
SELECT * FROM COWBOYS AS C
	RIGHT JOIN DESCRIPTIONS AS D
		ON C.ID = D.COWBOY_ID
```

Lets insert the last cowboys records into the `COWBOYS` and `TIMELINE` tables for a complete dataset:

```sql
INSERT INTO COWBOYS
VALUES('William Frederick Cody','Buffalo Bill')

INSERT INTO TIMELINE
VALUES(4,1846,1917)
```

```sql
SELECT * FROM COWBOYS AS C
	JOIN DESCRIPTIONS AS D
		ON C.ID = D.COWBOY_ID
	JOIN TIMELINE AS T
		ON T.TIMELINE_ID = C.ID
```

# Conclusion

In this tutorial we created a table structure, inserted data, and ran the three different type of join queries to understand how each join affects the result set.


---
layout: post
title:  SQL Join Types
categories: [SQL, SQL Server, Join Types]
---

Howdy partner, today we are going to explore the different types of SQL joins.

This article uses SQL Server but the concept of joins is universal to all RDMS's.


# Join Types

The three type of joins you will most commonly use in SQL are the inner, left outter, and right outter join.


# Data Setup

Use the following script to setup the database for this tutorial. 

<details>
  <summary>Click me</summary>
  
  ### Heading
  1. Foo
  2. Bar
     * Baz
     * Qux

  ### Some Javascript
  ```js
  function logSomething(something) {
    console.log('Something', something);
  }
  ```
</details>

<details>
  <summary>View script</summary>
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

	SELECT * FROM COWBOYS AS C
		INNER JOIN DESCRIPTIONS AS D
			ON C.ID = D.COWBOY_ID
		INNER JOIN TIMELINE AS T
			ON T.TIMELINE_ID = C.ID

	-- JOIN IS THE SAME AS INNER JOIN, BUT WITH LESS TEXT. SAME THING FOR "LEFT OUTER JOIN" == "LEFT JOIN", "RIGHT OUTER JOIN" == "RIGHT JOIN".
	SELECT * FROM COWBOYS AS C
		JOIN DESCRIPTIONS AS D
			ON C.ID = D.COWBOY_ID
		JOIN TIMELINE AS T
			ON T.TIMELINE_ID = C.ID

	-- MAKE A DESCRIPTION WITHOUT A COWBOY.
	INSERT INTO DESCRIPTIONS
	VALUES (4, 'Buffalo Bill is no doubt an iconic figure of the Wild West. An American soldier, bison hunter, and showman, he was the founder of Buffalo Bill’s Wild West Show. 
	Bill started out as a Pony Express Rider on the American frontier. Later, he fought as a Union soldier in the American Civil War. During the Indian Wars, Buffalo Bill received a Medal of Honor from the US Army while serving as a civilian scout. 
	As Cody’s fame grew, he established the wildly popular re-enactment show Buffalo Bill’s Wild West. Featuring other legends such as Wild Bill Hickok, Texas Jack, Calamity Jane, Sitting Bull, and Annie Oakley, Buffalo Bill’s show received international acclaim. 
	The show entertained crowds from all over the world with trick-shooting, staged races, sideshows, and full re-enactments. Audiences could watch Pony Express riders racing through the plains amid wagon trains and Indian attacks. 
	Stagecoach robberies and gunfights delighted the onlookers, before culminating in the final scene of Custer’s Last Stand. Buffalo Bill’s Wild West Show even toured Europe eight times, between 1887 and 1906.
	')

	-- IF YOU LEFT JOIN FROM THE COWBOYS TABLE YOU GET THE ORIGINAL INNER JOIN RESULTS.
	SELECT * FROM COWBOYS AS C
		LEFT JOIN DESCRIPTIONS AS D
			ON C.ID = D.COWBOY_ID

	-- IF YOU MAKE DESCRIPTIONS THE BASE TABLE, YOU CAN SEE THE NULL RECORDS FROM THE COWBOY TABLE.
	SELECT * FROM DESCRIPTIONS AS D
		LEFT JOIN COWBOYS AS C
			ON C.ID = D.COWBOY_ID

	-- TO VIEW THE NULL RECORDS WITH COWBOYS AS THE BASE TABLE, USE A RIGHT JOIN.
	SELECT * FROM COWBOYS AS C
		RIGHT JOIN DESCRIPTIONS AS D
			ON C.ID = D.COWBOY_ID

	--OPTIONAL, FILL IN THE MISSING COWBOYS DATA!

	INSERT INTO COWBOYS
	VALUES('William Frederick Cody','Buffalo Bill')

	INSERT INTO TIMELINE
	VALUES(4,1846,1917)

	SELECT * FROM COWBOYS AS C
		JOIN DESCRIPTIONS AS D
			ON C.ID = D.COWBOY_ID
		JOIN TIMELINE AS T
			ON T.TIMELINE_ID = C.ID
	```
</details>



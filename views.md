## VIEWS
* Is a stored query.
* Can be accessed as a virtual table in Postgres.
* Represents data of one or more tables through a SELECT statement.
* Does not store data physically.
* A view masks a query behind a virtual table.

##### Advantages
* A consistent interface to the data.
    * Even if tables behind it change.
* Can mask the details of the tables.
* Queries against views can reduce complexity.
    * Because you can query a view.
* Can improve security by giving selective access to data.
    * Grant access to users through a view that has specific data that users are authorized to see.

### View skeleton
```
CREATE [ OR REPLACE ] [ TEMP | TEMPORARY ] VIEW name [ ( column_name [, ...] ) ]
    [ WITH ( view_option_name [= view_option_value] [, ... ] ) ]
    AS query
```
* First, define view name after CREATE keyword.
* Second, define a query after AS keyword.

### Example
```
CREATE VIEW comedies AS
SELECT *
FROM films
WHERE kind = 'Comedy';
```
* A view consisting of all comedy films.

## PRACTICE PROBLEMS

<b>Practice problem 1 - tables </b>
```
CREATE TABLE People (
	PID SERIAL, 
	pName VARCHAR(50),
	pGender CHAR(1),
	pHeight FLOAT,
	PRIMARY KEY (PID)
);

CREATE TABLE Accounts (
    AID SERIAL, 
    PID INT,
    aDate DATE,
    aBalance INT,
    aOver INT,
    PRIMARY KEY (AID),
    FOREIGN KEY (PID) REFERENCES People(PID)
);

CREATE TABLE AccountRecords (
    RID SERIAL,
    AID INT,
    rDate DATE,
    rType CHAR(1),
    rAmount INT,
    rBalance INT,
    PRIMARY KEY (RID),
    FOREIGN KEY (AID) REFERENCES Accounts(AID)
);
```
<b> 1. Create a view to combine the information from the People and AccountRecords tables. Use this view to select all records for a particular individual, ordered by AID and RID.. </b>

```
CREATE VIEW peopleRecords(PID, pName, pGender, pHeight, AID, RID, rData, rType, rAmount, rBalance)
AS
SELECT
    P.PID, P.pName, P.pGender, P.pHeight,
    R.AID, R.RID, R.rDate, R.rType, R.rAmound, R.rBalance
FROM People P
    JOIN Accounts A ON P.PID = A.PID
    JOIN AccountRecords R ON R.AID = A.AID;
```
```
SELECT PR.*
FROM peopleRecords PR
WHERE PR.PID = 'Jon Jonsson'
ORDER BY PR.AID, PR.RID;
```

<b> Practice problem 2 - tables </b>
```
CREATE TABLE Agents (
    AgentID serial PRIMARY KEY,
    codename varchar(255),
    designation char(4),
    killLicense boolean NOT NULL,
    status varchar(20),
    secretIdentity int,
    GenderID int,
    FOREIGN KEY (secretIdentity) REFERENCES People (PersonID),
    FOREIGN KEY (GenderID) REFERENCES Genders (GenderID)
);

CREATE TABLE Cases (
    CaseID serial PRIMARY KEY,
    title varchar(255),
    isClosed boolean NOT NULL,
    year smallint,
    AgentID int NOT NULL REFERENCES Agents (AgentID),
    LocationID int REFERENCES Locations (LocationID)
);

CREATE TABLE Locations (
    LocationID serial PRIMARY KEY,
    location VARCHAR(255) NOT NULL,
    caseCount int NOT NULL DEFAULT 0
);
```

<b> 2. Create a view that finds the codename and status of each agent along with the number of cases that they have led, as well as the most common location that they have led cases in. In case of a tie, display all the locations as an array. </b>

Solution 1

```
CREATE OR REPLACE VIEW getAgentStats
(Codename, Status, "Num Cases", Locations)                                  -- Column names for view.
AS
    SELECT A.codename, A.status, (CASE WHEN                                 -- Number of cases, if NULL return 0.
                                        CO.case_count IS NULL THEN 0
                                  ELSE 
                                        CO.case_count
                                  END) 
                                        AS "Case Count",               
    array_agg(L.location) AS "Locations"                                    -- array_agg, returns array of locations.

    /* SELECT codename & status FROM query containing most common location for agents. */
    FROM 
        (SELECT C2.agentID, C2.locationID                                   -- Select case agentID and case locationID. 'RE'
            FROM Cases C2
            INNER JOIN                                                      -- Inner join agentID's from subquery.

                -- Select count of most common location for agentID.                                                     'R2'
                (SELECT R3.agentID, max(R3.loc_count) AS "loc_max_count"    -- Get the max count of locationID's for agents.
                    FROM (                                                  -- Get the count of locationID's for agents. 'R3'
                        SELECT C3.agentID, C3.locationID, count(C3.locationID) AS "loc_count"
                        FROM Cases C3
                        GROUP BY C3.agentID, C3.locationID 
                    ) R3 
                GROUP BY R3.AgentID) R2
                ON R2.agentID = C2.agentID                                  /* Join case agentID's with most agentID's with their
                                                                               most common location. */
            GROUP BY 
                C2.agentID, C2.locationID, R2.loc_max_count                 -- GROUP BY to get only the most common locationID's.
            HAVING 
                count(C2.locationID) = R2.loc_max_count) RE                 -- Only the most common locationID's for agents.

    /* Finally join most common locationID's to get the location names */
    INNER JOIN Locations L
        ON L.locationID = RE.locationID
    RIGHT JOIN Agents A
        ON A.agentID = RE.agentID                                           -- Right join to get agentID's with most common location.
    LEFT JOIN (SELECT CC.agentID, count(CC.locationID) AS "case_count"      -- Left join agentID's with Cases to get the case count.
                FROM Cases CC
                GROUP BY CC.agentID) CO
        ON CO.agentID = A.agentID
    GROUP BY A.agentID, CO.case_count;                                     
```

Solution 2 - with functions

* Get the number of cases led by a agent.
```
CREATE OR REPLACE FUNCTION agentCaseCount(id INT)
RETURNS INT
LANGUAGE plpgsql
AS
$$
DECLARE
    cnt INTEGER;
BEGIN                       -- Simply count the number of cases for a given agentID.
    SELECT count(*) 
    INTO cnt
    FROM agents A
    INNER JOIN Cases C ON A.agentID = C.agentID
    WHERE C.agentID = id;

    RETURN cnt;
END;
$$;
```
* Get the most common locations of cases led by a agent.
```
CREATE OR REPLACE FUNCTION avgAgentCaseLocations(id INT)
RETURNS TABLE (locations TEXT[])
LANGUAGE SQL
AS
$$
    SELECT 
        ARRAY_AGG(L.location) commonLocations           -- Return an array of locations.
    FROM
        (SELECT C.agentID, C.locationID
        FROM Cases C
        INNER JOIN (SELECT AllLoc.agentID, MAX(AllLoc.locationCount) AS maxLocations
                    FROM
                        (SELECT C1.agentID, C1.locationID, COUNT(C1.locationID) AS locationCount
                            FROM Cases C1
                            GROUP BY C1.agentID, C1.locationID
                        ) AllLoc
                    WHERE AllLoc.agentID = id           -- Get the locations for agentID here
                    GROUP BY AllLoc.agentID) AgentLoc
        ON AgentLoc.agentID = C.agentID
        GROUP BY
            C.agentID, C.locationID, AgentLoc.maxLocations
        HAVING
            COUNT(C.locationID) = AgentLoc.maxLocations) M
    INNER JOIN Locations L ON M.locationID = L.locationID;
$$
```
* Finally the view
```
CREATE OR REPLACE VIEW agentRecords
AS
SELECT
    A.codename, A.status,
    agentCaseCount(A.AgentID),
    avgAgentCaseLocations(A.agentID)
FROM
    Agents A;
```

<b> Practice problem 3 - tables </b>
```
CREATE TABLE InvolvedIn (
    PersonID int REFERENCES People (PersonID),
    CaseID int REFERENCES Cases (CaseID),
    AgentID int REFERENCES Agents (AgentID), /* investigates */
    isCulprit boolean,
    PRIMARY KEY (PersonID, CaseID)
);

CREATE TABLE People (
    PersonID serial PRIMARY KEY,
    name varchar(255),
    ProfessionID int REFERENCES Professions (ProfessionID), /* WorksIn */
    GenderID int REFERENCES Genders (GenderID),
    LocationID int REFERENCES Locations (LocationID) /* LivesIn */
);
```
<b> 3. Create a view that displays a top 3 list of suspects from ‘Stokkseyri’. Those are the people involved in the most number of cases. The view should return each suspect's ID, name and the town they are from. </b>
```
CREATE OR REPLACE VIEW Top3Stokkseyri(PersonID, Name, Location)
AS
    SELECT P.personID, P.name, L.location
    FROM People P
    INNER JOIN InvolvedIn I
        ON P.personID = I.personID
    INNER JOIN Locations L
        ON P.locationID = L.locationID
    WHERE L.location = 'Stokkseyri'
    GROUP BY P.personID, L.locationID
    ORDER BY count(I.caseID) DESC
    LIMIT 3;
```
## PROCEDURES
* Procedures also know as 'stored procedures'.
* Allow for extending database functionality.
* User-defined functions, using various procedural languages.
* Re-usable SQL code, mainly queries.
* If you have a repeating SQL query, save it as a stored procedure,
and then just call it to execute it.

##### How do they differ from functions?
* A function is used to calculate result using given inputs. 
* A procedure is used to perform certain task in order. 
* A function can be called by a procedure.
* A procedure cannot be called by a function.

### Why use stored procedures ?

##### General
* Avoid rewriting subqueries and improve readability.
* If a query can't be stored in a library that all applications can access
you can put that query in a stored procedure.
* Allow for other languages to be used.
    * Postgres comes with  PL/pgSQL, PL/Tcl, PL/Perl and PL/Python.

##### Data integrity
* Use triggers or constraints to prevent bad data from entering.
* Run several independent queries in a transaction, in a single stored procedure.

##### Log handling
* Log various changes.
* Notify other systems of new data.

### Why NOT use stored procedures ?
* Views may be sufficent and all you need.
* Difficult to version control stored procedurs
* Software rollouts may require more DB changes.
* Could slow development process.


### Procedure skeleton
```
create [or replace] procedure procedure_name(parameter_list)
language plpgsql
as $$
declare
-- variable declaration
begin
-- stored procedure body
end; $$
```
* First, specify the name of the stored procedure after the create procedure keywords.
* Second, define parameters for the stored procedure. A stored procedure can accept zero or more parameters.
* Third, specify plpgsql as the procedural language for the stored procedure. Note that you can use other procedural languages for the stored procedure such as SQL, C, etc.
* Finally, use the dollar-quoted string constant syntax to define the body of the stored procedure.
    * Parameters in stored procedures can have the in and inout modes. They cannot have the out mode.

### PRACTICE PROBLEMS

* <b>Tables - practice problem 1</b>
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
```

<b> 1. Create a procedure to insert a new account </b>

```
CREATE OR REPLACE PROCEDURE insertNewAccount (  -- First: Specify name.
    PID_in INT,
    aDate_in INT,
    aBalance_in INT,                            -- Second: Define parameters.
    aOver_in INT,
)
LANGUAGE SQL                                    -- Third: Specify language.
AS $$                                           -- Finally: Define the body of the procedure

    INSERT INTO ACCOUNTS(AID, PID, aDate, aBalance, aOver)
    VALUES
    (default, PID_in, aDate_in, aBalance_in, aOver_in);
$$;

BEGIN;
    CALL insertNewAccount(5, CURRENT_DATE, 9, 4);
    SELECT * FROM Accounts;
ROLLBACK;
```
<b> Similar function alternative</b>
```
CREATE OR REPLACE FUNCTION insertNewAccount (IN PID_in INT)
RETURNS VOID
AS
$$
    INSERT INTO Accounts(PID, aDate, aBalance, aOver)
    VALUES (PID_in, CURRENT_DATE, 0, 0);
$$
LANGUAGE sql;
```
* <b>Tables - practice problem 2</b>
```
Students(sID:int, sname:string, sGPA:float)
Grades(sID:int, cID:int, registered:date, grade:float)
```

<b> 2. 
Write an SQL procedure 'insertStudent' that accepts the number and name of a student and inserts a record into Students with sGPA = 0 </b>
```
CREATE OR REPLACE PROCEDURE insertStudent( 
    sID_in INT,
    sname_in string
)
LANGUAGE SQL
AS $$
    INSERT INTO Students(sID, sname, sGPA)
    VALUES (sID_in, sname_in, 0);
$$;
```
## TRIGGERS
* A trigger can be defined to either execute before or
after an INSERT, UPDATE or DELETE, either per statement
or per modified row.
* Triggers execute other functions.
* Trigger functions must be defined in an available procedural language.

### Automatic variables in triggers
* NEW: The new row for INSERT/UPDATE statments.
* OLD: The old row for UPDATE/DELETE statements.

* TG_NAME: Name of the trigger.
* TG_WHEN: 'BEFORE' or 'AFTER'.
* TG_LEVEL: 'ROW' or 'STATEMENT'.
* TG_OP: 'INSERT', 'UPDATE', or 'DELETE'.

* TG_TABLE_NAME: Name of the table that invoked the trigger.
* TG_TABLE_SCHEMA: Schema for the table in TG_TABLE_NAME.

### Trigger function skeleton
```
CREATE FUNCTION trigger_function() 
   RETURNS TRIGGER 
   LANGUAGE PLPGSQL
AS $$
BEGIN
   -- trigger logic
END;
$$
```
* Note that the function returns a trigger.

### Trigger skeleton
```
CREATE TRIGGER trigger_name 
   {BEFORE | AFTER} { event }
   ON table_name
   [FOR [EACH] { ROW | STATEMENT }]
       EXECUTE PROCEDURE trigger_function()

where event can be one of:
    INSERT
    UPDATE [ OF column_name [, ... ] ]
    DELETE
    TRUNCATE
```
CREATE TRIGGER creates a new trigger. The trigger will be associated with the specified table or view and will execute the specified function function_name when certain events occur.

### Examples
```
CREATE TRIGGER check_update
    BEFORE UPDATE ON accounts
    FOR EACH ROW
    EXECUTE PROCEDURE check_account_update();
```
* Execute the function check_account_update whenever a row of the table accounts is about to be updated. 
</br>
  

```
CREATE TRIGGER check_update
    BEFORE UPDATE OF balance ON accounts
    FOR EACH ROW
    EXECUTE PROCEDURE check_account_update();
```
* The same, but only execute the function if column balance
  is specified as a target in the UPDATE command.
</br>

```
CREATE TRIGGER check_update
    BEFORE UPDATE ON accounts
    FOR EACH ROW
    WHEN (OLD.balance IS DISTINCT FROM NEW.balance)
    EXECUTE PROCEDURE check_account_update();
```
* This form only executes the function if column balance has in fact changed value.
</br>

```
CREATE TRIGGER log_update
    AFTER UPDATE ON accounts
    FOR EACH ROW
    WHEN (OLD.* IS DISTINCT FROM NEW.*)
    EXECUTE PROCEDURE log_account_update();
```
* Call a function to log updates of accounts, but only if something changed
</br>

```
CREATE TRIGGER view_insert
    INSTEAD OF INSERT ON my_view
    FOR EACH ROW
    EXECUTE PROCEDURE view_insert_row();
```
* Execute the function view_insert_row for each row to insert rows into the tables underlying a view

## PRACTICE PROBLEMS

<b>Practice problem 1 - table </b>
```
CREATE TABLE People (
	PID SERIAL, 
	pName VARCHAR(50),
	pGender CHAR(1),
	pHeight FLOAT,
	PRIMARY KEY (PID)
);
```
<b> 1. Create a trigger on the People table to a) ensure that gender is correct
      (either ‘M’ or ‘F’) and b) that height is > 0. 
       If either is not correct, then abort the transaction.</b>


```
-- Function for the trigger.
CREATE FUNCTION checkPeople()
RETURNS TRIGGER
LANGUAGE plpgsql
AS
$$
BEGIN
    ( IF NEW.pGender <> 'M' AND new.pGender <> 'F ) THEN
        -- This is one very simple way to signal an error
		RAISE EXCEPTION 'Gender must be M or F!' USING ERRCODE = '45000';
    END IF;

    IF ( NEW.pHeight < 0 ) THEN
        -- This also aborts the transaction, but with more information
            RAISE EXCEPTION 'Height cannot be negative!' USING ERRCODE = '45000';
	END IF;

	RETURN NEW;
END;
$$


-- The trigger itself
CREATE TRIGGER checkPeople
AFTER INSERT OR UPDATE ON People
FOR EACH ROW EXECUTE PROCEDURE checkPeople();
```

<b>Practice problem 2 - tables </b>
```
CREATE TABLE employees(
   id INT GENERATED ALWAYS AS IDENTITY,
   first_name VARCHAR(40) NOT NULL,
   last_name VARCHAR(40) NOT NULL,
   PRIMARY KEY(id)
);
CREATE TABLE employee_audits (
   id INT GENERATED ALWAYS AS IDENTITY,
   employee_id INT NOT NULL,
   last_name VARCHAR(40) NOT NULL,
   changed_on TIMESTAMP(6) NOT NULL
);
```
<b> 2. Create a trigger that logs the name of an employee
       in the employee_audits whenever the last name of an employee
       changes</b>

```
-- Function for trigger
CREATE OR REPLACE FUNCTION logEmployeeLastName()
RETURNS TRIGGER
LANGAUGE plpgsql
AS
$$
BEGIN
    IF NEW.last_name <> OLD.last_name THEN
        INSERT INTO employee_audits(employee_id, ,last_name, changed_on)
        VALUES (OLD.id, OLD.last_name, now());
    END IF;

    RETURN NEW;
END;

CREATE OR REPLACE TRIGGER lastNameChanges
BEFORE UPDATE
ON employees
FOR EACH ROW
EXECUTE PROCEDURE logEmployeeLastName();
```
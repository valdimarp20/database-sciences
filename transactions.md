## TRANSACTIONS
* Transactions are a fundemental concept of all database
  systems.
* Transactions allow users to make changes to data and then
  decide whether to save or discard the work.
* Database transactions bundle multiple steps into one logical
  unit of work.
#### Consist of one of the following

* DML statements which constitute one consistent change to the data.
  * INSERT, UPDATE and DELETE.
* One DDL statement such as CREATE, ALTER, DROP, RENAME or TRUNCTATE.
* One DCL statement such as GRANT or REVOKE.

### ACID Properties
A transaction in a database system must maintain ACID properties,
 in order to ensure accuracy, completeness and data integrity.
<b>A</b>tomicity, <b>C</b>onsistency, <b>I</b>solation, <b>D</b>urability

### A : Atomicity.
<b>Transaction is "one operation" </b> (atomic unit).

  * All changes to data are performed as if they are a single operation.
  * All changes are performed or none of them.
  * Must guarantee atomicity in every situation.

### C : Consistency.
  <b>Data is in a consistent state when a transaction starts and when it ends.</b>

  * Each transaction moves the database from one consistent state to antoher.
  * No transaction should have any adverse affect on data.
  * Can only bring the database from one valid state to another.
  * Any data written to the database must be valid according to all defined rules.
    * Including triggers, constraints & cascades (and in combination).
### I : Isolation.
  <b>The intermediate state of a transaction is invisible to other transactions.</b>

  * Each transaction is isolated.
  * Multiple transactions occur independently without interference.
  * transactions that run concurrently appear to be serialized.
  * Isolation is the goal of concurrency control.
### D : Durability.
<b>After a succesfull transaction, changes to data persist and are not undone.</b>

  * Upon commit, the effects of a transaction stay in the database.
  * Changes of a successful transaction occur even if the system fails.
  * completed transactions (or their effects) are recorded in
  non-violated memory.


### Transaction skeleton
### Begin a transaction
To start a transaction use the following statement.
```
BEGIN TRANSACTION;
```
or
```
BEGIN WORK;
```
or just
```
BEGIN;
```
* For example, the following statements start a new transaction
  and inserts a new account into the accounts table:
```
BEGIN;

INSERT INTO accounts(name,balance)
VALUES('Alice',10000);
```
* From the current session, you can see the change by querying the accounts table.
* However, if you start a new session and execute the query above, you will not see the change.

### Commit a transaction
To make the change become visible to other sessions (or users)
you need to commit the transaction by using the COMMIT WORK statement.
```
COMMIT WORK;
```
or
```
COMMIT TRANSACTION;
```
or simply
```
COMMIT;
```

* Using one of the statements inserts Alice’s account to the accounts table.
* From other sessions, you can view the change by querying the accounts table.

### Put it all together
```
-- start a transaction
BEGIN;

-- insert a new row into the accounts table
INSERT INTO accounts(name,balance)
VALUES('Alice',10000);

-- commit the change (or roll it back later)
COMMIT;
```
### Examples

An example of transfering 1000USD from a bank account.
we will transfer from account 'A' to account 'B'.
(Transaction steps are numbered)

<b>1. Start a new transaction in the first session</b>
```
BEGIN;
```

<b>2. Subtract 1000USD from account 'A' </b>

```
UPDATE accounts 
SET balance = balance - 1000
WHERE id = 'A';
```
<b>In the second session, check the account balance
    of account 'A' and 'B' </b>
```
SELECT 
    id,
    name,
    balance
FROM 
    accounts;
```

* The change will not be visible in the second session,
  we have to commit the transaction.

<b>4. Add the same amount (1000USD) to account 'B' </b>
```
UPDATE accounts
SET balance = balance + 1000
WHERE id = 'B;
``` 

* This change also is not visible to the second session
  until we commit it.

<b>5. Commit the change</b>
```
COMMIT;
```

* Now, you can view the change from any session.

<b>Put it all together</b>
```
-- start a transaction
BEGIN;

-- deduct 1000 from account 'A'
UPDATE accounts 
SET balance = balance - 1000
WHERE id = 'A';

-- add 1000 to account 'B'
UPDATE accounts
SET balance = balance + 1000
WHERE id = 'B'; 

-- select the data from accounts
SELECT id, name, balance
FROM accounts;

-- commit the transaction
COMMIT;
```

* To undo a change, you can execute the ROLLBACK statement.
```
ROLLBACK;
```

```
-- begin the transaction
BEGIN;

-- deduct amount from account 'A'
UPDATE accounts 
SET balance = balance - 1000
WHERE id = 'A';

-- add 1000 to account 'B'
UPDATE accounts
SET balance = balance + 1000
WHERE id = 'B'; 

-- select the data from accounts
SELECT id, name, balance
FROM accounts;

-- commit the transaction
COMMIT;

-- roll back the transaction
ROLLBACK;
```

#### Set transaction 'savepoints'

```
SAVEPOINT <name>;
```
* Creates a marker in the transaction, which divides it into smaller pieces.

```
ROLLBACK TO SAVEPOINT <name>;
```
* Allows the user to roll back a transaction to a specified save point.
## TRANSACTIONS
* The essential point of a transaction is that it bundles multiple steps into a single, all-or-nothing operation.
* Are a group of operations to the database.
* Can be defined as a group of 'tasks'.
* Wish for isolation from other transactions.
* A single transaction is the minimum processing unit.
    * Cannot be divided further.
* A transaction is a very small unit of a program.
* A transaction may contain several low-level tasks.
* A transaction must maintain a number of properties.
(see below)

### ACID Properties
A transaction must maintain ACID properties.

* #### A : Atomicity.
    * Transaction is "one operation".
* #### C : Consistency.
    * Each transaction moves the database from
    one consistent state to antoher.
* #### I : Isolation.
    * Each transaction is isolated.
* #### D : Durability.
    * Upon commit, the effects of each
    transaction stay in the database
</br>

### Atomicity
* Transactions are composed of multiple units/statements.

* A transaction must be treated as an atomic unit.
    * Meaning either all of its operations are executed
      or none.

* Must guarantee atomicity in every situation.
* There must be no state in the database where a transaction
  is left partially completed.

* States should be defined before execution of the transaction or
  after the exectuion/abortion/failure of the transaction.

### Consistency
* Consistency means that a transaction can only bring the database
  from one valid state to another.
* If the database was in a consistent state before a transaction,
  it must remain consistent after a transaction.

* Any data written to the database must be valid(consistent) according
  to all defined rules.
    * Including triggers, constraints & cascades (and in combination).
* The database must remain in a consistent state after
  any transaction.

* No transaction should have any adverse effect on the data
  residing in the database.

### Isolation
* All the transactions will be carried out and executed as
  if it is the only transaction in the system.
* Meaning that transactions are executed concurrently (all at the same time).

* No transaction will affect the existence of any other transaction.
* Each transaction happens in isolation.
* Isolation is the goal of concurrency control.

### Durability
* Durability guarantees that when a transaction is commited,
  it remains commited even during system failure.
* If a transaction commits but the system fails before the data
  could be written on the disk, then that data will be updated 
  once the system spring back into action.

* This means that completed transactions (or their effects) are recorded in
  non-violated memory.

### Transaction skeleton
#### Begin a transaction
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
  and insert a new account into the accounts table:
```
BEGIN;

INSERT INTO accounts(name,balance)
VALUES('Alice',10000);
```
* From the current session, you can see the change by querying the accounts table.
* However, if you start a new session and execute the query above, you will not see the change.

#### Commit a transaction
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

#### Put it all together
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
* Set a savepoint.

```
ROLLBACK TO SAVEPOINT <name>;
```
* Undo changes until savepoint.
    * Rollback to a given savepoint.
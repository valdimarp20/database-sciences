## TRANSACTIONS - continued

### ACID properties - summary

### Atomicity
* Two possible outcomes for a transaction.
    * It <i>commits</i>: All the changes are made.
    * It <i>aborts</i>: No changes are made.

* That is, transactions' activites are 'all or nothing'.

### Consistency
* The state of tables is restricted by integrity contraints, for example.
    * Account number is unique.
    * Stock amounts can't be negative.
    * Sum of debits and of credits is 0.
* Constraints may be <b>explicit</b> or <b>implicit</b>.
    * Implicit constraints are applied in the data model.
    * Explicit constraints are defined in DDL.
* How consistency is achieved.
    * Make sure that a transaction takes a 
    <i>consistent state to a constistent state.</i>
    * The system makes sure that a transaction is atomic.

### Isolation
* A transaction executes concurrently with other transaction.
* Isolation: The effect is as if each transaction
  executes in isolation of others.

### Durability
* The effect of a transaction must continue to
  exists after the transaction, or the while program
  has terminated.

* Means: Write data (to disk).

### Serializability

When multiple transactions are being executed there are possibilities
that instructions of one transactions are interleaved with some other
transaction.

### Meaning of interleaving transactions

A database can't wait for a previous user to complete his transaction.
Meaning that all the transactions can't happen in in sequential order.
</br>

So transactions are <i>interleaved</i>. Meaning that a <b>second transaction is started before the first one could end.</b>
Execution can then switch between the transactions back and forth.
It can also switch between multiple transactions.

This could cause inconsistency in the the system. But are handled by the
database.

This is where <i>schedules</i> come in.

### Schedules

### Schedule

* A chronological execution sequence of a transaction is called a schedule.
* A schedule can have many transactions in it.
    * Each comprising og a number of tasks.

### Serial schedule

* A schedule in which transactions are aligned in such a way
  that on transaction is executed first.
* When the first transaction completes it's cycle, then the next transaction
  is executed.
* This type of schedule is called a serial schedule.
    * Transactions are executed in a serial manner.
</br>

### Serial schedules are considered as the "gold standard".

* The execution sequence of an instruction in a transaction cannot be changed.
* Two transactions can have their instructions executed in a random fashion.
* This execution does no harm if two transactions are mutually independent
  and working on different segments of data.
* In the case these two transactions are working on the same data.
    * Results may vary.
    * This varying result may bring the database to an inconsistent state.

### Concurrent execution of transactions

* An important task of a DBMS is to schedule concurrent accesses to data.
    * So that each user can safely ignore the fact that others
      are accessing the data concurrently.

* A <b>locking protocol</b> is a set of rules to be followed by each 
  transaction (and en- forced by the DBMS).

* Even though actions of several transactions might be interleaved.
    * The net effect is identical to executing all transactions in some serial order.

* A lock is a mechanism used to control access to database objects.
</br>

<b>Two kinds of locks are commonly supported by a DBMS.</b>

* <b>Shared locks</b>: When two transactions are granted read access.
    1. One transaction gets the shared lock on data.
    2. When the second transaction requests the same data it is also given a shared lock.
    3. Both transactions are in a read-only mode.
    4. Updating the data is not allowed until the shared lock is released.

* <b>Exclusive locks</b>: When a statement modifies data, its transaction
                          holds an exclusive lock on data.
1. Preventing other transactions from accessing the data.
2. This lock remains until the transaction holding the lock
   issues a commit or rollback.

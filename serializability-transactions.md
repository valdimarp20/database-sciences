## TRANSACTIONS - SERIAL
* If transactions are executed serially,
  i.e., sequentially with no overlap time then no transaction concurrency exists.

<b> However if concurrent transactions with interleaving operations are 
allowed in an uncontrolled manned, some unexpected, undesirable results 
may occur. </b>
</br>

#### The lost update problem
* A second transaction overwrites the value written by a first concurrent
  transaction.
</br>

* The first value is then lost to other transactions running concurrently,
  which need by their precedence, to read the first value.
</br>

* Transactions have therefore read the wrong value and end up with 
  incorrect results.

<i>In essence this means that succesful updates can be overwritten
by other transactions.</i>

#### The lost update problem - example

Following table used for our example
```
Product (id, name, itemsInStock)
```

Consider the following series of transactions.
* <b> Transaction 1 </b>
    * User 1 starts the process of selling 2 items. 

* <b> Transaction 2 </b>
    * User 2 starts the process of selling 3 items.

| <b> Transaction 1 </b>  | <b> Transaction 2 </b>                |
|-------------------------|---------------------------------------|
| Reads items in stock, 12 available    |                         |
|                         |   Reads items in stock, 12 available  |
|                         |   Sells 3 items.                      |
| Sells 2 items           |                                       |
|                         |   Update items in stock, 9 available  |
|  Update items in stock, 10 available          |                 |

* Transaction 2 will complete it's execution first and
  updates itemsInStock to 9.

* Transaction 1 commits itself. Since transaction 1 sold two items and
  updates itemsInStock to 10.

<b><i> This is incorrect, the correct figure is 12-3-2 = 7 </i></b>

#### The dirty read problem
* Transactions read a value written by a transaction that has
  been aborted.
</br>

* This value disappears from the database upon abort and should
  not have been read by any transaction ("dirty read").
</br>

* The reading transactions end with incorrect results.

#### The incorrect summary problem
* While one transaction takes a summary over the values of
  all the instances of a repeated data-item.
</br>

* Second transaction updates some instances of that data-item.
</br>

* The resulting summary does not reflect a correct result for any
  precedence order between the two transactions.


### Serializability

#### Properties

<b> Serial schedule </b>

* A schedule where operations of transactions are not
  interleaved.

* "Gold standard" - but no concurrent execution!

<b> Equivalent shedules </b>

* Two (or more) schedules that have the same effect on the database.
* Equivalent to "gold standard" - could be concurrent!

### Deadlocks

* Cycle of transactions, each waiting for the next
  to release a lock.

* Resolve by aborting on transaction.
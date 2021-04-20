## RELATIONAL ALGEBRA
* Procedual query language.
* Performed recursively.
* Yields instances of relations as output.

#### Operations - RELATIONAL ALGEBRA
* U : Union.
* ∩ : Intersection.
* \ : Set difference.
* x : Cartersian product
* ∏ : Projection
* σ : Selection
    * σ <sub>predicate</sub> (relation)
     * select from relation, with predicate logic.
    * p : Prepositional logic.
    * r : Relation.
* ρ : Rename

#### Examples
* `σ `<sub>`subject = "database"`</sub>` (Books)`
    * Output: Selects tuples from books where subject is 'database'.
* ` σ `<sub>`subject = "database" and price = "450"`</sub>` (Books)`
    * Output: Same as above but price is 450.
    
## RELATIONAL CALCULUS
* Filtering variable ranges over tuples.
* Notation: `{ T | Condition }`.
    * Returns all tuples T that satisfy a condition.
* Mathematical operations apply.

### Examples
* `{ T.name | Author(T) ^ T.article='database' }`
    * Output: Returns tuples with 'name' from Author who has written an article on 'database'.

* Can be quantified:
* `{ R | ∃T ∈ Authors(T.article='database' ^ R.name=T.name) }`
    * Output: Same as above.


## PRACTICE PROBLEMS
<b>Tables - practice problem 1</b>
```
Movies(movieID:int, mname:string, myear:int, mbudget:int) 
Roles(actorID:int, movieID:int, rname:string, rsalary:int)
```

<b> 1. A query that returns the names of movies with a budget of more than 10,000,000. </b>

- Relational algebra
`∏`<sub>`mname`</sub>`( σ `<sub>`mbudget > 10.000.000 `</sub>` )`

- Relational calculus
`{T | ∃M ∈ (M.mbudget > 10.000.000 ^ M.mname = P.pname)}`

<b> 2. A query that returns the names of roles where the salary paid was more than 10% of the movie’s budget. </b>

- Relational algebra
`∏`<sub>`rname`</sub>`( σ `<sub>`rsalary > mbudget/10 `</sub>`(Roles ⋈ Movies))`

- Relational Calculus
```
{ T | ∃M ∈ ∃R ∈ Roles ( R.movieID = M.movieID 
                        ^ R.salary > M.mbudget/10 
                        ^ T.rname = R.rname) }
```

<b>Tables - practice problem 2</b>
```
CREATE TABLE People (
    ID SERIAL,
    Pname CHARACTER VARYING NOT NULL,
    Phone CHARACTER VARYING NOT NULL,
    DOB DATE NOT NULL,
    PRIMARY KEY (ID)
);
CREATE TABLE Member (
    ID INTEGER,
    Start_date DATE NOT NULL,
    FOREIGN KEY(ID) REFERENCES People(ID)
);
CREATE TABLE Enemy (
    ID INTEGER,
    PRIMARY KEY(ID),
    FOREIGN KEY(ID) REFERENCES People(ID)
);
```

<b> 1. A query that returns ID and phone number of all members named Jón Jónsson. </b>

* Relational algebra
`∏`<sub>`ID, Phone`</sub>`(σ `<sub>`Pname='Jon Jonsson' `</sub>`(People ⋈ Members))`

* Relational calculus
```
{ T | ∃P ∈ ∃M ∈ (P.ID = M.ID
                 ^ P.Pname = 'Jon Jonsson'
                 ^ T.name 
                 ^ T.ID = P.ID
                 ^ R.Phone = P.Phone)}
```

<b> 2. A query that returns the ID of all people who are both enemies and members. </b>

* Relational algebra
`∏`<sub>`ID`</sub>`(Enemy)` ∩ `∏`<sub>`ID`</sub>`(Member)`

* Relational calculus
```
{R | ∃e ∈ Enemey (R.ID = e.ID)}
∩
{P | ∃M ∈ Member (P.ID = M.ID)}

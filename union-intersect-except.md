## UNION
* The UNION operator combines result sets of two or more SELECT statements
  into a single result set.

* The UNION operator removes all duplicate rows from the combined data set.
* To retain the duplicate rows, you use the the UNION ALL instead.
</br>

* The number and the order of the columns in the select list of both queries must be the same.
* The data types of respective columns must be compatible.

<b> Skeleton </b>
```
SELECT select_list_1
FROM table_expresssion_1
UNION
SELECT select_list_2
FROM table_expression_2
```
<b> Diagram </b>
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░██████████████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░████████████████████████░░░░░░░░░░░░░░░░
░░░░░░░░██████████████████████████░░░░░░░░░░░░░░░
░░░░░░░████████████████████████████░░░░░░░░░░░░░░
░░░░░░░░██████████████████████████░░░░░░░░░░░░░░░
░░░░░░░░░████████████████████████░░░░░░░░░░░░░░░░
░░░░░░░░░░██████████████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```

## INTERSECT
* The INTERSECT operator combines result sets of two or more SELECT statements
  into a single result set.
* The INTERSECT operator returns any rows that are available in both result sets.
</br>

* The number and the order of the columns in the select list of both queries must be the same.
* The data types of respective columns must be compatible.

<b> Skeleton </b>
```
SELECT select_list
FROM A
INTERSECT
SELECT select_list
FROM B;
```
<b> Diagram </b>
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░██████░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░██░░░░░░░░████████░░░░░░░░██░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░██████░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```

## EXCEPT
* The EXCEPT operator returns rows by comparing the result sets of two or more queries.
* The EXCEPT operator returns distinct rows from the first (left) query
  that are not in the output of the second (right) query.
</br>

* The number and the order of the columns in the select list of both queries must be the same.
* The data types of respective columns must be compatible.

<b> Skeleton </b>
```
SELECT select_list
FROM A
EXCEPT 
SELECT select_list
FROM B;
```
<b> Diagram </b>
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░█████████████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░██████████████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░████████████████░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░██████████████████░░░░░░░░██░░░░░░░░░░░░░░
░░░░░░░░████████████████░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░░░██████████████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░░░█████████████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```
## JOINS - venn diagrams

### INNER JOIN
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

SELECT <select list>
FROM TableA A
INNER JOIN Table B
ON A.key = B.key
```
## LEFT JOIN
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

SELECT <select list>
FROM TableA A
LEFT JOIN TableB B
ON A.key = B.key
```
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░█████████████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░██████████████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░████████████░░██░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░████████████░░░░██░░░░░░░░██░░░░░░░░░░░░░░
░░░░░░░░████████████░░██░░░░░░░░██░░░░░░░░░░░░░░░
░░░░░░░░░██████████████░░░░░░░░██░░░░░░░░░░░░░░░░
░░░░░░░░░░█████████████░░░░░░███░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

SELECT <select list>
FROM TableA A
LEFT JOIN TableB B
ON A.key = B.key
WHERE B.key IS NULL
```
## RIGHT JOIN
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░█████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░██████████████░░░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░████████████████░░░░░░░░░░░░░░░
░░░░░░░██░░░░░░░░██████████████████░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░████████████████░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░██████████████░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░█████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

SELECT <select list>
FROM TableA A
RIGHT JOIN TableB B
ON A.key = B.key
```

```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░█████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░██████████████░░░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░██░░████████████░░░░░░░░░░░░░░░
░░░░░░░██░░░░░░░░██░░░░████████████░░░░░░░░░░░░░░
░░░░░░░░██░░░░░░░░██░░████████████░░░░░░░░░░░░░░░
░░░░░░░░░██░░░░░░░░██████████████░░░░░░░░░░░░░░░░
░░░░░░░░░░███░░░░░░█████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

SELECT <select list>
FROM TableA A
RIGHT JOIN TableB B
ON A.key = B.key
WHERE A.key IS NULL
```
## FULL OUTER JOIN
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

SELECT <select list>
FROM TableA A
FULL OUTER JOIN TableB B
ON A.key = B.key
```
```
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░██████████████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░████████████████████████░░░░░░░░░░░░░░░░
░░░░░░░░████████████░░████████████░░░░░░░░░░░░░░░
░░░░░░░████████████░░░░████████████░░░░░░░░░░░░░░
░░░░░░░░████████████░░████████████░░░░░░░░░░░░░░░
░░░░░░░░░████████████████████████░░░░░░░░░░░░░░░░
░░░░░░░░░░██████████████████████░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░████████░░████████░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

SELECT <select list>
FROM TableA A
FULL OUTER JOIN TableB B
ON A.key = B.key
WHERE A.key IS NULL
OR B.key IS NULL
```
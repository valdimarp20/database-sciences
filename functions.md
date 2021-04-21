## FUNCTIONS
* also known as stored procedures.
### Function skeleton
```
create [or replace] function function_name(param_list)
   returns return_type 
   language plpgsql
  as
$$
declare 
-- variable declaration
begin
 -- logic
end;
$$
```

* First, specify the name of the function after the create/replace keyword.
* Second, specify the parameter list surrounded by parentheses after the function name.
    * Can have zero or many parameter.
* Third, specify the datatype of the returned value after the returns keyword.
* Fourth, use the language plpgsql to specify the procedural language of the function.
* Finally, place a block in the dollar-quoted string constant.

### Examples
```
CREATE FUNCTION get_film_count(len_from int, len_to int)
RETURNS int
LANGUAGE plpgsql
AS
$$
DECLARE
   film_count integer;
BEGIN
   SELECT COUNT(*) 
   INTO film_count
   FROM film
   WHERE length between len_from AND len_to;
   
   RETURN film_count;
END;
$$;
```
* The following statement creates a function that counts the films whose length between the len_from and len_to parameters:
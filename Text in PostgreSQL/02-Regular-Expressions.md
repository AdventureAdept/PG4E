# Autograder: Regular Expressions

This assignment requires writing a PostgreSQL regular expression to filter rows in the `purpose` column of the `taxdata` table that **contain a colon (:), comma (,), or semicolon (;)**.

---

## Understand the Table

Check the structure of the `taxdata` table (optional):

```sql
\d+ taxdata
```

## SQL Query Format

You are only required to write the **regular expression**, but for testing, here's how the full query would look:

```sql
SELECT purpose 
FROM taxdata 
WHERE purpose ~ '[:;,]' 
ORDER BY purpose 
LIMIT 3;
```

## Explanation of Regular Expression

```regex
[:;,]
```

* `[...]` is a **character class.**
* Inside it:
  * `:` matches a colon
  * `;` matches a semicolon
  * `,` matches a comma
This expression matches **any line that includes at least one of those characters anywhere.**

## Use this in the autograder input box.

```regex
[:;,]
```

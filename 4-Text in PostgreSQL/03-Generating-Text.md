# Generating Text in PostgreSQL

---

## Step 1: Create the `bigtext` Table

```sql
DROP TABLE IF EXISTS bigtext;

CREATE TABLE bigtext (
  content TEXT
);
```

## Step 2: Insert 100,000 Records Using generate_series()

```sql
INSERT INTO bigtext (content)
SELECT 
  'This is record number ' || i || ' of quite a few text records.'
FROM generate_series(100000, 199999) AS s(i);
```

## Step 3: Verify Insert

```sql
SELECT * FROM bigtext ORDER BY content LIMIT 5;
```

## Example output:

```vbnet
This is record number 100000 of quite a few text records.
This is record number 100001 of quite a few text records.
...
```

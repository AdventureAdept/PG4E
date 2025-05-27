# Autograder: Inserting some data into a table

## Instructions

In this assignment, you will create a table named `ages` and insert a few rows into it.

---

## Step 1: Create the `ages` Table

Run the following SQL to create the table:

```sql
CREATE TABLE ages (
  name VARCHAR(128),
  age INTEGER
);
```
## Step 2: Clear Existing Data

Make sure the table is empty by deleting any existing rows:

```sql
DELETE FROM ages;
```

## Step 3: Insert the Rows

Insert the following rows into the `ages` table:

```sql
INSERT INTO ages (name, age) VALUES ('Deryn', 32);
INSERT INTO ages (name, age) VALUES ('Malachai', 36);
INSERT INTO ages (name, age) VALUES ('Mehreen', 20);
INSERT INTO ages (name, age) VALUES ('Meryl', 14);
INSERT INTO ages (name, age) VALUES ('Rhia', 37);
```

## Step 4: Verify Your Data

```sql
SELECT * FROM ages;
```
### Expected Output:

| name     | age |
| -------- | --- |
| Deryn    | 32  |
| Malachai | 36  |
| Mehreen  | 20  |
| Meryl    | 14  |
| Rhia     | 37  |


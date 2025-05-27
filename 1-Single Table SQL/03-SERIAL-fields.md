# Autograder: Serial Fields / Auto Increment

## Objectives

Create a table named `automagic` with the following structure:

- `id`: auto-incrementing integer using `SERIAL`, set as the primary key
- `name`: a `VARCHAR(32)` that is **required** (i.e., `NOT NULL`)
- `height`: a `FLOAT` that is **required** (i.e., `NOT NULL`)

The autograder will insert rows into this table to verify that the schema is correct.

---

## Step 1: Create the Table

Run the following SQL to create the `automagic` table:

```sql
CREATE TABLE automagic (
  id SERIAL PRIMARY KEY,
  name VARCHAR(32) NOT NULL,
  height FLOAT NOT NULL
);
```

# Step 2: Verify the Table

After creating the table, you can verify the structure by running:

```sql
\d automatic
```

To check if the table is ready and accessible, you can also run:

```sql
SELECT * FROM automatic;
```

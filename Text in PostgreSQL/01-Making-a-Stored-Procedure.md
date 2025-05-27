# Autograder: Making a Stored Procedure

This guide shows how to create a `keyvalue` table with a trigger-based stored procedure that automatically updates the `updated_at` timestamp whenever a row is updated.

---

## Step 1: Create the `keyvalue` Table

```sql
DROP TABLE IF EXISTS keyvalue CASCADE;

CREATE TABLE keyvalue ( 
  id SERIAL,
  key VARCHAR(128) UNIQUE,
  value VARCHAR(128) UNIQUE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  PRIMARY KEY(id)
);
```

## Step 2: Create the Trigger Function

```sql
CREATE OR REPLACE FUNCTION update_modified_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

## Step 3: Create the Trigger on the Table
```sql
CREATE TRIGGER set_updated_at
BEFORE UPDATE ON keyvalue
FOR EACH ROW
EXECUTE FUNCTION update_modified_column();
```
## Step 4: Verify the Result

Run this query to check correctness:

```sql
INSERT INTO keyvalue(key, value) VALUES ('x', '1');
-- Wait a few seconds, then:
UPDATE keyvalue SET value = '2' WHERE key = 'x';
-- Then:
SELECT * FROM keyvalue;
```

You should see `updated_at` change when the row is updated.

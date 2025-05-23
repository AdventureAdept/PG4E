# Autograder: Entering Many-to-One Data - Automobiles

## Objectives

In this assignment, you will:

- Create two normalized tables: `make` and `model`
- Insert automobile data while maintaining proper many-to-one relationships
- Ensure foreign key constraints are respected

---

## Step 1: Create the Tables

Run the following SQL to create the `make` and `model` tables:

```sql
CREATE TABLE make (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

CREATE TABLE model (
  id SERIAL,
  name VARCHAR(128),
  make_id INTEGER REFERENCES make(id) ON DELETE CASCADE,
  PRIMARY KEY(id)
);
```
# Step 2: Insert Data into make

Insert unique car manufacturers:

```sql
INSERT INTO make (name) VALUES ('Chrysler');
INSERT INTO make (name) VALUES ('Volvo');
```
# Step 3: Insert Data into model

First, retrieve the IDs from the `make` table:

```sql
SELECT * FROM make;
```

Then use those IDs in the following inserts (adjust IDs based on your query result):

```sql
-- Assuming Chrysler has id = 1 and Volvo has id = 2
INSERT INTO model (name, make_id) VALUES ('LeBaron', 1);
INSERT INTO model (name, make_id) VALUES ('LeBaron Convertible', 1);
INSERT INTO model (name, make_id) VALUES ('LeBaron GTS', 1);
INSERT INTO model (name, make_id) VALUES ('960/S90', 2);
INSERT INTO model (name, make_id) VALUES ('C30 FWD', 2);
```
Step 4: Verify the Data

Run the following query to check your work:

```sql
SELECT make.name, model.name
FROM model
JOIN make ON model.make_id = make.id
ORDER BY make.name LIMIT 5;
```
# Expected Output
 
| name     | name                |
| -------- | ------------------- |
| Chrysler | LeBaron             |
| Chrysler | LeBaron Convertible |
| Chrysler | LeBaron GTS         |
| Volvo    | 960/S90             |
| Volvo    | C30 FWD             |

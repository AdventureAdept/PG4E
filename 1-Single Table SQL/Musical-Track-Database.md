# Autograder: Musical Track Database

## Objectives

In this assignment, you will:

- Create a table named `track_raw` to store data from an iTunes-style CSV file.
- Load the data from a provided CSV file into your PostgreSQL database.
- Verify the data with a test query.

This assignment focuses on managing a many-to-one relationship between tracks and albums, ignoring the artist field.

---

## Step 1: Download the CSV File

Download the `library.csv` file using one of the following commands in your terminal:

```bash
wget https://www.pg4e.com/tools/sql/library.csv
```
#### or
```bash
curl -O https://www.pg4e.com/tools/sql/library.csv
```
If neither command works, manually download the file and place it in the same directory as your SQL client or Jupyter environment.

# Step 2: Create the Table

Create the `track_raw` table using the following SQL:
```sql
CREATE TABLE track_raw (
  title TEXT,
  artist TEXT,
  album TEXT,
  count INTEGER,
  rating INTEGER,
  len INTEGER
);
```

# Step 3: Clear Existing Data (Optional but Recommended)
If you have run this assignment before, make sure to clear the table before reloading the data
```sql
DELETE FROM track_raw;
```
# Step 4: Load the CSV File
Use the `\copy` command in `psql` to load the data:

```sql
\copy track_raw(title,artist,album,count,rating,len) FROM 'library.csv' WITH DELIMITER ',' CSV;
```

# Step 5: Verify the Data
Run the following SQL to verify the data was loaded correctly:

```sql
SELECT title, album FROM track_raw ORDER BY title LIMIT 3;
```
| title                      | album                              |
| -------------------------- | ---------------------------------- |
| A Boy Named Sue (live)     | The Legend Of Johnny Cash          |
| A Brief History of Packets | Computing Conversations            |
| Aguas De Marco             | Natural Wonders Music Sampler 1999 |

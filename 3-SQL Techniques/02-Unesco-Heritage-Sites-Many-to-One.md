# Autograder: Unesco Heritage Sites Many-to-One
This assignment demonstrates how to normalize UNESCO heritage site data from a CSV file into properly normalized tables with many-to-one relationships.

---

## Schema Setup

Start by creating the raw and lookup tables. Run the following SQL commands:

```sql
DROP TABLE IF EXISTS unesco_raw;
CREATE TABLE unesco_raw (
  name TEXT,
  description TEXT,
  justification TEXT,
  year INTEGER,
  longitude FLOAT,
  latitude FLOAT,
  area_hectares FLOAT,
  category TEXT,
  category_id INTEGER,
  state TEXT,
  state_id INTEGER,
  region TEXT,
  region_id INTEGER,
  iso TEXT,
  iso_id INTEGER
);

DROP TABLE IF EXISTS category;
CREATE TABLE category (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS state;
CREATE TABLE state (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS region;
CREATE TABLE region (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS iso;
CREATE TABLE iso (
  id SERIAL,
  name VARCHAR(10) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS unesco;
CREATE TABLE unesco (
  id SERIAL,
  name TEXT,
  description TEXT,
  justification TEXT,
  year INTEGER,
  longitude FLOAT,
  latitude FLOAT,
  area_hectares FLOAT,
  category_id INTEGER REFERENCES category(id),
  state_id INTEGER REFERENCES state(id),
  region_id INTEGER REFERENCES region(id),
  iso_id INTEGER REFERENCES iso(id),
  PRIMARY KEY(id)
);
```
## Import CSV Data into unesco_raw

Load your UNESCO CSV data into the `unesco_raw` table using the `\copy` command (replace `'whc-sites-2018-small.csv'` with your file path or place in the same folder as the terminal):

```sql
\copy unesco_raw(name,description,justification,year,longitude,latitude,area_hectares,category,state,region,iso) FROM 'whc-sites-2018-small.csv' WITH DELIMITER ',' CSV HEADER;
```

## Normalize Lookup Tables

Insert distinct values into each lookup table:

```sql
INSERT INTO category(name)
SELECT DISTINCT category FROM unesco_raw WHERE category IS NOT NULL;

INSERT INTO state(name)
SELECT DISTINCT state FROM unesco_raw WHERE state IS NOT NULL;

INSERT INTO region(name)
SELECT DISTINCT region FROM unesco_raw WHERE region IS NOT NULL;

INSERT INTO iso(name)
SELECT DISTINCT iso FROM unesco_raw WHERE iso IS NOT NULL;
```

## Update unesco_raw with Foreign Keys

Set the foreign key columns in `unesco_raw` by matching names with their respective lookup tables:

```sql
UPDATE unesco_raw SET category_id = category.id FROM category WHERE unesco_raw.category = category.name;

UPDATE unesco_raw SET state_id = state.id FROM state WHERE unesco_raw.state = state.name;

UPDATE unesco_raw SET region_id = region.id FROM region WHERE unesco_raw.region = region.name;

UPDATE unesco_raw SET iso_id = iso.id FROM iso WHERE unesco_raw.iso = iso.name;
```

## Populate the Normalized unesco Table

Copy the normalized data into the `unesco` table, excluding redundant text fields:

```sql
INSERT INTO unesco (
  name, description, justification, year,
  longitude, latitude, area_hectares,
  category_id, state_id, region_id, iso_id
)
SELECT
  name, description, justification, year,
  longitude, latitude, area_hectares,
  category_id, state_id, region_id, iso_id
FROM unesco_raw
WHERE name IS NOT NULL;
```

## Verify Your Results

Run the following query to verify normalized data:

```sql
SELECT unesco.name, year, category.name, state.name, region.name, iso.name
FROM unesco
JOIN category ON unesco.category_id = category.id
JOIN state ON unesco.state_id = state.id
JOIN region ON unesco.region_id = region.id
JOIN iso ON unesco.iso_id = iso.id
ORDER BY category.name, unesco.name
LIMIT 3;
```

## Expected Output:

| Name                       | Year | Category | State        | Region                   | iso |
| -------------------------- | ---- | -------- | ------------ | ------------------------ | --- |
| Khomani Cultural Landscape | 2017 | Cultural | South Africa | Africa                   | za  |
| al Saflieni Hypogeum       | 1980 | Cultural | Malta        | Europe and North America | mt  |
| ingvellir National Park    | 2004 | Cultural | Iceland      | Europe and North America | is  |

## Reset Tables for Re-run

If you want to clear all data and restart:

```sql
TRUNCATE unesco, unesco_raw, category, state, region, iso RESTART IDENTITY CASCADE;
```

## Notes

* Make sure to run all steps in order.

* The `unesco_raw` table holds the unnormalized raw data.

* The assignment focuses on proper normalization with foreign keys to lookup tables.

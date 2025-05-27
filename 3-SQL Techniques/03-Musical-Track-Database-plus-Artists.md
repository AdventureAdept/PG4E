# Autograder: Musical Track Database plus Artists

This assignment demonstrates how to normalize an iTunes music library (CSV format) into properly structured relational tables, including a **many-to-many relationship** between tracks and artists using a **junction table**.

---

## Step 1: Table Setup

Run the following SQL to set up the schema:

```sql
DROP TABLE IF EXISTS tracktoartist CASCADE;
DROP TABLE IF EXISTS artist CASCADE;
DROP TABLE IF EXISTS track CASCADE;
DROP TABLE IF EXISTS album CASCADE;

CREATE TABLE album (
  id SERIAL,
  title VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

CREATE TABLE track (
  id SERIAL,
  title TEXT, 
  artist TEXT, 
  album TEXT, 
  album_id INTEGER REFERENCES album(id) ON DELETE CASCADE,
  count INTEGER, 
  rating INTEGER, 
  len INTEGER,
  PRIMARY KEY(id)
);

CREATE TABLE artist (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

CREATE TABLE tracktoartist (
  id SERIAL,
  track VARCHAR(128),
  track_id INTEGER REFERENCES track(id) ON DELETE CASCADE,
  artist VARCHAR(128),
  artist_id INTEGER REFERENCES artist(id) ON DELETE CASCADE,
  PRIMARY KEY(id)
);
```

## Step 2: Import CSV Data

Load your `library.csv` file into the `track` table directly:

```sql
\copy track(title,artist,album,count,rating,len) FROM 'library.csv' WITH DELIMITER ',' CSV HEADER;
```

## Step 3: Normalize Album Data

Insert distinct album titles and update `track.album_id`:

```sql
INSERT INTO album (title)
SELECT DISTINCT album FROM track
WHERE album IS NOT NULL;

UPDATE track
SET album_id = album.id
FROM album
WHERE track.album = album.title;
```

## Step 4: Normalize Artist Data

Insert distinct artist names into the `artist` table:

```sql
INSERT INTO artist (name)
SELECT DISTINCT artist FROM track
WHERE artist IS NOT NULL;
```

## Step 5: Create Junction Entries (tracktoartist)

Insert rows into the join table `tracktoartist`:

```sql
INSERT INTO tracktoartist (track, artist)
SELECT DISTINCT title, artist FROM track
WHERE title IS NOT NULL AND artist IS NOT NULL;
```

Update the foreign key columns in `tracktoartist`:

```sql
UPDATE tracktoartist
SET track_id = track.id
FROM track
WHERE tracktoartist.track = track.title;

UPDATE tracktoartist
SET artist_id = artist.id
FROM artist
WHERE tracktoartist.artist = artist.name;
```

## Step 6: Drop Unnormalized Columns

Remove the now-unneeded text columns:

```sql
ALTER TABLE track DROP COLUMN album;
ALTER TABLE track DROP COLUMN artist;
ALTER TABLE tracktoartist DROP COLUMN track;
ALTER TABLE tracktoartist DROP COLUMN artist;
```

## Step 7: Verify the Result

Run this query to check correctness:

```sql
SELECT track.title, album.title, artist.name
FROM track
JOIN album ON track.album_id = album.id
JOIN tracktoartist ON track.id = tracktoartist.track_id
JOIN artist ON tracktoartist.artist_id = artist.id
ORDER BY track.title
LIMIT 3;
```

## Expected Output:

| title                      | album                              | artist                |
| -------------------------- | ---------------------------------- | --------------------- |
| A Boy Named Sue (live)     | The Legend Of Johnny Cash          | Johnny Cash           |
| A Brief History of Packets | Computing Conversations            | IEEE Computer Society |
| Aguas De Marco             | Natural Wonders Music Sampler 1999 | Rosa Passos           |

## Reset Tables for Re-run

If needed, clear everything and start fresh:

```sql
TRUNCATE tracktoartist, artist, track, album RESTART IDENTITY CASCADE;
```

## Notes

* This assignment demonstrates many-to-many normalization using a join table.

* Avoid duplicate entries by using `DISTINCT` and checking for `NULL`.

* Dropping columns is essential to match the expected schema.

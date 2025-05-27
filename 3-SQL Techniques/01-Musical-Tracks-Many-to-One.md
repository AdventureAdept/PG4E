# Autograder: Musical Tracks Many-to-One

This assignment demonstrates how to normalize an iTunes CSV export into relational tables with a many-to-one relationship between tracks and albums.

---

## Schema Setup

Run these SQL commands to create the necessary tables:

```sql
DROP TABLE IF EXISTS track;
DROP TABLE IF EXISTS album;
DROP TABLE IF EXISTS track_raw;

CREATE TABLE album (
  id SERIAL,
  title VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

CREATE TABLE track (
  id SERIAL,
  title VARCHAR(128),
  len INTEGER,
  rating INTEGER,
  count INTEGER,
  album_id INTEGER REFERENCES album(id) ON DELETE CASCADE,
  UNIQUE(title, album_id),
  PRIMARY KEY(id)
);

CREATE TABLE track_raw (
  title TEXT,
  artist TEXT,
  album TEXT,
  album_id INTEGER,
  count INTEGER,
  rating INTEGER,
  len INTEGER
);
```

## Import CSV Data into track_raw

Load your iTunes CSV data into the `track_raw` table using the `\copy command` (replace `'tracks.csv'` with your file path):

```sql
\copy track_raw(title, artist, album, count, rating, len) FROM 'tracks.csv' DELIMITER ',' CSV HEADER;
```

## Populate album Table
Insert all distinct albums into the `album` table:

```sql
INSERT INTO album (title)
SELECT DISTINCT album
FROM track_raw
WHERE album IS NOT NULL;
```

## Update track_raw to Reference Albums

Set the `album_id` foreign key in `track_raw` by matching album titles:

```sql
UPDATE track_raw
SET album_id = album.id
FROM album
WHERE track_raw.album = album.title;
```

## Populate track Table
Insert the normalized track data, dropping the artist and album text fields:

```sql
INSERT INTO track (title, len, rating, count, album_id)
SELECT title, len, rating, count, album_id
FROM track_raw
WHERE title IS NOT NULL AND album_id IS NOT NULL;
```

## Verify Your Results
Run this query to check if data was normalized correctly:

```sql
SELECT track.title, album.title
FROM track
JOIN album ON track.album_id = album.id
ORDER BY track.title
LIMIT 3;
```

## Expected Output

| track                      | album                              |
| -------------------------- | ---------------------------------- |
| A Boy Named Sue (live)     | The Legend Of Johnny Cash          |
| A Brief History of Packets | Computing Conversations            |
| Aguas De Marco             | Natural Wonders Music Sampler 1999 |

## Reset Tables for Re-run

If you need to clear tables and start fresh, use:

```sql
TRUNCATE track, album, track_raw RESTART IDENTITY CASCADE;
```
## Notes

* The `artist` field is ignored for this assignment.

* Ensure your CSV file matches the expected format with headers: `title, artist, album, count, rating, len`.

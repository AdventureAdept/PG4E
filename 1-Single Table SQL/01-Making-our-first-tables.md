# Autograder: Making our first tables
This guide helps you create the PostgreSQL tables needed for grading and debugging in the **PostgreSQL for Everybody (PG4E)** course.

---

## Step 1: Connect to Your Database

Use the following command in your terminal to connect using `psql`. Replace the placeholders with your actual values:

```bash
psql -h <host> -p <port> -U <username> <database_name>
```
## Step 2: Create the Tables
```sql
CREATE TABLE IF NOT EXISTS pg4e_debug (
  id SERIAL,
  query VARCHAR(4096),
  result VARCHAR(4096),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY(id)
);

CREATE TABLE IF NOT EXISTS pg4e_result (
  id SERIAL,
  link_id INTEGER UNIQUE,
  score FLOAT,
  title VARCHAR(4096),
  note VARCHAR(4096),
  debug_log VARCHAR(8192),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP
);

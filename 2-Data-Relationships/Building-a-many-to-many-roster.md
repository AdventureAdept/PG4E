# Autograder: Building a Many-to-Many Roster

## Objectives

In this assignment, you will:

- Create three normalized tables: `student`, `course`, and `roster`
- Model a many-to-many relationship between students and courses
- Properly encode roles (instructor = `1`, learner = `0`)
- Verify your data with a JOIN query

---

## Step 1: Create the Tables

Run the following SQL commands to create the tables:

```sql
CREATE TABLE student (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS course CASCADE;
CREATE TABLE course (
  id SERIAL,
  title VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS roster CASCADE;
CREATE TABLE roster (
  id SERIAL,
  student_id INTEGER REFERENCES student(id) ON DELETE CASCADE,
  course_id INTEGER REFERENCES course(id) ON DELETE CASCADE,
  role INTEGER,
  UNIQUE(student_id, course_id),
  PRIMARY KEY(id)
);
```
# Step 2: Insert Students

```sql
INSERT INTO student (name) VALUES 
('Aimiee'), ('Derin'), ('Leeona'), ('Macy'), ('Yahya'),
('Jayme'), ('Katia'), ('Prabhasees'), ('Tembe'), ('Tye'),
('Zahide'), ('Aaiva'), ('Babur'), ('Dionne'), ('Keiryn');
```

# Step 3: Insert Courses

```sql
INSERT INTO course (title) VALUES 
('si106'), ('si110'), ('si206');
```
# Step 4: Insert Data into roster
```sql
INSERT INTO roster (student_id, course_id, role) VALUES
-- si106
(1, 1, 1),
(2, 1, 0),
(3, 1, 0),
(4, 1, 0),
(5, 1, 0),

-- si110
(6, 2, 1),
(7, 2, 0),
(8, 2, 0),
(9, 2, 0),
(10, 2, 0),

-- si206
(11, 3, 1),
(12, 3, 0),
(13, 3, 0),
(14, 3, 0),
(15, 3, 0);
```

# Step 5: Verify Your Data

Run the following query:

```sql
SELECT student.name, course.title, roster.role
FROM student 
JOIN roster ON student.id = roster.student_id
JOIN course ON roster.course_id = course.id
ORDER BY course.title, roster.role DESC, student.name;
```
# Expected Output:

| student    | course | role |
| ---------- | ------ | ---- |
| Aimiee     | si106  | 1    |
| Derin      | si106  | 0    |
| Leeona     | si106  | 0    |
| Macy       | si106  | 0    |
| Yahya      | si106  | 0    |
| Jayme      | si110  | 1    |
| Katia      | si110  | 0    |
| Prabhasees | si110  | 0    |
| Tembe      | si110  | 0    |
| Tye        | si110  | 0    |
| Zahide     | si206  | 1    |
| Aaiva      | si206  | 0    |
| Babur      | si206  | 0    |
| Dionne     | si206  | 0    |
| Keiryn     | si206  | 0    |

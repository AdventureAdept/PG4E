# Puzzle: Break a Hashing Function
> **Try solving it yourself first!**  
> This puzzle is a great exercise in understanding how hashing functions work. You’re encouraged to explore it on your own before reading further.

---

## The Problem

You're given a Python hashing function that calculates a value from an input string using a weighted sum of character ASCII values:

```python
hv = 0
pos = 0
for let in txt:
    pos = (pos % 4) + 1  
    hv = (hv + (pos * ord(let))) % 1000000
```

The goal is to find **two different strings** that **produce the same hash value** (a **hash collision**). Each string must be between **3 and 10 characters** long.

## A Working Example

Two strings that produce a hash collision are:

* `ABCDE`
* `EBCDA`

### Let's see how:

**For** `ABCDE`:

| Char | pos | ASCII | pos × ASCII | Total   |
| ---- | --- | ----- | ----------- | ------- |
| A    | 1   | 65    | 65          | 65      |
| B    | 2   | 66    | 132         | 197     |
| C    | 3   | 67    | 201         | 398     |
| D    | 4   | 68    | 272         | 670     |
| E    | 1   | 69    | 69          | **739** |

**For** `EBCDA`:

| Char | pos | ASCII | pos × ASCII | Total   |
| ---- | --- | ----- | ----------- | ------- |
| E    | 1   | 69    | 69          | 69      |
| B    | 2   | 66    | 132         | 201     |
| C    | 3   | 67    | 201         | 402     |
| D    | 4   | 68    | 272         | 674     |
| A    | 1   | 65    | 65          | **739** |

Despite the different order, both strings result in the same final hash value: **739**.

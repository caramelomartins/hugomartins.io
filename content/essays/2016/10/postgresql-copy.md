---
title: Extracting Data from PostgreSQL
date: "2016-10-10T00:00:00"
aliases: ["/blog/extracting-data-from-postgresql/"]
category:
  - snippets
  - postgresql
---

PostgreSQL has an excellent module built specifically for the purpose of extracting data between a file and a table.

This can be used to extract several data in three formats:
- Text
- CSV
- Binary

Sometimes you'd want to extract data to a folder different than the one the database is in. You'd want to extract the data to the machine your logged on instead. This can be done using `\copy` instead of `COPY`.

An example of basic usage for extracting data would be, in CSV:

```
\copy (SELECT * FROM foo WHERE bar IS NOT NULL) TO '/path/to/folder' HEADER CSV
```

This simple snippet is incredibly useful to generate reports or extract data for analysis, simply and quickly.

---

Source:
 - https://www.postgresql.org/docs/9.5/static/sql-copy.html

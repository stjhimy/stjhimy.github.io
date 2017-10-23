---
layout: post
title:  Copy postgres query results to file
date:   2017-10-23
categories: #off-topic
---

```bash
COPY (query)
TO 'file';
```

```bash
COPY (select * from users)
TO '/tmp/users.csv';
```

[https://www.postgresql.org/docs/9.2/static/sql-copy.html](https://www.postgresql.org/docs/9.2/static/sql-copy.html)

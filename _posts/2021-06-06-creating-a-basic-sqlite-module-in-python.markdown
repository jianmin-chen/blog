---
categories: python sqlite
date: 2021-06-06 01:45:35 -0500
layout: post
tags: python sqlite
title: Creating a Basic SQLite Module in Python
---
![SQLite icon](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/SQLite370.svg/1200px-SQLite370.svg.png)

By default, Python provides a module known as `sqlite3` inside its standard library that can be used to interact with SQLite databases. If you understand how it works, you can actually use it to write a little module in Python that allows you to execute SQL commands. That's exactly what we're going to do today!

## An Example Program
By the end of this article, the following code will work:
```python
from sql import SQL

open("test.db", "w").close()
db = SQL("test.db")

db.execute("CREATE TABLE people (id INTEGER, name TEXT, PRIMARY KEY(id))")

first_id = db.execute("INSERT INTO people (name) VALUES ('Amanda')")
print(first_id)  # => 1

db.execute("INSERT INTO people (name) VALUES ('Bob')")
print(db.execute("SELECT * FROM people"))  # => [(1, 'Amanda'), (2, 'Bob')]
```
Let's get started writing the module! To write it, you're going to need a good understanding of how `sqlite3` works. [Here](https://docs.python.org/3/library/sqlite3.html)'s a quick reference if you need it.

## Setting Up
The first step is to create a new file named `sql.py`, and import `sqlite3`:
```python
import sqlite3
```
Next, we need to talk about how our module is going to work. Basically, we'll have a class named `SQL`. It's going to be able to load SQLite databases, and execute SQL commands on that database. The first step in creating this class is to actually lay down the structure for it:
```python
import sqlite3


class SQL:
    def __init__(self, path):
        """
            Initialization function to create a connection to the SQLite database.
        """
        pass

    def execute(self, query, *placeholders):
        """
            Execute function to execute a query based on the amount of placeholders.
        """
        pass
```
As you can see, the class has two methods: `__init__()`, which loads or *connects* to the SQLite database, and `execute()`, which executes a SQL command and substitutes placeholder in the query with the items in `*placeholders`. For now, let's start working on the initialization method:
```python
import sqlite3


class SQL:
    def __init__(self, path):
        """
            Initialization function to create a connection to the SQLite database.
        """

        self.db = sqlite3.connect(path, check_same_thread=False)

    def execute(self, query, *placeholders):
        """
            Execute function to execute a query based on the amount of placeholders.
        """
        pass
```
All we're doing here is using `sqlite3` to connect to a database. The option `check_same_thread` parameter is used so that errors that occur from running commands in the same thread don't happen, which is useful if you're using a SQLite database in a multi-threaded application.

Okay, we can now move onto the `execute()` method. This method is actually quite easy. First, we need to set up a "cursor" to interact with the SQLite database that's been loaded:
```python
cursor = self.db.cursor()
```
After we've done that, we can start executing the SQL query that's been provided. However, before we do that, we need to determine the number of placeholders. If placeholders are provided, we need to run the query with the placeholders. Otherwise, we can simply just run the query.
```python
if len(placeholders):
    # If there is at least one placeholder, add placeholders
    cursor.execute(query, placeholders)
else:
    cursor.execute(query)
```
Then, let's commit those changes so the database actually gets updated if necessary:
```python
# Commit to database
self.db.commit()
```
Perfect! The last step is to return the proper value, to represent what's been changed in the database. If the query provided is a `INSERT` query, we can simply return the ID of the last row, if applicable. If the query provided is a `SELECT` query, we need to return what's been selected. Otherwise, we can just return `None`. Here's the code for that:
```python
# Return certain value based upon query type
if query.upper().startswith("INSERT"):
    # INSERT query - return last row ID
    return cursor.lastrowid
elif query.upper().startswith("SELECT"):
    # SELECT query - return results of query
    return cursor.fetchall()

return None
```
Believe it or not, that's all there is to the module! Try the example program above - it should work!

## Conclusion
In this article, we built a little module that allows us to interact with SQLite databases. Obviously, this isn't as powerful as well-designed SQLite modules like `sqlalchemy`, but it works in smaller programs where you need a quick hack. The code for this article can be found [here](https://github.com/jianmin-chen/blog-programs/tree/main/Creating%20a%20Basic%20SQLite%20Module%20in%20Python). In the meantime, if you have any questions or comments, feel free to comment below!

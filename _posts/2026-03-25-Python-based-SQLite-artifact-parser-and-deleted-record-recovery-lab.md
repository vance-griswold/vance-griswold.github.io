## Python-based SQLite artifact parser and deleted-record recovery lab

Based on information from the Sheriffs Department on what their mobile forensics unit does.:

The unit primarily specializes in Mobile Forensics, focusing on the preservation, acquisition, and analysis of cellular devices, which is a critical source of evidence in modern investigations. We utilize specialized tools to extract, parse, and interpret complex datasets. As the industry shifts from SQLite databases toward other deeply nested data structures, your experience with Python and SQL would be particularly valuable to our work.  Both fields rely on the same technological DNA. If IIT is about building and managing the digital house, Mobile Forensics is about understanding how every brick was laid.  Both require a heavy understanding of SQL and SQLite. In IIT, you use it to manage data flow; in Mobile Forensics, you use it to recover deleted messages or location artifacts in app databases.  In IIT, Python automates administrative tasks; in Mobile Forensics, it is used to write custom scripts to parse data that commercial tools might miss.

Trying to familrize myself with these tools before going in.

tools: Sqlite, Sql, Python

---

### Creating SQLite Database

The first step of this project is creating the fake database to use for the deletion and they recovery of the data. There are datasets out there for this training purpose but since it is my first time I figuered I would just create my own.

#### create_db.py
Using sqlite in python, i was able to create a lab database and start to create tables like seen below with cursor.
```python
import sqlite3

connection = sqlite3.connect("lab.db")
cursor = connection.cursor()

cursor.execute('''
    CREATE TABLE IF NOT EXISTS messages (
        id        INTEGER PRIMARY KEY AUTOINCREMENT,
        contact_id INTEGER,
        content   TEXT NOT NULL,
        timestamp TEXT NOT NULL,
        direction TEXT NOT NULL
    )
''')
```

#### Adding Data
I then had ChatGPT create some data to insert into the db
```python
cursor.executemany('''
    INSERT INTO contacts (name, phone, email, notes)
    VALUES (?, ?, ?, ?)
''', [
    ("Greg Harmon",    "555-304-1782", "greg.harmon@email.com",  "College friend"),
    ("Sara Tillman",   "555-217-9934", "sara.t@webmail.net",     "Work contact"),
    ("Mike Delaney",   "555-408-5521", None,                     "Neighbor"),
    ("Priya Nath",     "555-619-2207", "priya.nath@inbox.com",   None),
    ("Carlos Mendez",  "555-713-8843", None,                     "Old roommate"),
```
Then use a horrible written python script to verify the data
#### verify_db.py
``` python
cursor.execute('''
               select *
               from messages
               ''')
row = cursor.fetchall()
print(row)
```

![](/images/test.png)

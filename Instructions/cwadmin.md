[Documentation Home](../README.md)

# CaseWorthy Administrator Instructions

This page will outline some of the basics of the day-to-day administration of CaseWorthy

1. Users
   1. Off-boarding
1. Household Composition Changes
1. Client Merges
1. Database Backups
1. SQL Server Management Studio (SSMS)
1. [Structured Query Language (SQL)](sql.md)
1. SQL Server Reporting Service (SSRS)
1. [`sqlGetter.py`](#sqlgetter.py)

---
---

## Users

Anybody at HRDC can access CaseWorthy with their manager's approval. It is generally a good idea to not provide login credentials until a new user has completed the introductory training.

### Where to configure users

You can create new users through the HRDC Admin role. Before creating a new user, do a search to make sure they do not already exist. This is done using the [Add User](../Forms/Baseline8.md) found on the [Users Summary](../Forms/Baseline7.md) form. Click-path shown below:

![User Setup](../Images/usersetup.png)

### Off-boarding

Once an employee leaves HRDC, their CaseWorthy privileges should be immediately revoked, unless otherwise specified. This is done by locking their account and making it inactive. This is accessed through the [Edit User](../Forms/Baseline8.md) form.

---
---

## Household Composition Changes

Sometimes clients need to be moved to different households. This section will cover the procedure for creating new households, moving clients into new households, and moving clients into existing households.

- Close all open program enrollments for the client being moved using the .
  - Identify which enrollments need to be closed using the [Program Enrollment](..Forms/1000000266.md) form.
  - Click path: Left-Nav bar - Case Management - Program Enrollment
  - Close open enrollments using the [Add/Edit Members](../Forms/Baseline49.md) form.
    - Change date from "Open" to the appropriate ending date.
    - Click path: Action gear - Member - "+ Add/Edit Members"
- View client family history with the [Client Families](../Forms/1000000048.md) form. Each row represents a family that this client has been a member of.

![Client Family History](../Images/clientfamilyhistory.png)

- If the client needs to go into a new family you can create it with the [ADMIN ONLY - Create a New Family](../Forms/1000000202.md) form
- If the client is moving into an existing family pull up the file of the HoH of the family that the client is moving into. Add the client using the [Add Family Member Spreadsheet](../Forms/Baseline7114.md).
- If the client is moving back into an old family you can simply change the date added on that row to the appropriate date, and change the date removed to "Open".

In most cases, a client should only be in one family at a time. Make sure that the date added to the new family is the same as the date removed from the old family. The most notable exception to this is children in a joint custody situation. 



---
---

## Client Merges


## [Structured Query Language (SQL)](sql.md)
## SQL Server Reporting Service (SSRS)


## `sqlGetter.py`

`sqlGetter.py` is a short Python script that allows you to access a local database instance through Python. To best use this tool, put this script in the same folder as the other Python script that is going to use it. Import the script using `from sqlGetter import *`. This will cause the whole script to run, which automatically establishes a connection to the database of your choice, and provides you with the `cnxn` object and the `pandize_data(query)` function.

Below is the script in it's entirety. To make it work on your local machine you will have to set `server` to the name of your local [SQL server]:

```python
import pyodbc
import pandas as pd

server = 'YOUR SQL SERVER'
database = 'HRDC09_Prod'

def connect(server, database):
	cnxn = pyodbc.connect('Trusted_Connection=Yes;DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database)	
	print('Connection to {} established on {}'.format(database,server))
	return cnxn	


def pandize_data(query):
	path = query+'.sql'
	with open(path, 'r') as file:
		sql = file.read()
	return pd.read_sql_query(sql,cnxn)

cnxn = connect(server, database)

```

### `connect(server, database)`

- Parameters
  - `server`: str
  - `database`: str
- Return Value
  - `cnxn`: pyodbc connection object


### `pandize_data(query)`

- Parameters
  - `query`: `str`,the name of the query to be used without the file suffix
    - e.g. `'clientdata'`
- Return Value
  - pandas dataframe object





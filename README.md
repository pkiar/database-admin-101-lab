# Database Admin 101 - Lab

## Introduction 

In this lab, you"ll go through the process of designing and creating a database. From there, you"ll begin to populate this table with mock data provided to you.

## Objectives

You will be able to:

* Use knowledge of the structure of databases to create a database and populate it

## The Scenario

You are looking to design a database for a school that will house various information from student grades to contact information, class roster lists and attendance. First, think of how you would design such a database. What tables would you include? What columns would each table have? What would be the primary means to join said tables?

## Creating the Database

Now that you"ve put a little thought into how you might design your database, it"s time to go ahead and create it! Start by import the necessary packages. Then, create a database called **school.sqlite**.


```python
import sqlite3 
import pandas as pd

con = sqlite3.connect("school.sqlite")
cur = con.cursor()

```


```python
ls

```

    contact_list.pickle  [0m[01;34menv[0m/         LICENSE.md  school.sqlite
    CONTRIBUTING.md      index.ipynb  README.md   school.sqlite-journal


## Create a Table for Contact Information

Create a table called contactInfo to house contact information for both students and staff. Be sure to include columns for first name, last name, role (student/staff), telephone number, street, city, state, and zipcode. Be sure to also create a primary key for the table. 


```python
cur.execute(""" CREATE TABLE Contactinfo ( 
                    Contact_Id INTEGER PRIMARY KEY,
                    fistName TEXT ,
                    lastName  TEXT,
                    role TEXT ,
                    telephone INTEGER,
                    street TEXT,
                    city TEXT,
                    state TEXT ,
                    zipcode TEXT
              )
            
           """)
```




    <sqlite3.Cursor at 0x723d091ca840>




```python
cur.execute("""  
                ALTER TABLE Contactinfo
                RENAME COLUMN fistName to firstName
             
            """)
```




    <sqlite3.Cursor at 0x723d091ca840>



## Populate the Table

Below, code is provided for you in order to load a list of dictionaries. Briefly examine the list. Each dictionary in the list will serve as an entry for your contact info table. Once you"ve briefly investigated the structure of this data, write a for loop to iterate through the list and create an entry in your table for each person"s contact info.


```python
# Load the list of dictionaries; just run this cell
import pickle

with open("contact_list.pickle", "rb") as f:
    contacts = pickle.load(f)
contacts    
```




    [{'firstName': 'Christine',
      'lastName': 'Holden',
      'role': 'staff',
      'telephone ': 2035687697,
      'street': '1672 Whitman Court',
      'city': 'Stamford',
      'state': 'CT',
      'zipcode ': '06995'},
     {'firstName': 'Christopher',
      'lastName': 'Warren',
      'role': 'student',
      'telephone ': 2175150957,
      'street': '1935 University Hill Road',
      'city': 'Champaign',
      'state': 'IL',
      'zipcode ': '61938'},
     {'firstName': 'Linda',
      'lastName': 'Jacobson',
      'role': 'staff',
      'telephone ': 4049446441,
      'street': '479 Musgrave Street',
      'city': 'Atlanta',
      'state': 'GA',
      'zipcode ': '30303'},
     {'firstName': 'Andrew',
      'lastName': 'Stepp',
      'role': 'student',
      'telephone ': 7866419252,
      'street': '2981 Lamberts Branch Road',
      'city': 'Hialeah',
      'state': 'Fl',
      'zipcode ': '33012'},
     {'firstName': 'Jane',
      'lastName': 'Evans',
      'role': 'student',
      'telephone ': 3259909290,
      'street': '1461 Briarhill Lane',
      'city': 'Abilene',
      'state': 'TX',
      'zipcode ': '79602'},
     {'firstName': 'Jane',
      'lastName': 'Evans',
      'role': 'student',
      'telephone ': 3259909290,
      'street': '1461 Briarhill Lane',
      'city': 'Abilene',
      'state': 'TX',
      'zipcode ': '79602'},
     {'firstName': 'Mary',
      'lastName': 'Raines',
      'role': 'student',
      'telephone ': 9075772295,
      'street': '3975 Jerry Toth Drive',
      'city': 'Ninilchik',
      'state': 'AK',
      'zipcode ': '99639'},
     {'firstName': 'Ed',
      'lastName': 'Lyman',
      'role': 'student',
      'telephone ': 5179695576,
      'street': '3478 Be Sreet',
      'city': 'Lansing',
      'state': 'MI',
      'zipcode ': '48933'}]




```python
pd.read_sql_query("SELECT * FROM Contactinfo", con)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Contact_Id</th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
contacts[0].keys()
```




    dict_keys(['firstName', 'lastName', 'role', 'telephone ', 'street', 'city', 'state', 'zipcode '])




```python
contacts[2].keys()
```




    dict_keys(['firstName', 'lastName', 'role', 'telephone ', 'street', 'city', 'state', 'zipcode '])




```python
# Clean all keys in all contacts by stripping whitespace
cleaned_contacts = []
for c in contacts:
    cleaned = {k.strip(): v for k, v in c.items()}
    cleaned_contacts.append(cleaned)

contacts = cleaned_contacts

```


```python
contacts
```




    [{'firstName': 'Christine',
      'lastName': 'Holden',
      'role': 'staff',
      'telephone': 2035687697,
      'street': '1672 Whitman Court',
      'city': 'Stamford',
      'state': 'CT',
      'zipcode': '06995'},
     {'firstName': 'Christopher',
      'lastName': 'Warren',
      'role': 'student',
      'telephone': 2175150957,
      'street': '1935 University Hill Road',
      'city': 'Champaign',
      'state': 'IL',
      'zipcode': '61938'},
     {'firstName': 'Linda',
      'lastName': 'Jacobson',
      'role': 'staff',
      'telephone': 4049446441,
      'street': '479 Musgrave Street',
      'city': 'Atlanta',
      'state': 'GA',
      'zipcode': '30303'},
     {'firstName': 'Andrew',
      'lastName': 'Stepp',
      'role': 'student',
      'telephone': 7866419252,
      'street': '2981 Lamberts Branch Road',
      'city': 'Hialeah',
      'state': 'Fl',
      'zipcode': '33012'},
     {'firstName': 'Jane',
      'lastName': 'Evans',
      'role': 'student',
      'telephone': 3259909290,
      'street': '1461 Briarhill Lane',
      'city': 'Abilene',
      'state': 'TX',
      'zipcode': '79602'},
     {'firstName': 'Jane',
      'lastName': 'Evans',
      'role': 'student',
      'telephone': 3259909290,
      'street': '1461 Briarhill Lane',
      'city': 'Abilene',
      'state': 'TX',
      'zipcode': '79602'},
     {'firstName': 'Mary',
      'lastName': 'Raines',
      'role': 'student',
      'telephone': 9075772295,
      'street': '3975 Jerry Toth Drive',
      'city': 'Ninilchik',
      'state': 'AK',
      'zipcode': '99639'},
     {'firstName': 'Ed',
      'lastName': 'Lyman',
      'role': 'student',
      'telephone': 5179695576,
      'street': '3478 Be Sreet',
      'city': 'Lansing',
      'state': 'MI',
      'zipcode': '48933'}]




```python
for contact in contacts:
    cur.execute("""
        INSERT INTO Contactinfo 
        (firstName, lastName, role, telephone, street, city, state, zipcode)
        VALUES (?, ?, ?, ?, ?, ?, ?, ?)
    """, (
        contact.get("firstName"),
        contact.get("lastName"),
        contact.get("role"),
        contact.get("telephone"),
        contact.get("street"),
        contact.get("city"),
        contact.get("state"),
        contact.get("zipcode")
    ))
```

**Query the Table to Ensure it is populated**


```python
pd.read_sql_query("SELECT * FROM Contactinfo", con)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Contact_Id</th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Christine</td>
      <td>Holden</td>
      <td>staff</td>
      <td>2035687697</td>
      <td>1672 Whitman Court</td>
      <td>Stamford</td>
      <td>CT</td>
      <td>06995</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Christopher</td>
      <td>Warren</td>
      <td>student</td>
      <td>2175150957</td>
      <td>1935 University Hill Road</td>
      <td>Champaign</td>
      <td>IL</td>
      <td>61938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Linda</td>
      <td>Jacobson</td>
      <td>staff</td>
      <td>4049446441</td>
      <td>479 Musgrave Street</td>
      <td>Atlanta</td>
      <td>GA</td>
      <td>30303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Andrew</td>
      <td>Stepp</td>
      <td>student</td>
      <td>7866419252</td>
      <td>2981 Lamberts Branch Road</td>
      <td>Hialeah</td>
      <td>Fl</td>
      <td>33012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Mary</td>
      <td>Raines</td>
      <td>student</td>
      <td>9075772295</td>
      <td>3975 Jerry Toth Drive</td>
      <td>Ninilchik</td>
      <td>AK</td>
      <td>99639</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Ed</td>
      <td>Lyman</td>
      <td>student</td>
      <td>5179695576</td>
      <td>3478 Be Sreet</td>
      <td>Lansing</td>
      <td>MI</td>
      <td>48933</td>
    </tr>
  </tbody>
</table>
</div>




```python
contacts[1].keys()

```




    dict_keys(['firstName', 'lastName', 'role', 'telephone', 'street', 'city', 'state', 'zipcode'])



## Commit Your Changes to the Database

Persist your changes by committing them to the database.


```python
con.commit()
```

## Create a Table for Student Grades

Create a new table in the database called "grades". In the table, include the following fields: userId, courseId, grade.

** This problem is a bit more tricky and will require a dual key. (A nuance you have yet to see.)
Here"s how to do that:

```SQL
CREATE TABLE table_name(
   column_1 INTEGER NOT NULL,
   column_2 INTEGER NOT NULL,
   ...
   PRIMARY KEY(column_1,column_2,...)
);
```


```python
cur.execute(""" 
            CREATE TABLE grades
            (
                gradeid INTEGER,
                userid  INTEGER,
                courseid,INTEGER,
                grade
            )
            
            
            """)
```




    <sqlite3.Cursor at 0x723d091ca840>



## Remove Duplicate Entries

An analyst just realized that there is a duplicate entry in the contactInfo table! Find and remove it.


```python
get_duplicate = """
                SELECT *
                FROM contactInfo
                WHERE (firstName, lastName, role, telephone, street, city, state, zipcode ) IN 
                      ( SELECT firstName, lastName, role, telephone, street, city, state, zipcode
                        FROM contactInfo
                        GROUP BY firstName, lastName, role, telephone, street, city, state, zipcode
                         HAVING COUNT(*) >1)
                """
                

```

## using python


```python
get_duplicate1 = """ 
                  SELECT *
                  FROM contactInfo

                 """
remove_duplicate = pd.read_sql_query(get_duplicate1,con)     
remove_duplicate.duplicated()    
```




    0    False
    1    False
    2    False
    3    False
    4    False
    5    False
    6    False
    7    False
    dtype: bool




```python

```


```python

remove_duplicate.drop_duplicates(inplace=True)  
```


```python
# Check that the duplicate entry was removed

```

## Updating an Address

Ed Lyman just moved to `2910 Simpson Avenue York, PA 17403`. Update his address accordingly.


```python
# Update Ed"s address
cur.execute ( """ 
             UPDATE contactInfo 
             SET  street = "2910 Simpson Avenue York", zipcode = "PA 17403"
             WHERE firstName = "Ed" and lastName = "Lyman"

             """)
```




    <sqlite3.Cursor at 0x723d091ca840>




```python
queryo = """ 
         SELECT *
         FROM contactInfo 
        """
pd.read_sql_query(queryo,con)            
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Contact_Id</th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Christine</td>
      <td>Holden</td>
      <td>staff</td>
      <td>2035687697</td>
      <td>1672 Whitman Court</td>
      <td>Stamford</td>
      <td>CT</td>
      <td>06995</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Christopher</td>
      <td>Warren</td>
      <td>student</td>
      <td>2175150957</td>
      <td>1935 University Hill Road</td>
      <td>Champaign</td>
      <td>IL</td>
      <td>61938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Linda</td>
      <td>Jacobson</td>
      <td>staff</td>
      <td>4049446441</td>
      <td>479 Musgrave Street</td>
      <td>Atlanta</td>
      <td>GA</td>
      <td>30303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Andrew</td>
      <td>Stepp</td>
      <td>student</td>
      <td>7866419252</td>
      <td>2981 Lamberts Branch Road</td>
      <td>Hialeah</td>
      <td>Fl</td>
      <td>33012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Mary</td>
      <td>Raines</td>
      <td>student</td>
      <td>9075772295</td>
      <td>3975 Jerry Toth Drive</td>
      <td>Ninilchik</td>
      <td>AK</td>
      <td>99639</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Ed</td>
      <td>Lyman</td>
      <td>student</td>
      <td>5179695576</td>
      <td>2910 Simpson Avenue York</td>
      <td>Lansing</td>
      <td>MI</td>
      <td>PA 17403</td>
    </tr>
  </tbody>
</table>
</div>



## Commit Your Changes to the Database

Once again, persist your changes by committing them to the database.


```python
con.commit()
```

## Summary

While there"s certainly more to do with setting up and managing this database, you got a taste for creating, populating, and maintaining databases! Feel free to continue fleshing out this exercise for more practice. 

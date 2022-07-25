# Creating Tables with SQL and Importing Data
A demonstration of how to create tables in PostgreSQL and import data for analysis.

## Overview
In this project, I will be using PostgreSQL and PgAdmin and I will demonstrate how to create tables from a dataset, review common data types and constraints, and how to import data via two different methods (GUI vs SQL Commands).

<strong>Language and Utilities Used:</strong>
- Language: `SQL`
- DBMS: [PostgreSQL 14](https://www.postgresql.org/)
- GUI: [PgAdmin 4](https://www.pgadmin.org/) on Windows 10

:round_pushpin: <b>Table of Contents</b> :round_pushpin:

- [Create Database (PgAdmin)](https://github.com/delaney-data/SQL-CreateTablesImport#creating-a-database-in-postgresql-with-pgadmin)
- [Syntax for Creating Tables](https://github.com/delaney-data/SQL-CreateTablesImport#creating-tables-in-postgresql)
	- [Data Types & Constraints](https://github.com/delaney-data/SQL-CreateTablesImport#creating-tables-data-types--constraints)
- [From CSV to SQL Table](https://github.com/delaney-data/SQL-CreateTablesImport#csv-dataset-%EF%B8%8F-sql-table) 
- [Importing Data into Tables (2 Methods)](https://github.com/delaney-data/SQL-CreateTablesImport#import-data-into-tables-2-methods)
	- [COPY Command: How to Fix Permission Errors](https://github.com/delaney-data/SQL-CreateTablesImport#how-to-fix-copy-errors)





# Creating a Database in PostgreSQL with PgAdmin
<p style="margin-top:0in;margin-right:0in;margin-bottom:8.0pt;margin-left:0in;line-height:107%;font-size:15px;font-family:&quot;Calibri&quot;,sans-serif;"></p>
<ol style="margin-bottom:0in;list-style-type: decimal;margin-left:0.5in;">
    <li style="margin: 0in 0in 8pt; line-height: 107%; font-size: 16px; font-family: Arial, Helvetica, sans-serif; text-align: left;"><span style="color: rgb(68, 114, 196);">In PgAdmin, create your database first by right-clicking <em>Databases</em> and create a new database.</span></li>
    <br><img src="https://i.imgur.com/5ctigjL.png" height="60%" width="50%" alt="SqlTut"/></br>
    <li style="margin: 0in 0in 8pt; line-height: 107%; font-size: 16px; font-family: Arial, Helvetica, sans-serif; text-align: left;"><span style="color: rgb(68, 114, 196);">Name your DB and click save. If you don&rsquo;t immediately see your DB, right-click again on <em>Databases</em> and click <em>refresh</em>.</span></li>
    <li style="margin: 0in 0in 8pt; line-height: 107%; font-size: 16px; font-family: Arial, Helvetica, sans-serif; text-align: left;"><span style="color: rgb(68, 114, 196);">Query your DB by right-clicking its name and selecting the <strong>Query Tool</strong>.&nbsp;This will bring up the Query Panel and then you can begin interacting with it via SQL.&nbsp;</span></li><br><img src="https://i.imgur.com/e1eVuUq.png" height =30% width=30% alt="SqlTut"/></br>
</ol>


Note: If you are working with multiple databases at once, you can keep track of which one you are currently via the name in the Query Panel. The panel name defines which DB you are in : `YourDatabaseName / PostgreSQL Version #`


# Creating tables in PostgreSQL
As a best practice, before creating tables that are based on a dataset in any SQL database:

<strong>🔎 Review your headers and data types in your dataset</strong>🔎
- The column headers from your <b>dataset</b> must match your <b>SQL table headers</b> and your data types must also agree in order to import into the table. 
	- Data type issue - A column data type as a `VARCHAR` when the corresponding <i>source</i> values is an `INTEGER`.
	- Null issue - Be familiar with your data set. You might discover there are `null` strings represented by 'NA' in your column that is assigned an Integer type 		(which cannot read strings of text). 
		I demonstrate how to handle nulls in the import section.

<br>For this project, I will use a dataset from Kaggle.com (USA People Without Internet in 2016). 

From the CSV file, I will create two tables in PostgreSQL:
1. A county population table (county_pop) which will have the population and racial data with the percentage of each county with no internet access.
2. An education level and income level table (education_income) that has the education, median age, and income to compare to the population table.

This is the general syntax for creating a table:

```sql
CREATE TABLE table_name (
                column_name TYPE column_constraint,

                column_name TYPE column_constraint,

                column_name TYPE column_constraint
                );

--- Don't forget the commas between your columns, minus the last column
```

## Creating Tables: Data Types & Constraints
- A <b>DATA TYPE</b> specifies the pattern (Text, Number...) of the data and how the value is stored. The values must adhere to the requirements of the type for PostgreSQL to accept.
    - Common data types:
        - True or False `boolean`
        - Character  `char`, `varchar`, and `text`
        - Numeric `integer` and `float`
        - Temporal `date`, `time`, `timestamp`


        PostgreSQL has many data types, for more details on each type (and storage limitations) refer to this handy documentation:                          https://www.postgresql.org/docs/current/datatype.html

- A column <b>CONSTRAINT</b> is an additional requirement or condition for the values in that column. 
    - Common column constaints:
        - `NOT NULL` Ensures there is never <i>null</i> data (or an absence of data). Very useful when importing new data to table. For example, if you are importing new  data into a customer sales table and company policy is to always require an email address you would set the email_address column constraint as `NOT NULL`.  
         
        - `UNIQUE` Ensures that each value in the column is unique (not repeated). For example, you may want to ensure each row of a phone number column is unique per person.
        - `PRIMARY KEY` An assigned number, used to uniquely identify each row in a table. This can allow you to target, retrieve or modify a row based on the specific PK (primary key).
	
		- `SERIAL` A way of automatically creating new PK integers (adds +1) as new row data is entered into a table.
	
		For more details on constraints: https://www.postgresql.org/docs/14/ddl-constraints.html

## CSV Dataset ➡️ SQL Table:
Upon reviewing the headers and data types my CSV dataset below, I can build my SQL query to create the table structure.
<br><img src="https://i.imgur.com/bRSOTyf.png?1" height="80%" width="70%" alt="SqlTut"/></br>     
```sql

--- First table (county_pop)
--- Contains counties, states, total popoulation numbers (indicated with 'P_'), race numbers, and a percentage of each without having Internet.

CREATE TABLE county_pop(
	id_pop SERIAL PRIMARY KEY,
	county VARCHAR(55) NOT NULL,
	state VARCHAR(2) NOT NULL,
	P_total INT,
	P_white INT,
	P_black INT,
	P_asian INT,	
	P_native INT,
	P_hawaiian INT,
	P_others INT,
	percent_no_internet decimal
	);
```

```sql
--- Second table (education_income)
--- Contains population numbers for education levels, poverty levels, and median age and median income per counties and states.

CREATE TABLE education_income(
	id SERIAL PRIMARY KEY,
	county VARCHAR(55) NOT NULL,
	state VARCHAR(2) NOT NULL,
	P_below_middle_school INT,
	P_some_high_school INT,
	P_high_school_equivalent INT,
	P_some_college INT,	
	P_bachelor_and_above INT,
	P_below_poverty INT,
	median_age decimal,
	median_household_income decimal,
	median_rent_per_income decimal
	);
```

Tip: If you make a mistake when assigning a data type or constraint, use the `ALTER` statement.


```sql
--- Changing data type for a column

ALTER TABLE table_name
	ALTER COLUMN column_name 
	TYPE your_new_data_type;
```

```sql
--- Removing a constraint
--- To add, replace drop with ADD

ALTER TABLE table_name
	ALTER COLUMN column_name 
	DROP constraint_name;
```



# Import Data into Tables (2 methods)
Now that we have tables in the database, we need to insert values into those tables. Here are two ways to accomplish this:

<br><strong>1. Using PgAdmin (GUI method)</strong></br>
- This is the easiest. Does not require special file permissions
- Right-click on your database 
➡️ go to <strong>Schemas</strong>
➡️ <strong>Tables</strong>
➡️ Right-click on your table and select <strong>Import/Export Data</strong>
<br>
	On the options side, locate the file path of your CSV dataset and toggle headers ON:</br>
<br><img src="https://i.imgur.com/S8jT1Yw.png?1" height="80%" width="50%" alt="SqlTut"/>
<br>
 On the columns side, select all columns in the file to import (minus the ID primary key column we created for the table). 
 <br>Note: If your dataset includes null strings (as in, nulls are represented by NA or some other string in your data), you need to specify what PostgreSQL should do when it encounters ‘NA’ in the data. For my example, I have the NULL Strings as ‘NA’. <br>
       <br><img src="https://i.imgur.com/bPTFrzW.png" height="80%" width="50%" alt="SqlTut"/>
    <br></li>
<p><strong>2. Using the SQL COPY command</strong></p>
<ul>
    <li>Requires special permissions for PostgreSQL to read/write files to the local PC (if that&rsquo;s where you&rsquo;re pulling data).</li>
</ul>
<p><u>General COPY command syntax:</u></p>


```sql
COPY table_name (column_name, column_name, etc ...)
FROM ‘C:\Users\Name\Location.csv’
DELIMITER ‘,’ --- Since we are using a CSV (comma seperated values) file formant, the DELIMITER is ','
NULL ‘NA’ --- We are declaring that any null data is represented by the string 'NA'
CSV HEADER; --- We want to leave the headers from our file

```



### How to Fix COPY Errors:
If you get a permission error from PostgreSQL, similar to something like 
<br>`  ERROR: could not open files "YourFile.csv" for reading: Permission Denied  `
<br>
<br>There are a couple of ways to approach this, but for my purposes I changed the permission settings for my specific file. 
- Go to the file/folder location you are using for your dataset
- Right click the file/folder
- Give Access to ➡️ Specific People
- Give Read/Write permissions to <i>Everyone</I>
	- Giving the <i>Write</i> permission will allow you to <i>export</i> from PostgreSQL later for sharing or for data visualizations.

For both of these import methods, the <i>messages</i> box in your Query Panel will return something like:
<br>
`COPY 820
Query returned succesfullly in 46 msec.`
	The 820 integer for this example is the total count of number of rows successfully imported. 

# Conclusion
In summary, we have: 
- Created a simple database using PgAdmin
- Created Tables
	- Basic Table Syntax
	- Covered Common Data Types
	- Covered Common Constraints
	- Used a Dataset (CVS file) to Create a Table in the Database
- Imported Values
	- Graphic Interface Method
	- COPY Command Method

Now we can begin analyzing the data with SQL commands to answer questions about our dataset. 

This concludes this chapter of the project and we will move to analyzing in the next section (I will update with a link once the write-up is complete). Thanks for reading! 👋

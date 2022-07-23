# Creating Tables with SQL and Importing Data
A demonstration of how to create tables in PostgreSQL and import data for analysis.

# Overview
<p style='margin-top:0in;margin-right:0in;margin-bottom:8.0pt;margin-left:0in;line-height:107%;font-size:15px;font-family:"Calibri",sans-serif;'>Before doing an analysis of our data with SQL, we first need to create the database and tables, then import data into our Database Management System (DBMS). In this project, we will be using PostgreSQL and PgAdmin and I will demonstrate how to create your tables and import your data in two different methods (one with SQL commands and one with the GUI).</p>
<p style='margin-top:0in;margin-right:0in;margin-bottom:8.0pt;margin-left:0in;line-height:107%;font-size:15px;font-family:"Calibri",sans-serif;'><strong>Language and Utilities Used:</strong></p>
<ul style="list-style-type: disc;">
    <li><u>Language</u>: SQL</li>
    <li><u>DBMS</u>: PostgreSQL 14</li>
    <li><u>GUI</u>: PgAdmin 4 (on Windows 10)</li>
</ul>
<p style='margin-top:0in;margin-right:0in;margin-bottom:8.0pt;margin-left:0in;line-height:107%;font-size:15px;font-family:"Calibri",sans-serif;'><strong>&nbsp;</strong></p>

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
As a best practice, before creating tables in any SQL database
- <strong>🔎 Review your headers and data types in your dataset</strong>🔎, as this will guide you in creating your table in the database.
- The column headers must match the one we will create with SQL and your data types must also be correct in order to import into the table. 
     - As in, you should not set a column data type as a `VARCHAR` when the corresponding source dataset column is an `INTEGER`.

For this project, I will use a dataset from Kaggle.com (USA People Without Internet in 2016). From the CSV file, I will create two tables in PostgreSQL:
- A county population table (county_pop) which will have the <strong>race and population data</strong> with the <strong>percent no internet</strong>.
- An education level and income level table (education_income) that has the&nbsp;<strong>education, median age, and income</strong> to compare to the population table.

This is the general syntax for creating a table:

```sql
CREATE TABLE table_name (
                column_name TYPE column_constraint,

                column_name TYPE column_constraint,

                column_name TYPE column_constraint
                );

--- Don't forget the commas between your columns
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
        - `NOT NULL` Ensures there is never <i>null</i> data (or an absenece of data). Very useful when importing new data to table. For example, if you are importing new  data into a customer sales table and company policy is to always require an email address you would set the email_address column constraint as `NOT NULL`.  
         
        - `UNIQUE` Ensures that each value in the column is unique (not repeated). For example, you may want to ensure each row of a phone number column is unique per person.
        - `PRIMARY KEY` An assigned number, used to uniquely identify each row in a table. This can allow you to target, retrieve or modify a row based on the specific PK (primary key).
  
### From Dataset ➡️ Table:
Upon reviewing the headers and data types my CSV dataset below, I can build my SQL query to create the table structure.
<br><img src="https://i.imgur.com/bRSOTyf.png?1" height="80%" width="70%" alt="SqlTut"/></br>
<br></br>       
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


To learn more about data types and constraints when building your tables, refer to the documention here: https://www.postgresql.org/docs/current/

# Import Data into Tables (2 methods)
Now that we have tables in the database, we need to insert data into those tables. Here are two different ways to accomplish this:
<br></br>
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
DELIMITER ‘,’ 
NULL ‘NA’ 
CSV HEADER; 
```

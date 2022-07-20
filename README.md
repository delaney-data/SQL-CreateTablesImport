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
    <br><img src="https://i.imgur.com/5ctigjL.png" height="80%" width="50%" alt="SqlTut"/></br>
    <li style="margin: 0in 0in 8pt; line-height: 107%; font-size: 16px; font-family: Arial, Helvetica, sans-serif; text-align: left;"><span style="color: rgb(68, 114, 196);">Name your DB and click save. If you don&rsquo;t immediately see your DB, right-click again on <em>Databases</em> and click <em>refresh</em>.</span></li>
    <li style="margin: 0in 0in 8pt; line-height: 107%; font-size: 16px; font-family: Arial, Helvetica, sans-serif; text-align: left;"><span style="color: rgb(68, 114, 196);">Query your DB by right-clicking its name and selecting the <strong>Query Tool</strong>.&nbsp;This will bring up the Query Panel and then you can begin interacting with it via SQL.&nbsp;</span></li><br><img src="https://i.imgur.com/e1eVuUq.png" height =40% width=40% alt="SqlTut"/></br>
</ol>
<p><span style="color: rgb(68, 114, 196); font-size: 16px; font-family: Arial, Helvetica, sans-serif;">Note: If you are working with multiple databases at once, be sure you are keeping track of which one you are currently querying. You can always see which one you currently querying via the top panel. The panel name should be: <strong>YourDatabaseName</strong>/PostgreSQL</span></p>

# Creating tables in PostgreSQL
<p><span style="color: rgb(0, 0, 0);">As a best practice, before creating tables in any SQL database:&nbsp;</span></p>
<ul>
    <li style="color: rgb(0, 0, 0);"><strong>🔎 Review your headers and&nbsp;</strong><strong>data types in your dataset</strong>🔎, as this will guide you in creating your table in the database.</li>
    <li style="color: rgb(0, 0, 0);">The column headers must match the one we will create with SQL and your datatypes must also be correct in order to import data. As in, you should not create a column as a VARCHAR datatype when the corresponding source dataset column is an INTEGER.&nbsp;</li>
</ul>
<p style='margin-top:0in;margin-right:0in;margin-bottom:0in;margin-left:0in;line-height:107%;font-size:15px;font-family:"Calibri",sans-serif;'><span style="color: rgb(0, 0, 0);">For this project, I will use a dataset from Kaggle.com (USA People Without Internet in 2016). I will create two tables:</span></p>
<ul style="list-style-type: disc;">
    <li style="color: rgb(0, 0, 0);">A county population table (county_pop) which will have the <strong>race and population data</strong> with the <strong>percent no internet</strong>.</li>
    <li><span style="color: rgb(0, 0, 0);">A demographic table (education_income) that has the&nbsp;<strong>education, median age, and income</strong> to compare to the population table</span><span style="color:#4472C4;">.</span></li>
</ul>
<figcaption style="margin: 0in; line-height: 12.05pt; font-family: Calibri, sans-serif; text-align: center;"><span style="font-size:15px;"><u>This is the general<em>&nbsp;syntax</em> for creating a table.&nbsp;</u></span><u>Make sure to put a comma in between your columns:</u></figcaption>
<p style='margin-top:0in;margin-right:0in;margin-bottom:0in;margin-left:1.0in;line-height:12.05pt;font-size:15px;font-family:"Calibri",sans-serif;'><span style="font-size:15px;">&nbsp;</span></p>
<blockquote>
    <p style="margin: 0in 0in 0in 176px; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif;"><span style="font-size:15px;">CREATE TABLE table_name<em>&nbsp;(</em></span></p>
    <p style="margin: 0in 0in 0in 156px; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif;"><em><span style="font-size:15px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></em><span style="font-size:15px;">column_name TYPE column_constraint<em>,</em></span></p>
    <p style="margin: 0in 0in 0in 156px; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif;"><span style="font-size:15px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;column_name TYPE column_constraint<em>,</em></span></p>
    <p style="margin: 0in 0in 0in 156px; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif;"><span style="font-size:15px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; column_name<em>&nbsp;</em>TYPE column_constraint</span></p>
    <p style="margin: 0in 0in 0in 156px; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif; text-indent: 0.5in;"><span style="font-size:15px;">);</span></p>
</blockquote>
<p style="margin: 0in; line-height: 12.05pt; font-size: 15px; font-family: Calibri, sans-serif; text-align: center;"><span style="font-size:15px;"><u>Upon reviewing the dataset below, I can build my SQL query to create the table format.</u>&nbsp;</span></p>
<br><img src="https://i.imgur.com/bRSOTyf.png" height="80%" width="70%" alt="SqlTut"/></br>

<blockquote>First table (county_pop)

<br><img src="https://i.imgur.com/mCjv9ct.png" height="80%" width="50%" alt="SqlTut"/></br></blockquote>

<blockquote>Second table (education_income)

<br><img src="https://i.imgur.com/Y7TAKn3.png?2" height="80%" width="50%" alt="SqlTut"/></br></blockquote>
<hr>
As we can see, PostgreSQL has color coding for the column names, data types, and constraints. To learn more about data types and constraints when building your tables, refer to the documention here: https://www.postgresql.org/docs/current/

# Import Data into Tables (2 methods)
<p>Now that you have the tables in your database, you need to insert data into those tables. Here are two different ways you can do this:</p>
<p><strong>1. Using PgAdmin (GUI method)</strong></p>
<ul>
    <li>This is the easiest. Does not require special file permissions.&nbsp;</li>
    <li>Right-click on your database ➡️ go to <strong>Schemas</strong> ➡️<strong>Tables</strong> ➡️ Right-click on the table and select <strong>Import/Export Data</strong></li>
</ul>
<br>On the options side, locate the file path of your CSV dataset and toggle headers ON:<br>
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
<blockquote>COPY table_name (column_name, column_name) <br>FROM &lsquo;C:\Users\Name\Location.csv&rsquo; 
    <br>DELIMITER &lsquo;,&rsquo; NULL &lsquo;NA&rsquo; CSV HEADER;&nbsp;</blockquote></li>
        </ol>
    </li>
</ol>


# More Practice With SQL Queries - Lab

## Introduction

In this lesson, we'll run through some practice questions to refresh your knowledge of SQL Queries!

## Objectives

You will be able to:

- Use `GROUP BY` statements in SQL to apply aggregate functions like: `COUNT`, `MAX`, `MIN`, and `SUM`
- Decide and perform whichever type of join is best for retrieving desired data
- Use the `HAVING` clause to compare different aggregates
- Write subqueries to decompose complex queries

## Getting Started

As in previous labs, we'll make use of the `sqlite3` library as well as Pandas. By combining them, we'll be able to write queries as Python strings, and make sure that the results are always returned as a Pandas DataFrame. 

We'll start by loading both libraries and connecting to the database we'll be using for this lab, `data.sqlite`. You may remember this database from a previous lab. As a refresher, here's the ERD diagram for this database: 

<img src='images/Database-Schema.png'>

In the cell below:

* Import the necessary libraries `pandas` and `sqlite3` 
* Establish a connection to the database `data.sqlite` 
* Get the `cursor` from the connection and store it in the variable `c` 


```python
import pandas as pd
import sqlite3
conn = sqlite3.connect('data.sqlite')
c = conn.cursor()
```

## Basic Queries

Now, let's review basic SQL queries. In the cell below:

Write a query that gets the first name, last name, phone number, address, and credit limit for all customers in California with a credit limit greater than 25000.00. 


```python
# For the first query, the boilerplate for getting 
# the query into a DataFrame has been provided for you
c.execute("""SELECT contactFirstName, contactLastName, phone, addressLine1, state, creditLimit
            FROM customers
            WHERE state = "CA"
            GROUP BY 1
            HAVING creditLimit > 25000;
            """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
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
      <th>contactFirstName</th>
      <th>contactLastName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>state</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Brian</td>
      <td>Chandler</td>
      <td>2155554369</td>
      <td>6047 Douglas Av.</td>
      <td>CA</td>
      <td>57700.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Julie</td>
      <td>Murphy</td>
      <td>6505555787</td>
      <td>5557 North Pendale Street</td>
      <td>CA</td>
      <td>64600.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Juri</td>
      <td>Hashimoto</td>
      <td>6505556809</td>
      <td>9408 Furth Circle</td>
      <td>CA</td>
      <td>84600.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Steve</td>
      <td>Thompson</td>
      <td>3105553722</td>
      <td>3675 Furth Circle</td>
      <td>CA</td>
      <td>55400.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Sue</td>
      <td>Frick</td>
      <td>4085553659</td>
      <td>3086 Ingle Ln.</td>
      <td>CA</td>
      <td>77600.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Susan</td>
      <td>Nelson</td>
      <td>4155551450</td>
      <td>5677 Strong St.</td>
      <td>CA</td>
      <td>210500.0</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Valarie</td>
      <td>Thompson</td>
      <td>7605558146</td>
      <td>361 Furth Circle</td>
      <td>CA</td>
      <td>105000.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-1.png'>

## Aggregate Functions and GROUP BY

Next, write a query that gets the average credit limit per state.


```python
c.execute("""SELECT state, creditLimit
            FROM customers
            GROUP BY 1
            HAVING AVG(creditLimit);
            """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
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
      <th>state</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td></td>
      <td>21000.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>BC</td>
      <td>90300.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>CA</td>
      <td>210500.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>CT</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Isle of Wight</td>
      <td>93900.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>MA</td>
      <td>43400.0</td>
    </tr>
    <tr>
      <td>6</td>
      <td>NH</td>
      <td>114200.0</td>
    </tr>
    <tr>
      <td>7</td>
      <td>NJ</td>
      <td>43000.0</td>
    </tr>
    <tr>
      <td>8</td>
      <td>NSW</td>
      <td>107800.0</td>
    </tr>
    <tr>
      <td>9</td>
      <td>NV</td>
      <td>71800.0</td>
    </tr>
    <tr>
      <td>10</td>
      <td>NY</td>
      <td>114900.0</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Osaka</td>
      <td>81200.0</td>
    </tr>
    <tr>
      <td>12</td>
      <td>PA</td>
      <td>100600.0</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Queensland</td>
      <td>51600.0</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Québec</td>
      <td>48700.0</td>
    </tr>
    <tr>
      <td>15</td>
      <td>Tokyo</td>
      <td>94400.0</td>
    </tr>
    <tr>
      <td>16</td>
      <td>Victoria</td>
      <td>117300.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-2.png'>

## JOINs

Now, write a query that uses JOIN statements to get the customer name, customer number, order number, status, and quantity ordered. Print only the head of this DataFrame. 


```python
c.execute("""SELECT customerName, customerNumber, orderNumber, status, quantityOrdered 
            FROM customers
            JOIN orders
            USING(customerNumber)
            JOIN orderDetails
            USING(orderNumber);
            """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head()
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
      <th>customerName</th>
      <th>customerNumber</th>
      <th>orderNumber</th>
      <th>status</th>
      <th>quantityOrdered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>22</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>30</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>49</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>50</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Blauer See Auto, Co.</td>
      <td>128</td>
      <td>10101</td>
      <td>Shipped</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/joins.png'>

## HAVING and ORDER BY

Now, return the customerName, customerNumber, productName, productCode, and total number ordered for any product a customer has bought 10 or more of cumulatively. Sort the rows in descending order by the quantity ordered. 

**_Hint_**: For this one, you'll need to make use of HAVING, GROUP BY, and ORDER BY -- make sure you get the order of them correct!


```python
c.execute("""SELECT customerName, customerNumber, productName, productCode, SUM(quantityOrdered) AS total
            FROM customers
            JOIN orders
            USING(customerNumber)
            JOIN orderDetails
            USING(orderNumber)
            JOIN products
            USING(productCode)
            GROUP BY productCode, customerName
            HAVING total >10
            ORDER BY total DESC;
            """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head()
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
      <th>customerName</th>
      <th>customerNumber</th>
      <th>productName</th>
      <th>productCode</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1992 Ferrari 360 Spider red</td>
      <td>S18_3232</td>
      <td>308</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1958 Chevy Corvette Limited Edition</td>
      <td>S24_2840</td>
      <td>245</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1970 Dodge Coronet</td>
      <td>S24_1444</td>
      <td>197</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1957 Chevy Pickup</td>
      <td>S12_4473</td>
      <td>183</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>2002 Chevy Corvette</td>
      <td>S24_3432</td>
      <td>174</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/having_order.png'>

## Subqueries

Finally, get the first name, last name, employee number, and office code for employees from offices with less than 5 employees. Print the first five rows of this DataFrame. 


```python
c.execute("""SELECT firstName, lastName, employeeNumber, officeCode
            FROM offices
            JOIN employees
            USING(officeCode)
            WHERE officeCode in (SELECT officeCode
                                        FROM employees
                                        GROUP BY officeCode
                                        HAVING COUNT(DISTINCT officeCode < 5));
                                        """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
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
      <th>firstName</th>
      <th>lastName</th>
      <th>employeeNumber</th>
      <th>officeCode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Diane</td>
      <td>Murphy</td>
      <td>1002</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Mary</td>
      <td>Patterson</td>
      <td>1056</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Jeff</td>
      <td>Firrelli</td>
      <td>1076</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Anthony</td>
      <td>Bow</td>
      <td>1143</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1165</td>
      <td>1</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>1166</td>
      <td>1</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>1188</td>
      <td>2</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Steve</td>
      <td>Patterson</td>
      <td>1216</td>
      <td>2</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>1286</td>
      <td>3</td>
    </tr>
    <tr>
      <td>9</td>
      <td>George</td>
      <td>Vanauf</td>
      <td>1323</td>
      <td>3</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Gerard</td>
      <td>Bondur</td>
      <td>1102</td>
      <td>4</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Loui</td>
      <td>Bondur</td>
      <td>1337</td>
      <td>4</td>
    </tr>
    <tr>
      <td>12</td>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>1370</td>
      <td>4</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>1401</td>
      <td>4</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1702</td>
      <td>4</td>
    </tr>
    <tr>
      <td>15</td>
      <td>Mami</td>
      <td>Nishi</td>
      <td>1621</td>
      <td>5</td>
    </tr>
    <tr>
      <td>16</td>
      <td>Yoshimi</td>
      <td>Kato</td>
      <td>1625</td>
      <td>5</td>
    </tr>
    <tr>
      <td>17</td>
      <td>William</td>
      <td>Patterson</td>
      <td>1088</td>
      <td>6</td>
    </tr>
    <tr>
      <td>18</td>
      <td>Andy</td>
      <td>Fixter</td>
      <td>1611</td>
      <td>6</td>
    </tr>
    <tr>
      <td>19</td>
      <td>Peter</td>
      <td>Marsh</td>
      <td>1612</td>
      <td>6</td>
    </tr>
    <tr>
      <td>20</td>
      <td>Tom</td>
      <td>King</td>
      <td>1619</td>
      <td>6</td>
    </tr>
    <tr>
      <td>21</td>
      <td>Larry</td>
      <td>Bott</td>
      <td>1501</td>
      <td>7</td>
    </tr>
    <tr>
      <td>22</td>
      <td>Barry</td>
      <td>Jones</td>
      <td>1504</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-5.png'>

# Summary

In this lesson, we reviewed all the major concepts and keywords associated with SQL queries!

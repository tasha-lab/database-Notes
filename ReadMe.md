# Normalization

Normalization is a technique that reduces data redundancy, dependency and anomalies while performing insertions, updates and deletion.  
It improves a database design by generating relations of higher normal forms.

## Problems caused by Redundancy

### Insertion Anomalies

It might be impossible to store information if some inormation is not stored. eg:  
If we are adding a new employee who isnt assigned to any department yet, it would be hard to enter their data if the emp_dept area doesnt allow null.

### Update anomalies

Update anomalies is basically caused by inconsistency. If we have any redundant data, if we update one and not every other field with the same data, it causes an update anomaly

### Deletion Anomaly

Deletion anomaly happends when we delete data and for that reason we lose other useful data.

### Redundant storage

Basically happens when some information is stored repeatedly.

## Dependencies

**Functional Dependency\_** - It generalizes the concept of a key. One column determines another eg.
<code>sql Employee_ID -> Employee_name<code>

**_ Transitive dependency _** - For instance if we have attributes A,B and C. And <code>A->B</code> and <code>B->C</code>. Functional dependencies are also transitive. Therefore <code>A->C</code>. We can say, C is transitively dependent on A through B.  
We can also say it is a dependency of one non-prime attribute (An attribute thats not part of the candidate key) to another non-prime attribute.

**_ Partial Dependency_** - Exists when an attribute is functionally dependent on another attribute and the other attribute is a component of multipart candidate key

## Normalization rules

These is a list of Normal Forms in SQL

### 1<sup>st</sup> Normal form

#### Rules

- Each cell must have only one value and not a set of values
- Each record is unique
- Each column is unique and no repeating columns  
   For example:
  | Employee | Age | Department |
  | -------- | --- | ------------------ |
  | Melvin | 32 | Marketing, Sales |
  | Edward | 45 | Quality Assuarance |

Our Table is not in any normal form yet. Melving, is in two departments. To sort this out we arrang the table as:

| Employee | Age | Department         |
| -------- | --- | ------------------ |
| Melvin   | 32  | Marketing          |
| Melvin   | 32  | Sales              |
| Edward   | 45  | Quality Assuarance |

### 2<sup>nd</sup> Normal form

#### Rules

- The table is in 1NF
- There is not partial dependency. ie. no non-prime attribute is dependent on any candidate key of the table.
  (prime attribute is the opposite of non-prime attribute)

For example:

| Employee_ID | Employee | Age | Department         | Dept_ID |
| ----------- | -------- | --- | ------------------ | ------- |
| 101         | Melvin   | 32  | Marketing          | 01      |
| 101         | Melvin   | 32  | Sales              | 02      |
| 103         | Edward   | 45  | Quality Assuarance | 03      |

The Employee_ID and Dept_ID are the prime attributes. Since employee name can be identified by Employee_ID and department by Dept_ID. Pertial dependency exists.

To fix this, we can split the table into:  
Table 1: Employee Table

| Employee_ID | Employee | Age | Dept_ID |
| ----------- | -------- | --- | ------- |
| 101         | Melvin   | 32  | 01      |
| 101         | Melvin   | 32  | 02      |
| 103         | Edward   | 45  | 03      |

Table 2: Department Table

| Dept_ID | Department         |
| ------- | ------------------ |
| 01      | Marketing          |
| 01      | Sales              |
| 03      | Quality Assuarance |

### 3<sup>rd</sup> Normal form

#### Rules

- Must be in 2<sup>nd</sup> Normal form
- There is no transitive functional dependency. Every non-prime attribute is dependent on primary key.

Using the table we turned to 2<sup>nd</sup> earlier:

| Employee_ID | Employee | Age | Dept_ID |
| ----------- | -------- | --- | ------- |
| 101         | Melvin   | 32  | 01      |
| 101         | Melvin   | 32  | 02      |
| 103         | Edward   | 45  | 03      |

Employee-> Employee_ID

We can split the table into an employee name table and have our initial employee table

| Employee_ID | Age | Dept_ID |
| ----------- | --- | ------- |
| 101         | 32  | 01      |
| 101         | 32  | 02      |
| 103         | 45  | 03      |

| Employee_ID | Employee |
| ----------- | -------- |
| 101         | Melvin   |
| 103         | Edward   |

# ACID properties

ACID is a set of principles that ensure reliable database transactions. Transactions are commands executed to access and modify information in databases.  
ACID stands for

- Atomicity
- Consistency
- Isolation
- Durability

## Atomicity

Atomicity means that either the entire transaction takes place or doesn't happen at all. Incase connection is lost, in the middle of operation, the database is rolled back to its prior state.

For example:  
Consider a transaction **T** consisting of two transactions **T1** and **T2**. We are transafering ksh 200 from account **X** to **Y** in both transactions. If the transaction fails after completion of **T1**, the amount has already been deducted from the account **X** which will cause inconsistency in data.  
Therefore, a single transaction must be executed completely or not at all to ensure correctness.

## Consistency

Consistency basically means maintaining data integrity constraints so that data is consistent before and after transaction.

Using the previous example:  
If in for example account **X** we only have ksh 50, and the rules say that no amount should be sent incase there are no enough funds, the transaction is rolled backed.

## Isolation

This means that multiple transactions can occur concurrently without causing any inconsistencies. Any operations on one set of data will not impact other operations of separate transactions. The operations will also not be visible to any other transaction.

Using the previous example:  
Incase we have two transactions **T** and **Z** these two transactions can happen at the same time with no problems. Any effects on **T** will not impact **Z**.

## Durability

This ensures that once the transaction has completed, changes made to the transaction that are successfully committed will be there permanently even in the instance of failure. eg. service outages, crashes etc

Using the previous example:  
Back to our transaction **T** example, if the transaction happened to completion, and we sent the money from **X** to **Y**, and the transaction was successful, the changes should stay even in any case of failure.

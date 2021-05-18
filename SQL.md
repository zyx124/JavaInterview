## Data Modeling

Process of analyzing business requirement, designing and creating a plan for DB.

A stage in SDLC. 

###### keys

- **Super** Key: the key that can identify the table.
- **Primary** key: one per table, usually ID column.
- **Unique** key:  minimal super key
- **candidate** key: a kind of unique key
- **foreign **key: keys to link between two tables

| Primary key         | candidate key                   |
| ------------------- | ------------------------------- |
| only one in a table | can have multiple               |
| cannot be NULL      | can be NULL                     |
| is a candidate key  | may or may not be a primary key |



###### Normalization

why: **redundancy** (values repeated unnecessarily in multiple recoreds) and **anomaly** (modify a relation, redundant data may arise problems in relations). Minimize redesign when extending and ensure the data dependencies are forced by **integrity** (**Entity** integrity, **referntial** integrity, **domain** integrity) constraints.

when: lots of read/write, not too much fetch, data integrity is important.

- 1NF: each table should contain a **atomic** value, each record needs to be unique.
- 2NF: Meet 1NF. All non-prime attributes are fully dependent on a prime attribute. **No partial dependencies**.
- 3NF: Meet 2NF and 1NF. Every non-prime attribute is **non-transitively** dependent on the prime attributes.
- BCNF: Boy and Codd normal form, for any dependency A->B, A should be a super key.

###### RDB vs NoSQL:

|             | SQL                                                   | NoSQL                                                        |
| ----------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| type        | table based                                           | document based (MongoDB), key-value, graph                   |
| scalability | vertically scalable                                   | horizontally scalable                                        |
| consistency | strong                                                | eventual consistency                                         |
|             | ACID (Atomicity, Colnsistency, Isolation, Durability) | Base (Basically available, Soft state, eventually consistent) |
| usage       | acid and complex queries                              | availability and scale                                       |

how to deal with many-to-many relationship? Use a **conjunction table**.

---

**aggregate function**: COUNT, SUM, MIN, MAX, AVG, GROUP BY, HAVING

**joins**: ![join](/home/zyx/repo/JavaInterview/join.png)



**self join**: can be used to find manager of employee table.

**sub query**: correlated & non-correlated depends on whether the sub query depends on the outer query. cannot use in **BETWEEN** clause. 

**set operators**: UNION, UNION ALL, INTERSECT, MINUS

**UNION vs JOIN**: union creates new rows while join creates new columns.

**view**: virtual table, updated every time it's referred to, 

for security since you can choose data to show

moidified data will also be updated in the source data.

**variables**: Scope: Local, Batch-bound (only visible within the batch)

user defined variables are with @, system defined variables are with @@

**temporary table**: global (can be used across sessions) local (current session)

stored in tempDB and cannot be partitioned

**Stored procedure**: can have different input and output parameters 

**index**: sort and optimize the data, when created, an index will create a dynamic Balance Tree (B+ tree). Indexes can use max of 16 columns or 900B of data.

| clustered index                                              | non-clustered index                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| store data in the Leaf Pages sort them based on the Key values of the column you choose | NOT store data in the leaf pages, point to the rows they are referencing |
| Only one in each table                                       | can be many based on the server version                      |

How to deal with large table: 

- avoid scan
- index

## JDBC (Java Database Connectivity)

It's an API which consists of java classes, interfaces and exceptions to deal with the relational databases.

![](/home/zyx/repo/JavaInterview/The-JDBC-architecture.jpg)

Drivers:

![](/home/zyx/repo/JavaInterview/jdbc_drivers_graphic.png)

###### **JDBC API components:**

- DriverManager
- Driver
- Connection
- Statement
- ResultSet



###### **How JDBC works:**

- Load the driver (`Class.forName(driverName)`)

  `Conection con = DriverManager.getConnection(dbURL)`

  If there's exception, `SQLException` is generated. 

- create statements

  - `Statement` is used for general purpose access to the database, useful when using static sql at runtime. Does not accept parameters. can call `addBatch()` for batch processing.

  - `PreparedStatement` is to increase efficiency to execute sql statements many times, can accept parameters. use `PreparedStatement` instead of `statement` to prevent **SQL injection**.

  `PreparedStatement ps = con.prepareStatement(String query);`

  - A CallableStatement object is used to call **stored procedure**s.

    `CallableStatement cs = con.prepareCall(String query);`

- execute queries and get result

  `execute()` is for any kind of sql statements, returns boolean value.

  `executeQuery()` is for executing a query that returns a single ResultSet.

  `executeUpdate`() is used for DDL and DML statements like insert, update, delete and create. returns an int value to represent affected rows.

- iterate through the results

  scrollable result set allows retrieve data by moving forward or backward. use `createStatement()` to make it scrollable in connection interface.

  `public Statement createStatement(int Type, int Mode);`

  • TYPE_FORWARD_ONLY -> 1

  • TYPE_SCROLL_INSENSITIVE -> 2

  • CONCUR_READ_ONLY -> 3

- close connection\

  `close()`

**MetaData**: the data that provides a structured description about some other data, like vies, column types, column names, result sets, stored procedures etc.

**Transaction Management**: 

A transaction represents a single unit of work. 

Transaction control is performed by the `Connection` object, default mode is auto-commit, i.e, each sql statement as a transaction. `con.setAutoCommit(boolean)`;

`con.commit()` and `con.rollback()`

**ACID transaction**

- Atomicity: single operation
- Consistency: data consistent when transaction starts and ends
- Isolation: invisible to other transactions
- Durability: once a transaction completes, changes to data are 

| **Isolation Level** | Dirty Read | Non Repeatable Read | Phantom Read |
| ------------------- | ---------- | ------------------- | ------------ |
| Read Uncommitted    | Yes        | Yes                 | Yes          |
| Read Committed      |            | Yes                 | Yes          |
| Repeatable Read     |            |                     | Yes          |
| Serializable        |            |                     |              |

**Read Uncommitted**: a transaction A will read the updated but not committed data of another transaction B. If B rolls back, the data is the **Dirty Read**

**Read Committed**: In one transaction A, a record is repeatedly read. If another transaction B change the value of the record during A, the values of the same record in A may be different.

**Repeatable Read**: In transaction A, a record isn't found but when trying to update the record, it appears and can be found because another transaction has inserted it into the table in between the steps of A. This is called **Phantom Reading**.

**Serializable**: the most strict isolation level. 

## 
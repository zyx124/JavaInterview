# Database



## JDBC

use `PreparedStatement` instead of `statement` to prevent SQL injection.

**ACID transaction**

- Atomicity
- Consistency
- Isolation
- Duration

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
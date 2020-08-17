# Spring 

## 

Spring

## IOC

Responsibility of IoC container:

- Dependent Injection & Dependent lookup
- Life cycle management: container, resources (java beans)
- Configuration





## JPA

**how to handle transactions?**

 --- @transactional

**update entities in several steps, one step fails, what will you do?**

 --- @transactional to put them in one transaction, if any step fails, rollback the entire transaction

**How do you handle exceptions in rest api/Spring MVC?** 

--- @ExceptionHandler

**How do you turn on 2nd level cache and what is the cache provider??** 

--- how: set property use_second_level_cache to TRUE.

--- what: EHCache

**query cache**

--- does not store entities, 

**improve performance**

--- bulk updates, cache result, fetchtype=LAZY

**Pagination** is used when dealing a large amount of data.
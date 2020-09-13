# Spring 



## HTTP code

2xx: successful response

200 OK, 201 created, 

3xx: redirects

4xx: client side errors

400 bad requests, 403 Forbidden, 404 Not found, 405 method not allowed,

5xx: server side error

500 internal server error, 502 bad gateway

## IOC

Responsibility of IoC container:

- Dependent Injection & Dependent lookup
- Life cycle management: container, resources (java beans)
- Configuration

Inversion of Control means that the components are not created and managed by the program. The management of components is given to the IOC container. We just need to **inject** the components (beans) into the program. 

**Types of Dependency Injection**:

constructor-based:

```java
public class Controller {
    private Helper helper;
    
    @Autowired
    public Controller(Helper helper, String flow) {
        this.helper = helper;
        this.flow = flow;
    }
}
```

```xml
<beans>
	<bean id="Controller" class="com.demo.controller.Controller">
    	<constructor-arg ref="helper" />
        <constructor-arg type="java.lang.String" value="new" />
    </bean>
    <bean id="helper" class="com.demo.service.StudentHelper"></bean>
</beans>
```

setter-based

```java
public class Controller {
    private Helper helper;
    
    @Autowired
    public void setHelper(Helper helper) {
        this.helper = helper;
    }
}
```

field-based

```java
public class Controller {
    @Autowired
    private StudentHelper helper;
    
}
```

**`BeanFactory` vs `ApplicationContext`**

`ApplicationContext` extends `BeanFactory`, which means they both support bean instantiation/wiring. `ApplicationContext` supports more functions like `ApplicationEvent` publication and internationalization.

**Bean Scope**

| Scope       | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| singleton   | Default. A bean is created as a singleton.                   |
| prototype   | A single bean can have any number of instances.              |
| request     | every HTTP request will generate a new bean which only lives in this  request |
| session     | every HTTP request will generate a new bean which only lives in this  session |
| application | `ServletContext`                                             |
| websocket   | `WebSocket`                                                  |

**Bean life cycle**

![lifecycle](http://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-9-17/48376272.jpg)

**How to inject with names**

`@Bean("name")`or `@Bean("name") + @Qualifier("name")`

**difference of `@Bean` and `@Component`**

`@Component` applies to classes, while `@Bean` applies to methods. 



## MVC

`@RestController = @Controller + @ResponseBody`

**how to define input and output to json in controllers**

```
@PostMapping(value="rest",
			 consumes = "application/json;charset=UTF-8", 
			 produces = "application/json;charset=UTF-8")
```



## Hibernate

In Hibernate, **do not use** primitive types like `long`, use boxed types like `Long, Integer`.



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
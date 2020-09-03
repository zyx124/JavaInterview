# Spring 



## HTTP code

2xx: successful response

3xx: redirects

4xx: client side errors

5xx: server side error

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
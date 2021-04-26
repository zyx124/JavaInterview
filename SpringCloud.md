# SpringCloud

How to use Nacos as configuration center?

- inject dependencies 

- create `bootstrap.properties` and add address and app name to it

- add a Data Id to the center named as `[NAME].properties`

- add configurations to the Data Id file

- Add `@RefreshScope` for the app to dynamically change the value. Add `@Values(${CONFIG_CONTENT})`  to the values from the Data ID that need to be injected into the app.

Configuration Center:

- Namespace: it is for isolations of configurations. Default is public. The namespace can be changed by adding **namespace ID** to `bootstrap.properties`. 
- Data ID
- Group: for different situations, like Black Friday



**Spring Cloud Gateway**

**route:** defined by an ID and a destination URI, a collection of filters and a collection of predicates. A route is matched if aggregate predicate is true.

**predicate**: input type is `ServerWebExchange`. This allows developers to match on anything from the HTTP requests.

**Filter:** requests and responses can be modified before or after sending the downstream request.



**JSR303**

1. Add validation annotation for the bean and redefine the message. (`javax.validation.constraints`)

2. Turn on the validation process. `@Valid`

3. Add a `BindingResult`, then we can check the result of the validation.

4. Group the validation. 

   1. `@NotBlank(message = "Cannot be blank!", group = {UpdateGroup.class})`
   2. Add `@Validated({UpdateGroup.class})` in the controller to specify the group
   3. Validation annotations without group will not be validated in group-specified validation. 

5. Customized validation

   1. Define a customized validation annotation

   2. Create a validator `ConstraintValidator`

   3. Connect the annotation and the validator

      ```java
      @Documented
      // can assign multiple validators here
      @Constraint(validatedBy = {ListValueConstraintValidator.class})
      @Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
      @Retention(RetentionPolicy.RUNTIME)
      public @interface ListValue {
          String message() default "{com.zyx.common.valid.ListValue.message}";
      
          Class<?>[] groups() default {};
      
          Class<? extends Payload>[] payload() default {};
      
          int[] vals() default {};
      }
      ```

      

## Different Objects in Mall Applications

**PO (Persistent Object):** A record in some table, which doesn't have any operations in the database.

**DO (Domain Object)**: Entity to realize the domain model. If the model consists of restaurant, order and customer, the DOs are like Restaurant, Order and Customer. Not all objects in the domain model are necessarily to be DO. For example, a phone number is a **value object**.

**DTO (Data Transfer Object**)

**VO (Value Object)**: Data transfer object between services, only contains data objects. 

**BO (Business Object)**: Encapsule the business logic into an object, which may include multiple POs.

**POJO (Plain Ordinary Java Object)**: A java object that is bound to no specific framework. JavaBean is a type of POJO, which has access levels (private properties and getter and setters), method names, default constructor ands serializable.

**DAO (Data Access Object)**: for data persistent to access the database.


https://donate.cafe/ricky



## JMeter

Concepts:

- Response Time
- Hits per second (HPS)
- Query per second (QPS)
- Maxium/Minimum Response Time 
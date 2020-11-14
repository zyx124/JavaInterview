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





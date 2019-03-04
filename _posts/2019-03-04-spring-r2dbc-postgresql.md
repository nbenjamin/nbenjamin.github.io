---
layout: post
title: Spring Data R2dbc PostgreSQL example
categories:
  - spring
  - R2dbc
  - postgresql
comments: true
---
In this post, we will be creating a reactive rest application using [Spring webFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html) and  [spring-data-r2dbc](https://github.com/spring-projects/spring-data-r2dbc)

### Dependency required for spring-data-r2dbc
```xml
<!-- webflux -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>

<!-- spring-data-r2dbc-->
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-r2dbc</artifactId>
    <version>${spring-data-r2dbc.version}</version>
  </dependency>
  <dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-postgresql</artifactId>
    <version>${r2dbc.version}</version>
  </dependency>
```

### Setup your environment
1. This example use [docker](https://www.docker.com/) in locally to spin up postgreSQL
2. If you have Docker installed in your system, please run the following command to start
[PostgreSQL](https://www.postgresql.org/) and [Adminer](https://www.adminer.org/) (Database management web tool)

  ```properties
  docker-compose -f docker/docker-compose.yml up
  ```
3. Verify postgreSQL is up and running

  ```properties
  docker ps -a | grep postgres
  ```
output

  ```properties
  48414afd73fc  postgres   "docker-entrypoint.s…"   2 days ago  etc....

  ```
4. Verify [Adminer](https://www.adminer.org/) (Database management web tool) is up and running

  ```properties
  docker ps -a | grep adminer
  ```
output

  ```properties
  3b2402e3d8cb        adminer                                 "entrypoint.sh docke…"   2 days ago
  ```
  
5. You can access [Adminer](https://www.adminer.org/) from browser using the following url
  ```properties
  http://localhost:8080/
  ```

### Register `ConnectionFactory`

```java

@Configuration
@EnableR2dbcRepositories(basePackages = "com.nbenja.spring.r2dbcpostgresqlexample.repository")
public class R2dbcPostgresqlConfiguration extends AbstractR2dbcConfiguration {

  @Value("${datasource.host}")
  private String host;
  @Value("${datasource.port}")
  private int port;
  @Value("${datasource.database}")
  private String database;
  @Value("${datasource.username}")
  private String username;
  @Value("${datasource.password}")
  private String password;
@Bean
@Override
public PostgresqlConnectionFactory connectionFactory() {
  return new PostgresqlConnectionFactory(PostgresqlConnectionConfiguration
      .builder()
      .host(host)
      .database(database)
      .username(username)
      .password(password)
      .port(port)
      .build());
}
}
```
Once we extend `AbstractR2dbcConfiguration` and provide the right `ConnectionFactory` spring-data-r2dbc framework will register `DatabaseClient` for database interaction for Repository implementation.

### Configure your Repository

Currently `spring-data-r2dbc` provide two interaces to the Repository layer.

1. R2dbcRepository
2. ReactiveCrudRepository

In our example we extend `ReactiveCrudRepository`

```java
public interface UserRepository extends ReactiveCrudRepository<User, Integer> {

}

```


### Unit test

The unit testcases will be using [TestContainers](https://www.testcontainers.org/) to spin up the lightweight
postgresql container instance.

### Running the application
Right click and run`R2dbcPostgresqlExampleApplication` from any IDE or you can run
from command line using the following

```properties
java -jar target/r2dbc-postgresql-example-0.0.1-SNAPSHOT.jar
```

##### Create a new USER

```properties
curl -X POST \
  http://localhost:7878/users \
  -H 'Content-Type: application/json' \
  -d '{
	"name": "demo"
}'
```

##### get all USERS
```properties
curl -X GET http://localhost:7878/users

response:
[{"id":1,"name":"demo"}]%
```

The code used in this article can be found [here](https://github.com/nbenjamin/spring-data-r2dbc-examples/tree/master/r2dbc-postgresql-example) .

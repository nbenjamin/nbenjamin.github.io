---
layout: post
title: Java 12 features
categories:
  - java
  - java 12
  - teeing
  - switch statement
comments: true
---

`Preview Feature` - This feature enables to use features which are not finalized or not really ready, this might be ready in upcoming releases.

By default, this feature is disabled in Java 12 both in compile and runtime.

Enable preview feature in compile time

```bash
javac --enable-preeview Foo.java
```

Enable preview feature in runtime
```bash
java --enable-preview Foo
```

To enable this for jar
```bash
java --enable-preview -jar microservice.jar
```

#### Switch Expression (Preview language feature)
Java 12 supports multiple comma-separated labels in a single switch label.

Prior to java 12

```java
switch (day) {
    case "M":
    case "T":
    case "W":
    case "F": 
        System.out.println("Weekdays");
        break;
    case "S":
        System.out.println("Weekends");
        break;
```

Java 12 looks like this
```java
switch (day) {
    case "M", "T", "W", "F" -> System.out.println("Weekdays");
    case "S" -> System.out.println("Weekends");
```

Switch statement expression can be assign to variable, e.g:

```java
String type = switch (scanner.next()) {
    case "M", "T", "W", "F" -> "Weekdays";
    case "S" -> "Weekends";

    default -> throw new IllegalStateException("Unexpected value: " + scanner.next());
};

System.out.println(" Switch return type - "+ type);
```

Switch case response can have a full block of code, in that case, `break` will use to return the value.
Similar to `return some value` break will have something like this `break value`


### Collectors.teeing

A new static method `teeing` to collectors interface, which allows collecting
two independent collectors and then merge their results using the supplied function.

ex:
```java
BiFunction<Optional<Integer>, Optional<Integer>, Optional<String>> ofOp = (o, o2) -> {
    return Optional.of("min value - " + o.get() +" Max value - "+ o2.get());
};

Optional<String> result = Stream.of(5,11,1,9,3)
        .collect(teeing(minBy(Integer::compareTo), maxBy(Integer::compareTo),ofOp));

System.out.println(result.get());
```

### String literal changes ( Not yet supported as of now)

Traditional String Literals
```java
String html = "<html>\n" +
              "    <body>\n" +
              "		    <p>Hello World.</p>\n" +
              "    </body>\n" +
              "</html>\n";
```

            
Raw String Literals

```java
String html = `<html>
                   <body>
                       <p>Hello World.</p>
                   </body>
               </html>
              `;
```


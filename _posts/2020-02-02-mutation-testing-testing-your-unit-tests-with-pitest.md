---
title: "Mutation Testing - Testing your unit tests with PiTest"
summary: "XXX"
tags:
    - " #java"
    - " #unittests"
    - " #mutationtesting"
---

# What is Mutation Testing?

Mutation Testing is a type of software testing where we mutate certain statements in the source code and check if the test cases can find the errors.

The main idea of Mutation Testing is to help the developers to find false positive unit tests, in other words, unit tests that pass even when the code was mutated and return an error.

This is a good practice to elevate the quality of delivered code and reduce bugs.

# Why PITest?

There are other mutation testing systems for Java, but they are not widely used.

They are mostly slow, difficult to use and written to meet the needs of academic research rather than real development teams.

PIT is different. It's

* Fast - can analyse in minutes what would take earlier systems days
* Easy to use - works with ant, maven, gradle and others
* Actively developed
* Actively supported

# Who PITest works:

PIT runs your unit tests against automatically modified versions of your application code. When the application code changes, it should produce different results and cause the unit tests to fail. If a unit test does not fail in this situation, it may indicate an issue with the test suite.

# Let us code?

It quite simple to configure the PITest, we only need to add a plugin 

## Create an application

To create an application we are going to use Maven, so run the command below to create the app:

```
mvn archetype:generate \
-DarchetypeGroupId=org.apache.maven.archetypes \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DarchetypeVersion=1.4 \
-DgroupId=com.example \
-DartifactId=pitest \
-Dpackage=com.example.pitest
```

## Maven

Add this block of code in your `pom.xml`

```
<plugin>
    <groupId>org.pitest</groupId>
    <artifactId>pitest-maven</artifactId>
    <version>1.4.11</version>
    <executions>
        <execution>
            <id>mutation-coverage</id>
            <phase>test</phase>
            <goals>
                <goal>mutationCoverage</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Coding

In this example we will use pure Java, so let's start creating the `Application.java`

```
public class Application {

  public static void main(String[] args) {
      // TODO
  }

}
```

Our use case is a product service, so we need to represent it, so let's create the `Product.java`

```
public class Product {

  private String id;

  private String name;

  private Double value;

  private Integer quantity;

  // TODO - Getters and Setters
  // TODO - Equals and HashCode
  // TODO - ToString

}
```

Now it is time to create a Repository, as we are using pure Java, we will represent the repository 
using `Map`, so let's do it creating the `ProductRepository.java`

```
public class ProductRepository {

  private final Map<String, Product> products = new HashMap<>();

}
```

Now to represent the business logic, we need to create a Service,  so let's do it creating the 
`ProductService.java`

```
public class ProductService {

  private final ProductRepository repository = new ProductRepository();

}
```

Well, all the structure has been defined then it is time to code some flows, such as saving flow

Into your `ProductRepository` java file add the method `save`

```
public Product save(Product product) {
    System.out.println("Saving " + product);
    products.putIfAbsent(product.getId(), product);
    System.out.println("Saved " + product);

    return product;
  }
```

Into your `ProductService` java file add the method `create`

```
public Product create(String name, Double value, Integer quantity) {
    Product product = new Product(UUID.randomUUID().toString(), name, value, quantity);
    System.out.println("Creating " + product);
    repository.save(product);
    System.out.println("Created " + product);

    return product;
  }
```

## Testing the Application

To test the application use the `Application.java` and add this block of code

```
public static void main(String[] args) {
      ProductService service = new ProductService();
      Product product = service.create("Product B", 55.5, 10);
      System.out.println(product);
  }
```

Now run the `main method` and the output should be like this:

```
Creating Product{id='72ae61a1-96af-44c6-aeab-6276a5ee9293', name='Product B', value=55.5, quantity=10}

Saving Product{id='72ae61a1-96af-44c6-aeab-6276a5ee9293', name='Product B', value=55.5, quantity=10}

Saved Product{id='72ae61a1-96af-44c6-aeab-6276a5ee9293', name='Product B', value=55.5, quantity=10}

Created Product{id='72ae61a1-96af-44c6-aeab-6276a5ee9293', name='Product B', value=55.5, quantity=10}
```

## Testing Pitest

We have configured the plugin to run the goal `mutationCoverage` after the maven `test` phase, so run commando below: 

`mvn clean test`

# References

[PITest](https://pitest.org/)

[Mutation Testing](https://www.guru99.com/mutation-testing.html)

# Codebase

**Be Happy**
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

## Testing

We have configured the plugin to run the goal `mutationCoverage` after the maven `test` phase, so run commando below: 

`mvn clean test`

# References

[PITest](https://pitest.org/)

[Mutation Testing](https://www.guru99.com/mutation-testing.html)

# Codebase

**Be Happy**
# Quarkus Challenge

## Introduction

The main goal will be for you to create a Microservice to expose simple CRUD-like capabilities using Quarkus. The Quarkus Application you are coding should expose
a REST API that allows operations on books stored in a Postgres database. If you have never used Quarkus before, remember that you can attend the Quarkus workshop 
before the Games. The only requirement is Java basic development knowledge.

Throughout this challenge we propose to use the following JPA Entity:

```java
package com.santander.games.challenges.quarkus;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Book {

    @Id
    private Integer id;

   public String name;

   public Integer publicationYear;

    public Integer getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public Integer getPublicationYear() {
        return publicationYear;
    }
}
```

You have to provide a CRUD service exposing the following endpoints over REST:
- An endpoint returning all the books stored in the database. Suggested path to access this endpoint: `/books`, `/all`
- Endpoint allowing to retrieve a book by his name. Suggested path to access this endpoint: `/books/{name}`, `/books/byName/{name}`
- Endpoint allowing to retrieve a book which the publication year is between two provided values. Suggested path to access this endpoint: `/books/{lowerYear}/{higherYear}`, `/books/byPublicationYearBetween/{lowerYear}/{higherYear}`
You can of course add others endpoints if you want.

You are encouraged to use Hibernate ORM with Panache for Relational Database access but if you are a Spring Developer, you could be interested in using the Quarkus Spring extensions. 
Quarkus provides a compatibility layer for Spring Data JPA repositories in the form of the spring-data-jpa extension. Take a look at what is available in the Quarkus.io site

## Bootstrapping the project

First, you need a new project. The easiest way to create a new Quarkus project is to open a terminal and run the following command:

```shell
mvn io.quarkus.platform:quarkus-maven-plugin:2.2.1.Final:create \
    -DprojectGroupId=org.acme \
    -DprojectArtifactId=quarkus-challenge \
    -DclassName="org.acme.getting.started.GreetingResource" \
    -Dpath="/hello"
```
This command allows to bootstrap a maven based application and creates a JAX-RS endpoint.
NOTE: check the last Quarkus version available in the [site](https://quarkus.io/)

[comment]: <> (Then, depending on the implementation that you have chosen, Hibernate ORM with Panache or Quarkus Spring extensions, you should add the corresponding dependencies to the `pom.xml`.)

As already mentioned, the application that you are coding allows operations on books stored in a Postgres database, thus you need a Postgres database. 
When testing or running in dev mode you should use the postgres database provided out of the box by DevServices from Quarkus. Check how it works [here](https://quarkus.io/guides/datasource#dev-services)

The following SQL statements will help you to populate the database. To load them when Hibernate ORM starts, add an import.sql file to the root of your resources directory and add the relevant configuration properties in `application.properties`.
```sql
INSERT INTO book(id, name, publicationYear) VALUES (1, 'Sapiens' , 2011);
INSERT INTO book(id, name, publicationYear) VALUES (2, 'The Neverending Story' , 1979);
INSERT INTO book(id, name, publicationYear) VALUES (3, 'Behind Closed Doors' , 2018);
INSERT INTO book(id, name, publicationYear) VALUES (4, 'Eleanor Oliphant is Completely Fine' , 2018);
INSERT INTO book(id, name, publicationYear) VALUES (5, 'Inferno' , 2013);
INSERT INTO book(id, name, publicationYear) VALUES (6, 'The Silk Roads' , 2015);
```

For prod mode, as this exercise is focused on creating the Quarkus Application, you will be provided all the resources needed to have a Postgres database running in the Openshift cluster. You will only need to configure the Quarkus Application to connect such Database.

Now you should be ready to run the boostraped application. The dev mode is an awesome mode that Quarkus has that allows you to write code and get the result of your changes without having to restart the application at all.
To start the application in `dev` mode use: ./mvnw compile quarkus:dev.
This will also listen for a debugger on port 5005. If you want to wait for the debugger to attach before running you can pass `-Dsuspend` on the command line. If you don’t want the debugger at all you can use `-Ddebug=false`.

Once started, you can request the provided endpoint: 
```shell
curl -w "\n" http://localhost:8080/hello
```

Now you are set! Depending on the implementation that you have chosen, Hibernate ORM with Panache or Quarkus Spring extensions, add the corresponding dependencies to the `pom.xml` and start coding the Library Application!

## Packaging and run the application
The application is packaged using `./mvnw package`. Check the information in the `README.md` file that will help you with some basics commands.

## Containerization and deployment in OpenShift
In the second part of the challenge, once the Quarkus Application is coded and is accepting requests locally, you should package it and deploy it to the provided the OpenShift Cluster. For that, I recommend using the [Quarkus OpenShift extension](https://quarkus.io/guides/deploying-to-openshift). This extension offers the ability to generate OpenShift resources. Once the resources have been generated, you can deploy them by running the command `oc apply -f path_to_file`
If you are not familiar with the OpenShift client, the `oc` command, pass the `quarkus.kubernetes.deploy` flag in the command line to build and deploy the application in a single step.


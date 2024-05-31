# Learning Center Platform - Part 1

## Table of contents



## Creación del project

Cargar el navegador y generar un proyecto Spring a partir del siguiente enlace:

https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.2.6&packaging=jar&jvmVersion=21&groupId=pe.edu.upc.center&artifactId=learning-center-platform&name=learning-center-platform&description=Demo%20project%20for%20Spring%20Boot&packageName=pe.edu.upc.center.platform&dependencies=data-jpa,validation,web,devtools,postgresql,lombok

Generar el project Spring, Guardarlo en la carpeta `IdeaProjects` y luego abrirlo el `IntelliJ IDEA`.

## Configuración inicial

### Base de datos

Abrir pgAdmin y crear la base de datos: `learningws5x`.

### pom.xml

Abrir el archivo `pom.xml` y agregar las siguientes dependencias:

```xml
        <!-- https://mvnrepository.com/artifact/io.github.encryptorcode/pluralize -->
        <dependency>
            <groupId>io.github.encryptorcode</groupId>
            <artifactId>pluralize</artifactId>
            <version>1.0.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-starter-webmvc-ui -->
        <dependency>
            <groupId>org.springdoc</groupId>
            <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
            <version>2.5.0</version>
        </dependency>
```

### LearningCenterPlatformApplication

Abrir el archivo `LearningCenterPlatformApplication.java` ubicado en el package `pe.edu.upc.learning.platform` y agregar la anotación `@EnableJpaAuditing` debajo de la anotación `@SpringBootApplication`.

La clase debe quedar:
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@SpringBootApplication
@EnableJpaAuditing
public class LearningCenterPlatformApplication {

	public static void main(String[] args) {
		SpringApplication.run(LearningCenterPlatformApplication.class, args);
	}
}
```

### application.properties

Abrir el archivo `application.properties` y reemplazar con el siguiente código:
```ini
# Spring Application Name
spring.application.name=ACME Learning Center Platform

# Spring DataSource Configuration
###    JDBC : SGDB :// HOST : PORT / DB
spring.datasource.url: jdbc:postgresql://localhost:5432/learningws53
spring.datasource.username: postgres
spring.datasource.password: 1234
spring.datasource.driver-class-name: org.postgresql.Driver

# Spring Data JPA Configuration
spring.jpa.database: postgresql
spring.jpa.show-sql: true

# Spring Data JPA Hibernate Configuration
spring.jpa.hibernate.ddl-auto: update
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.format_sql: true
spring.jpa.properties.hibernate.dialect: org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.naming.physical-strategy=pe.edu.upc.center.platform.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port: 8090
```

## Package shared

### Project structur for shared

En el package principal `pe.edu.upc.learning.platform`, crear la siguiente estructura para el package `shared`.

```markdown
- 📁 shared
  - 📁 domain.model
    - 📁 aggregates
      - 📄 AuditableAbstractAggregateRoot.java
    - 📁 entities
      - 📄 AuditableModel.java
  - 📁 infrastructure
    - 📁 documentation.openapi.configuration
    - 📁 persistence.jpa.configuration.strategy
      - 📄 SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
  - 📁 interfaces.rest.resources
    - 📄 MessageResource.java
```

### Package domain.model aggregates

En el package `aggregates` crear la clase `AuditableAbstractAggregateRoot` con el siguiente contenido:

```java
import jakarta.persistence.*;
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.domain.AbstractAggregateRoot;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import java.util.Date;

@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class AuditableAbstractAggregateRoot<T extends AbstractAggregateRoot<T>> extends AbstractAggregateRoot<T> {

  @Id
  @Getter
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Getter
  @CreatedDate
  @Column(nullable = false, updatable = false)
  private Date createdAt;

  @Getter
  @LastModifiedDate
  @Column(nullable = false)
  private Date updatedAt;
}
```
### Package domain.model entities

En el package `entities` crear la clase `AuditableModel` con el siguiente contenido:

```java
import jakarta.persistence.Column;
import jakarta.persistence.EntityListeners;
import jakarta.persistence.MappedSuperclass;
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import java.util.Date;

@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class AuditableModel {

  @Getter
  @CreatedDate
  @Column(nullable = false, updatable = false)
  private Date createdAt;

  @Getter
  @LastModifiedDate
  @Column(nullable = false)
  private Date updatedAt;
}
```

### Package infrastructure persistence.jpa.strategy

En el package `persistence.jpa.strategy` crear la clase `SnakeCaseWithPluralizedTablePhysicalNamingStrategy` con el siguiente contenido:

```java
import org.hibernate.boot.model.naming.Identifier;
import org.hibernate.boot.model.naming.PhysicalNamingStrategy;
import org.hibernate.engine.jdbc.env.spi.JdbcEnvironment;

import static io.github.encryptorcode.pluralize.Pluralize.pluralize;

public class SnakeCaseWithPluralizedTablePhysicalNamingStrategy implements PhysicalNamingStrategy {
  @Override
  public Identifier toPhysicalCatalogName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
    return this.toSnakeCase(identifier);
  }

  @Override
  public Identifier toPhysicalSchemaName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
    return this.toSnakeCase(identifier);
  }

  @Override
  public Identifier toPhysicalTableName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {

    return this.toSnakeCase(this.toPlural(identifier));
  }

  @Override
  public Identifier toPhysicalSequenceName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
    return this.toSnakeCase(identifier);
  }

  @Override
  public Identifier toPhysicalColumnName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
    return this.toSnakeCase(identifier);
  }

  private Identifier toSnakeCase(final Identifier identifier) {
    if (identifier == null) {
      return null;
    }
    final String regex = "([a-z])([A-Z])";
    final String replacement = "$1_$2";
    final String newName = identifier.getText()
            .replaceAll(regex, replacement)
            .toLowerCase();
    return Identifier.toIdentifier(newName);
  }

  private Identifier toPlural(final Identifier identifier) {
    final String newName = pluralize(identifier.getText());
    return Identifier.toIdentifier(newName);
  }
}
```

### Package interfaces.rest.resources

En el package `interfaces.rest.resources` crear el record `MessageResource` con el siguiente contenido:
```java
public record MessageResource(String message) {
}
```

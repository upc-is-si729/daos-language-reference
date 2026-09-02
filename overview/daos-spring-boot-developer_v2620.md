# Hello Spring Boot Developer 

## Table of contents



## Creación del project

Cargar el navegador y generar un proyecto Spring a partir del siguiente enlace:

https://start.spring.io/#!type=maven-project&language=java&platformVersion=4.1.1&packaging=jar&configurationFileFormat=properties&jvmVersion=26&groupId=pe.edu.upc.hello.platform&artifactId=hello-spring-boot-developer&packageName=pe.edu.upc.hello.platform&dependencies=validation,web,devtools,lombok,springdoc-openapi

Generar el project Spring, Guardarlo en la carpeta `IdeaProjects` y luego abrirlo el `IntelliJ IDEA`.

## Configuración inicial

### pom.xml

Abrir el archivo `pom.xml` y agregar las siguientes dependencias:

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.20.0</version>
    <scope>compile</scope>
</dependency>    
```

### application.properties

Abrir el archivo `application.properties` y agregar el siguiente código:
```ini
server.port: 8090
```

## Profile Bounded generic

### Project structur for Generic Bounded Context

En el package principal `pe.edu.upc.hello.platform`, 
Crear la siguiente estructura para el Bounded Context `generic`:

```markdown
- 📁 profiles
  - 📁 domain
    - 📁 model
      - 📁 entity
      - 📁 valueobjects
    - 📁 service
      - 📁 internal
  - 📁 interfaces.rest
    - 📁 assemblers
    - 📁 controllers
    - 📁 resources
```

### Package domain.model valueobjects

En el package `domain.model.valueobjects` crear l record `PersonName` con el siguiente contenido:

```java
import org.apache.commons.lang3.StringUtils;

/**
 * Value object representing a person's name.
 * Encapsulates validation logic and formatting for first and last names.
 *
 * @param firstName The first name.
 * @param lastName  The last name.
 */
public record PersonName(String firstName, String lastName) {
  private static final int FIRST_NAME_MAX_LENGTH = 35;
  private static final int LAST_NAME_MAX_LENGTH = 40;

  public PersonName {
    if (StringUtils.isBlank(firstName)) {
      throw new IllegalArgumentException("First name cannot be blank");
    }
    if (firstName.length() > FIRST_NAME_MAX_LENGTH) {
      throw new IllegalArgumentException("First name must be at most " + FIRST_NAME_MAX_LENGTH + " characters");
    }
    if (StringUtils.isBlank(lastName)) {
      throw new IllegalArgumentException("Last name cannot be blank");
    }
    if (lastName.length() > LAST_NAME_MAX_LENGTH) {
      throw new IllegalArgumentException("Last name must be at most " + LAST_NAME_MAX_LENGTH + " characters");
    }
    firstName = firstName.trim();
    lastName = lastName.trim();
  }

  /**
   * Returns the full name by combining first and last names.
   *
   * @return The full name.
   */
  public String getFullName() {
    return String.format("%s %s", firstName, lastName);
  }
}
```

### Package domain.model entity

En el package `domain.model.entity` crear la clase `Developer` con el siguiente contenido:

```java
import com.acme.hello.platform.profiles.domain.model.valueobjects.PersonName;
import lombok.Getter;

import java.util.UUID;

/**
 * Represents a Developer entity.
 * <p>
 * This entity represents a developer in the system, with a unique identifier and basic profile information.
 * It also tracks the number of times the developer has been greeted.
 * </p>
 * <p>
 * The identifier is a time-ordered UUID v7.
 * </p>
 *
 * @author Open-Source Application Development Team
 * @version 1.3.0
 */
@Getter
public class Developer {
  /**
   * The unique identifier of the developer.
   * Uses UUID v7 for time-ordered, lexicographically sortable identifiers.
   */
  private final UUID id;

  /**
   * The developer's name.
   */
  private PersonName name;

  /**
   * The number of times this developer has been greeted.
   */
  @Getter
  private static int greetingCount = 0;

  /**
   * Constructs a new Developer with the given first and last names.
   * Initializes the greeting count to zero and generates a time-ordered UUID v7.
   *
   * @param firstName the developer's first name
   * @param lastName  the developer's last name
   * @throws IllegalArgumentException if either name is blank or exceeds the maximum length
   */
  public Developer(String firstName, String lastName) {
    this.id = UUID.ofEpochMillis(System.currentTimeMillis());
    this.name = new PersonName(firstName, lastName);
  }

  /**
   * Updates the developer's first name.
   *
   * @param firstName the new first name
   * @throws IllegalArgumentException if the first name is blank or exceeds the maximum length
   */
  public void setFirstName(String firstName) {
    this.name = new PersonName(firstName, this.name.lastName());
  }

  /**
   * Updates the developer's last name.
   *
   * @param lastName the new last name
   * @throws IllegalArgumentException if the last name is blank or exceeds the maximum length
   */
  public void setLastName(String lastName) {
    this.name = new PersonName(this.name.firstName(), lastName);
  }

  /**
   * Returns the developer's first name.
   *
   * @return the first name
   */
  public String getFirstName() {
    return name.firstName();
  }

  /**
   * Returns the developer's last name.
   *
   * @return the last name
   */
  public String getLastName() {
    return name.lastName();
  }

  /**
   * Returns the full name of the developer.
   *
   * @return the combined first and last names
   */
  public String getFullName() {
    return name.getFullName();
  }

  /**
   * Increments the greeting counter for this developer.
   */
  public void incrementGreetingCount() {
    Developer.greetingCount++;
  }
}
```

### Package domain service

En el package `domain.service` crear el interface `GreetingCounter` con el siguiente contenido:

```java
public interface GreetingCounter {
  int getGreetingCount();
  void incrementGreetingCount();
}
```

### Package domain.service internal

En el package `domain.service.internal` crear la clase `GreetingCounterImpl` con el siguiente contenido:

```java
import com.acme.hello.platform.profiles.domain.service.GreetingCounter;
import org.springframework.stereotype.Service;

@Service
public class GreetingCounterImpl implements GreetingCounter {
  private int greetingCount = 0;

  @Override
  public int getGreetingCount() {
    return greetingCount;
  }

  @Override
  public void incrementGreetingCount() {
    greetingCount++;
  }
}
```

### Package domain.model.entity

En el package `domain.service` crear el interface `GreetingCounter` con el siguiente contenido:

```java
public interface GreetingCounter {
  int getGreetingCount();
  void incrementGreetingCount();
}
```


### Package interfaces.rest resources

En el package `resources` crear el record `GreetDeveloperRequest` con el siguiente contenido:

```java
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

/**
 * Request to greet a developer.
 */
public record GreetDeveloperRequest(
    @NotBlank(message = "First name cannot be blank")
    @Size(max = 35, message = "First name cannot exceed 35 characters") String firstName,
    @NotBlank(message = "Last name cannot be blank")
    @Size(max = 40, message = "Last name cannot exceed 40 characters") String lastName) {
}
```

En el package `resources` crear el record `GreetDeveloperResponse` con el siguiente contenido:

```java
import java.util.UUID;

/**
 * Response to a greet developer request.
 */
public record GreetDeveloperResponse(UUID id, String fullName) {
}
```

En el package `resources` crear el record `GetGreetingCountResponse` con el siguiente contenido:

```java
/// A response for getting the total number of greetings.
/// @param greetingCount the greeting count since service start
public record GetGreetingCountResponse(int greetingCount) {}
```

### Package interfaces.rest assemblers

En el package `assemblers` crear la clase `GreetDeveloperAssembler` con el siguiente contenido:

```java
import com.acme.hello.platform.profiles.domain.model.entity.Developer;
import com.acme.hello.platform.profiles.interfaces.rest.resources.GreetDeveloperResponse;

/**
 * Assembler for greeting developer resources.
 */
public class GreetDeveloperAssembler {

  /**
   * Converts a Developer entity to a GreetDeveloperResponse.
   *
   * @param entity the developer entity
   * @return the greet-developer response
   */
  public static GreetDeveloperResponse toResponseFromEntity(Developer entity) {
    return new GreetDeveloperResponse(entity.getId(), entity.getFullName());
  }
}
```


### Package interfaces.rest controllers

En el package `controllers` crear la clase `GreetingsController` con el siguiente contenido:

```java
import com.acme.hello.platform.profiles.domain.model.entity.Developer;
import com.acme.hello.platform.profiles.domain.service.GreetingCounter;
import com.acme.hello.platform.profiles.interfaces.rest.assemblers.GreetDeveloperAssembler;
import com.acme.hello.platform.profiles.interfaces.rest.resources.GetGreetingCountResponse;
import com.acme.hello.platform.profiles.interfaces.rest.resources.GreetDeveloperRequest;
import com.acme.hello.platform.profiles.interfaces.rest.resources.GreetDeveloperResponse;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.validation.Valid;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.net.URI;

/**
 * Controller for greeting developers.
 */
@RestController
@RequestMapping("/api/v1/greetings")
@Tag(name = "Greetings", description = "Endpoints for greeting developers and tracking statistics")
public class GreetingsController {
  private final GreetingCounter greetingCounter;

  /// Creates a new GreetingsController with the specified GreetingCounter.
  /// @param greetingCounter the service instance for greeting counting
  public GreetingsController(GreetingCounter greetingCounter) {
    this.greetingCounter = greetingCounter;
  }

  /**
   * Greets a developer.
   *
   * @param request the developer-to-greet request
   * @return the greeted developer response
   */
  @PostMapping
  public ResponseEntity<GreetDeveloperResponse> greetDeveloper(@Valid @RequestBody GreetDeveloperRequest request) {
    var developer = new Developer(request.firstName(), request.lastName());
    greetingCounter.incrementGreetingCount();
    var response = GreetDeveloperAssembler.toResponseFromEntity(developer);
    return ResponseEntity.created(URI.create("/api/v1/greetings/" + developer.getId())).body(response);
  }

  /**
   * Gets the total number of greetings.
   *
   * @return the total number of greetings
   */
  @GetMapping
  public ResponseEntity<GetGreetingCountResponse> getGreetingCount() {
    var response = new GetGreetingCountResponse(greetingCounter.getGreetingCount());
    return ResponseEntity.ok(response);
  }
}
```

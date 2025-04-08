# Hello Spring Boot Developer 

## Table of contents



## Creación del project

Cargar el navegador y generar un proyecto Spring a partir del siguiente enlace:

https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.4.4&packaging=jar&jvmVersion=21&groupId=pe.edu.upc.hello.platform&artifactId=hello-spring-boot-developer&name=hello-spring-boot-developer&description=Demo%20project%20for%20Spring%20Boot&packageName=pe.edu.upc.hello.platform&dependencies=web,devtools,lombok

Generar el project Spring, Guardarlo en la carpeta `IdeaProjects` y luego abrirlo el `IntelliJ IDEA`.

## Configuración inicial

### pom.xml

Abrir el archivo `pom.xml` y agregar las siguientes dependencias:

```xml
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.14.0</version>
        </dependency>        
```

En el archivo `pom.xml` y agregar la siguiente linea: `<version>${lombok.version}</version>`:

```xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<annotationProcessorPaths>
						<path>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
							<version>${lombok.version}</version>	<!-- Add here -->
						</path>
					</annotationProcessorPaths>
				</configuration>
			</plugin>    
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
- 📁 generic
  - 📁 domain.model.entity
  - 📁 interfaces.rest
    - 📁 assemblers
    - 📁 controllers
    - 📁 resources
```

### Package domain.model.entity

En el package `domain.model.entity` crear la clase `Developer` con el siguiente contenido:

```java
import lombok.Builder;
import lombok.Getter;
import org.apache.commons.lang3.StringUtils;

import java.util.UUID;

@Getter
@Builder
public class Developer {
  private final UUID id = UUID.randomUUID();
  private final String firstName;
  private final String lastName;

}
```

Agregue un contructor private a la clase `Developer`:

```java
  private Developer(String firstName, String lastName) {
    this.firstName = StringUtils.trimToEmpty(firstName);
    this.lastName = StringUtils.trimToEmpty(lastName);
  }
```

Agregue los métodos de manipulación de atributos a la clase `Developer`:

```java
  public String getFullName() {
    return firstName + " " + lastName;
  }
  public boolean isAnyNameBlank() {
    return StringUtils.isAnyBlank(firstName, lastName);
  }
  public boolean isAnyNameEmpty() {
    return StringUtils.isAnyEmpty(firstName, lastName);
  }
```


### Package interfaces.rest resources

En el package `resources` crear el record `GreetDeveloperRequest` con el siguiente contenido:

```java
import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;

public record GreetDeveloperRequest(String firstName, String lastName) {

  @JsonCreator
  public GreetDeveloperRequest(@JsonProperty("firstName") String firstName,
                               @JsonProperty("lastName") String lastName) {
    this.firstName = firstName != null ? firstName : "";
    this.lastName = lastName != null ? lastName : "";
  }
}
```

En el package `resources` crear el record `GreetDeveloperResponse` con el siguiente contenido:

```java
import java.util.UUID;

public record GreetDeveloperResponse(UUID id, String fullName, String message) {

  public GreetDeveloperResponse(String message) {
    this(null, null, message);
  }
}
```

### Package interfaces.rest assemblers

En el package `assemblers` crear la clase `DeveloperAssembler` con el siguiente contenido:

```java
import org.apache.commons.lang3.ObjectUtils;
import org.apache.commons.lang3.StringUtils;
import pe.edu.upc.hello.platform.generic.domain.model.entity.Developer;
import pe.edu.upc.hello.platform.generic.interfaces.rest.resources.GreetDeveloperRequest;

public class DeveloperAssembler {

  public static Developer toEntityFromRequest(GreetDeveloperRequest request) {
    if (ObjectUtils.isEmpty(request) ||
        StringUtils.isAnyBlank(request.firstName(), request.lastName())) {
      return null;
    }

    return Developer.builder()
        .firstName(request.firstName())
        .lastName(request.lastName())
        .build();
  }
}
```

En el package `assemblers` crear la clase `DeveloperAssembler` con el siguiente contenido:

```java
import org.apache.commons.lang3.ObjectUtils;
import org.apache.commons.lang3.StringUtils;
import pe.edu.upc.hello.platform.generic.domain.model.entity.Developer;
import pe.edu.upc.hello.platform.generic.interfaces.rest.resources.GreetDeveloperRequest;

public class DeveloperAssembler {

  public static Developer toEntityFromRequest(GreetDeveloperRequest request) {
    if (ObjectUtils.isEmpty(request) ||
        StringUtils.isAnyBlank(request.firstName(), request.lastName())) {
      return null;
    }

    return Developer.builder()
        .firstName(request.firstName())
        .lastName(request.lastName())
        .build();
  }
}
```


### Package interfaces.rest controllers

En el package `controllers` crear la clase `GreetingsController` con el siguiente contenido:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.hello.platform.generic.domain.model.entity.Developer;
import pe.edu.upc.hello.platform.generic.interfaces.rest.assemblers.DeveloperAssembler;
import pe.edu.upc.hello.platform.generic.interfaces.rest.assemblers.GreetDeveloperAssembler;
import pe.edu.upc.hello.platform.generic.interfaces.rest.resources.GreetDeveloperRequest;
import pe.edu.upc.hello.platform.generic.interfaces.rest.resources.GreetDeveloperResponse;

/**
 * REST controller for handling greeting-related requests.
 * Provides endpoints to retrieve and create greetings for developers.
 */
@RestController
@RequestMapping("/greetings")
public class GreetingsController {

  /**
   * Handles GET requests to retrieve a greeting for a developer based on query parameters.
   *
   * @param firstName the optional first name of the developer
   * @param lastName  the optional last name of the developer
   * @return a ResponseEntity containing a GreetDeveloperResponse with a 200 OK status
   */
  @GetMapping
  public ResponseEntity<GreetDeveloperResponse> greetDeveloper(
      @RequestParam(required = false) String firstName,
      @RequestParam(required = false) String lastName) {

    Developer developer = (firstName != null && lastName != null)
        ? Developer.builder().firstName(firstName).lastName(lastName).build()
        : null;

    GreetDeveloperResponse response = GreetDeveloperAssembler.toResourceFromEntity(developer);
    return ResponseEntity.ok(response);
  }

  /**
   * Handles POST requests to create a greeting for a developer based on the request body.
   *
   * @param request the GreetDeveloperRequest containing first and last names
   * @return a ResponseEntity containing a GreetDeveloperResponse with a 201 Created status
   */
  @PostMapping
  public ResponseEntity<GreetDeveloperResponse> createGreeting(
      @RequestBody GreetDeveloperRequest request) {

    Developer developer = DeveloperAssembler.toEntityFromRequest(request);

    GreetDeveloperResponse response = GreetDeveloperAssembler.toResourceFromEntity(developer);
    return ResponseEntity.status(HttpStatus.CREATED).body(response);
  }
}
```

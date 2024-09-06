# Learning Center Platform - Part 7

## Table of contents

## Configuración 

### pom.xml

Abrir el archivo `pom.xml` y agregar las siguientes dependencias:

```xml
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-security -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-api -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-api</artifactId>
            <version>0.12.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-impl -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-impl</artifactId>
            <version>0.12.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-jackson -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-jackson</artifactId>
            <version>0.12.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.12.0</version>
        </dependency>
```

### application.properties

Abrir el archivo `application.properties` y agregar el siguiente código al final del archivo:
```ini
# Application Information for Documentation

# Elements take their values from maven pom.xml build-related information
documentation.application.description=@project.description@
documentation.application.version=@project.version@

# JWT Configuration Properties
authorization.jwt.secret = WriteHereYourSecretStringForTokenSigningCredentials
authorization.jwt.expiration.days = 7
```

## Package shared

### Package infrastructure documentation.openapi.configuration

En el package `configuration` reemplazar la clase `OpenApiConfiguration` con el siguiente contenido:
```java
import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.ExternalDocumentation;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
import io.swagger.v3.oas.models.security.SecurityRequirement;
import io.swagger.v3.oas.models.security.SecurityScheme;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfiguration {
  @Bean
  public OpenAPI learningPlatformOpenApi() {
    // General configuration
    var openApi = new OpenAPI();
    openApi
        .info(new Info()
            .title("Learning Platform API")
            .description("Learning Platform application REST API documentation.")
            .version("v1.0.0")
            .license(new License().name("Apache 2.0")
                .url("https://springdoc.org")))
        .externalDocs(new ExternalDocumentation()
            .description("Learning Platform wiki Documentation")
            .url("https://github.com/upc-is-si729/daos-language-reference"));

    // Add security scheme
    final String securitySchemeName = "bearerAuth";

    openApi.addSecurityItem(new SecurityRequirement()
            .addList(securitySchemeName))
        .components(new Components()
            .addSecuritySchemes(securitySchemeName,
                new SecurityScheme()
                    .name(securitySchemeName)
                    .type(SecurityScheme.Type.HTTP)
                    .scheme("bearer")
                    .bearerFormat("JWT")));

    // Return OpenAPI configuration object with all the settings
    return openApi;
  }
}
```

## Identity and Access Management (IAM) Bounded Context

### Project structur for Profile Bounded Context

Crear la siguiente estructura para el Bounded Context `profiles`:

```markdown
- 📁 iam
  - 📁 application.internal
    - 📁 commandservices
    - 📁 eventhandlers
    - 📁 outboundservices
      - 📁 hashing
      - 📁 tokens
    - 📁 queryservices
  - 📁 domain
    - 📁 model
      - 📁 aggregates
      - 📁 commands
      - 📁 entities
      - 📁 queries
      - 📁 valueobjects
    - 📁 services
  - 📁 infrastructure
    - 📁 authorization.sfs
      - 📁 configuration
      - 📁 model
      - 📁 pipeline
      - 📁 services
    - 📁 hashing.bcrypt
      - 📁 services
    - 📁 persistence.jpa.repositories
    - 📁 tokens.jwt
      - 📁 services
  - 📁 interfaces
    - 📁 acl
    - 📁 rest
      - 📁 resources
      - 📁 transform
```

### Package domain . model . valueobjects

En el package `valueobjects` crear el record `Roles` con el siguiente contenido:
```java
public enum Roles {
  ROLE_USER,
  ROLE_ADMIN,
  ROLE_INSTRUCTOR
}
```

### Package domain . model . entities

En el package `entities` crear la clase `Role` con el siguiente contenido:
```java
import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.With;
import pe.edu.upc.center.platform.iam.domain.model.valueobjects.Roles;

import java.util.List;

/**
 * Role entity
 * <p>
 *     This entity represents the role of a user in the system.
 *     It is used to define the permissions of a user.
 * </p>
 */
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@With
public class Role {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Enumerated(EnumType.STRING)
  @Column(length = 20)
  private Roles name;

  public Role(Roles name) {
    this.name = name;
  }

  /**
   * Get the name of the role as a string
   * @return the name of the role as a string
   */
  public String getStringName() {
    return name.name();
  }

  /**
   * Get the default role
   * @return the default role
   */
  public static Role getDefaultRole() {
    return new Role(Roles.ROLE_USER);
  }

  /**
   * Get the role from its name
   * @param name the name of the role
   * @return the role
   */
  public static Role toRoleFromName(String name) {
    return new Role(Roles.valueOf(name));
  }

  /**
   * Validate the role set
   * <p>
   *     This method validates the role set and returns the default role if the set is empty.
   * </p>
   * @param roles the role set
   * @return the role set
   */
  public static List<Role> validateRoleSet(List<Role> roles) {
    if (roles == null || roles.isEmpty()) {
      return List.of(getDefaultRole());
    }
    return roles;
  }
}
```

### Package domain . model . aggregates

En el package `aggregates` crear la clase `User` con el siguiente contenido:
```java
import jakarta.persistence.*;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;
import lombok.Getter;
import lombok.Setter;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

/**
 * User aggregate root
 * This class represents the aggregate root for the User entity.
 *
 * @see AuditableAbstractAggregateRoot
 */
@Getter
@Setter
@Entity
public class User extends AuditableAbstractAggregateRoot<User> {

  @NotBlank
  @Size(max = 50)
  @Column(unique = true)
  private String username;

  @NotBlank
  @Size(max = 120)
  private String password;

  @ManyToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
  @JoinTable(	name = "user_roles",
      joinColumns = @JoinColumn(name = "user_id"),
      inverseJoinColumns = @JoinColumn(name = "role_id"))
  private Set<Role> roles;

  public User() {
    this.roles = new HashSet<>();
  }
  public User(String username, String password) {
    this.username = username;
    this.password = password;
    this.roles = new HashSet<>();
  }

  public User(String username, String password, List<Role> roles) {
    this(username, password);
    addRoles(roles);
  }

  /**
   * Add a role to the user
   * @param role the role to add
   * @return the user with the added role
   */
  public User addRole(Role role) {
    this.roles.add(role);
    return this;
  }

  /**
   * Add a list of roles to the user
   * @param roles the list of roles to add
   * @return the user with the added roles
   */
  public User addRoles(List<Role> roles) {
    var validatedRoleSet = Role.validateRoleSet(roles);
    this.roles.addAll(validatedRoleSet);
    return this;
  }
}
```
### Package domain . model . queries

En el package `queries` crear el record `GetAllRolesQuery` con el siguiente contenido:
```java
public record GetAllRolesQuery() {
}
```

En el package `queries` crear el record `GetAllUsersQuery` con el siguiente contenido:
```java
public record GetAllUsersQuery() {
}
```

En el package `queries` crear el record `GetRoleByNameQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.valueobjects.Roles;

public record GetRoleByNameQuery(Roles name) {
}
```

En el package `queries` crear el record `GetUserByIdQuery` con el siguiente contenido:
```java
public record GetUserByIdQuery(Long userId) {
}
```

En el package `queries` crear el record `GetUserByUsernameQuery` con el siguiente contenido:
```java
public record GetUserByUsernameQuery(String username) {
}
```

### Package domain . model . commands

En el package `commands` crear el record `SeedRolesCommand` con el siguiente contenido:
```java
public record SeedRolesCommand() {
}
```

En el package `commands` crear el record `SignInCommand` con el siguiente contenido:
```java
public record SignInCommand(String username, String password) {
}
```

En el package `commands` crear el record `SignUpCommand` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;

import java.util.List;

public record SignUpCommand(String username, String password, List<Role> roles) {
}
```

### Package domain . services

En el package `services` crear el interface `RoleCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.commands.SeedRolesCommand;

public interface RoleCommandService {
  void handle(SeedRolesCommand command);
}
```

En el package `services` crear el interface `RoleQueryService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllRolesQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetRoleByNameQuery;

import java.util.List;
import java.util.Optional;

public interface RoleQueryService {
  List<Role> handle(GetAllRolesQuery query);
  Optional<Role> handle(GetRoleByNameQuery query);
}
```

En el package `services` crear el interface `UserCommandService` con el siguiente contenido:
```java
import org.apache.commons.lang3.tuple.ImmutablePair;
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignInCommand;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignUpCommand;

import java.util.Optional;

public interface UserCommandService {
  Optional<ImmutablePair<User, String>> handle(SignInCommand command);
  Optional<User> handle(SignUpCommand command);
}
```

En el package `services` crear el interface `UserQueryService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllUsersQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByIdQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByUsernameQuery;

import java.util.List;
import java.util.Optional;

public interface UserQueryService {
  List<User> handle(GetAllUsersQuery query);
  Optional<User> handle(GetUserByIdQuery query);
  Optional<User> handle(GetUserByUsernameQuery query);
}
```

# Learning Center Platform Mini 

## Table of contents



## Creación del project

Cargar el navegador y generar un proyecto Spring a partir del siguiente enlace:

https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.3.4&packaging=jar&jvmVersion=21&groupId=pe.edu.upc.center&artifactId=learning-center-platform-mini&name=learning-center-platform-mini&description=Demo%20project%20for%20Spring%20Boot&packageName=pe.edu.upc.center.platform&dependencies=data-jpa,validation,web,devtools,postgresql,lombok

Generar el project Spring, Guardarlo en la carpeta `IdeaProjects` y luego abrirlo el `IntelliJ IDEA`.

## Configuración inicial

### Base de datos

Abrir pgAdmin y crear la base de datos: `learningmini`.

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
            <version>2.6.0</version>
        </dependency>
```

### LearningCenterPlatformApplication

Abrir el archivo `LearningCenterPlatformMiniApplication.java` ubicado en el package `pe.edu.upc.learning.platform` y agregar la anotación `@EnableJpaAuditing` debajo de la anotación `@SpringBootApplication`.

La clase debe quedar:
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@SpringBootApplication
@EnableJpaAuditing
public class LearningCenterPlatformMiniApplication {

	public static void main(String[] args) {
		SpringApplication.run(LearningCenterPlatformApplication.class, args);
	}
}
```

### application.properties

Abrir el archivo `application.properties` y reemplazar con el siguiente código:
```ini
# Spring Application Name
spring.application.name=learning-center-platform-mini

# Spring DataSource Configuration
###    JDBC : SGDB :// HOST : PORT / DB
spring.datasource.url: jdbc:postgresql://localhost:5432/learningmini
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
      - 📄 OpenApiConfiguration.java
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

### Package infrastructure documentation.openapi.configuration

En el package `configuration` crear la clase `OpenApiConfiguration` con el siguiente contenido:
```java
import io.swagger.v3.oas.models.ExternalDocumentation;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
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
                    .description("Learning Platform Documentation")
                    .url("https://github.com/upc-is-si729/daos-language-reference"));
    return openApi;
  }
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

## Profile Bounded Context

### Project structur for Profile Bounded Context

Crear la siguiente estructura para el Bounded Context `profiles`:

```markdown
- 📁 profiles
  - 📁 application.internal
    - 📁 commandservices
    - 📁 queryservices
  - 📁 domain
    - 📁 exceptions
    - 📁 model
      - 📁 aggregates
      - 📁 commands      
      - 📁 queries
      - 📁 valueobjects
    - 📁 services
  - 📁 infrastructure.persistence.jpa.repositories
  - 📁 interfaces
    - 📁 acl
    - 📁 rest
      - 📁 resources
      - 📁 transform
```

### Package domain . model . valueobjects

En el package `valueobjects` crear el record `StreetAddress` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

@Embeddable
public record StreetAddress(String street) {
  public StreetAddress() {
    this(null);
  }
  public StreetAddress {
    if (street == null || street.isBlank()) {
      throw new IllegalArgumentException("Address cannot be null or blank");
    }
  }
}
```


### Package domain . model . aggregates

En el package `aggregates` crear la clase `Profile` con el siguiente contenido:
```java
import jakarta.persistence.*;
import jakarta.validation.constraints.Max;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import lombok.Getter;
//import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.StreetAddress;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

@Entity
@Table(name = "profiles")
public class Profile extends AuditableAbstractAggregateRoot<Profile> {

  @Getter
  @NotNull
  @NotBlank
  @Column(name = "full_name", length = 50, nullable = false)
  private String fullName;

  @Getter
  @Min(0)
  @Max(100)
  @Column(name = "age", columnDefinition = "smallint", nullable = false)
  private int age;

  @Embedded
  @AttributeOverrides( {
      @AttributeOverride(name = "street", column = @Column(name = "address_street", length = 100, nullable = false))
  })
  private StreetAddress address;

  //---------------------------------------------------
  public Profile(String fullName, int age, String street) {
    this.fullName = fullName;
    this.age = age;
    this.address = new StreetAddress(street);
  }

  public Profile() {
  }

  public void updateStreetAddress(String street) {
    this.address = new StreetAddress(street);
  }

  public String getAddress() {
    return address.street();
  }
  //---------------------------------------------------
  /*public Profile(CreateProfileCommand command) {
    this.fullName = command.fullName();
    this.age = command.age();
    this.address = new StreetAddress(command.street());
  }

  public Profile updateInformation(String fullName, int age, String street) {
    this.fullName = fullName;
    this.age = age;
    this.address = new StreetAddress(street);
    return this;
  }*/
}
```

## Learning Bounded Context

### Project structur for Learning Bounded Context

Crear la siguiente estructura para el Bounded Context `learning`:

```markdown
- 📁 learning
  - 📁 application.internal
    - 📁 commandservices
    - 📁 eventhandlers
    - 📁 outboundservices.acl
    - 📁 queryservices
  - 📁 domain
    - 📁 exceptions
    - 📁 model
      - 📁 aggregates
      - 📁 commands
      - 📁 entities
      - 📁 events
      - 📁 queries
      - 📁 valueobjects
    - 📁 services
  - 📁 infrastructure.persistence.jpa.repositories
  - 📁 interfaces.rest
    - 📁 resources
    - 📁 transform
```

### Package domain . model . valueobjects

En el package `valueobjects` crear el record `ProfileId` con el siguiente contenido:
```java
public record ProfileId(Long profileId) {
    public ProfileId {
      if (profileId < 0) {
        throw new IllegalArgumentException("Profile profileId cannot be negative");
      }
    }

    public ProfileId() {
      this(0L);
    }
}
```

En el package `valueobjects` crear el record `StudentCode` con el siguiente contenido:
```java
import java.util.UUID;

public record StudentCode(String studentCode) {
  public StudentCode() {
    this(UUID.randomUUID().toString());
  }
  public StudentCode {
    if (studentCode == null || studentCode.isBlank()) {
      throw new IllegalArgumentException("Student code cannot be null or blank");
    }
    if (studentCode.length() != 36) {
      throw new IllegalArgumentException("Student code must be 36 characters long");
    }
    if (!studentCode.matches("[a-f0-9]{8}-([a-f0-9]{4}-){3}[a-f0-9]{12}")) {
      throw new IllegalArgumentException("Student code must be a valid UUID");
    }
  }
}
```

### Package domain . model . aggregates

En el package `aggregates` modificar la clase `Student` con el siguiente contenido:
```java
import jakarta.persistence.*;
import lombok.Getter;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

@Entity
@Table(name = "students")
public class Student extends AuditableAbstractAggregateRoot<Student> {

  @Getter
  @Embedded
  @AttributeOverrides( {
      @AttributeOverride(name = "studentCode", column = @Column(name = "code", length = 36, nullable = false))
  })
  private final StudentCode studentCode;

  @Embedded
  @AttributeOverrides( {
      @AttributeOverride(name = "profileId", column = @Column(name = "profile_id", nullable = false))
  })
  private ProfileId profileId;

  //---------------------------------------------------
  public Student() {
    this.studentCode = new StudentCode();
  }

  public Student(Long profileId) {
    this();
    this.profileId = new ProfileId(profileId);
  }

  public Student(ProfileId profileId) {
    this();
    this.profileId = profileId;
  }

  public Long getProfileId() {
    return profileId.profileId();
  }
}
```

## Generación de las Tablas en la Base de datos

### 

Ejecute el proyecto y Verifique que se ha generado las tablas en la base de datos: `learningmini`.

## Profile Bounded Context

### Package domain . model . queries

En el package `queries` crear el record `GetAllProfilesQuery` con el siguiente contenido:
```java
public record GetAllProfilesQuery() {
}
```

En el package `queries` crear el record `GetProfileByFullNameQuery` con el siguiente contenido:
```java
public record GetProfileByFullNameQuery(String fullName) {
}
```

En el package `queries` crear el record `GetProfileByIdQuery` con el siguiente contenido:
```java
public record GetProfileByIdQuery(Long profileId) {
}
```

### Package domain . model . commands

En el package `commands` crear el record `CreateProfileCommand` con el siguiente contenido:
```java
public record CreateProfileCommand(String fullName, int age, String street) {
}
```

En el package `commands` crear el record `UpdateProfileCommand` con el siguiente contenido:
```java
public record UpdateProfileCommand(Long profileId, String fullName, int age, String street) {
}
```

En el package `commands` crear el record `DeleteProfileCommand` con el siguiente contenido:
```java
public record DeleteProfileCommand(Long profileId) {
}
```


### Package domain . services

En el package `services` crear el interface `ProfileCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.DeleteProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.UpdateProfileCommand;

import java.util.Optional;

public interface ProfileCommandService {
  Long handle(CreateProfileCommand command);
  Optional<Profile> handle(UpdateProfileCommand command);
  void handle(DeleteProfileCommand command);
}
```

En el package `services` crear el interface `ProfileQueryService` con el siguiente contenido:

```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByFullNameQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;

import java.util.List;
import java.util.Optional;

public interface ProfileQueryService {
  List<Profile> handle(GetAllProfilesQuery query);
  Optional<Profile> handle(GetProfileByIdQuery query);
  Optional<Profile> handle(GetProfileByFullNameQuery query);
}
```

### Package domain . model . aggregates

En el package `aggregates` en la clase `Profile` quite el comentario de las siguientes lineas de código:

En los imports:
```java
//import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
```

Al final del archivo:
```java
  //---------------------------------------------------
  /*public Profile(CreateProfileCommand command) {
    this.fullName = command.fullName();
    this.age = command.age();
    this.address = new StreetAddress(command.street());
  }

  public Profile updateInformation(String fullName, int age, String street) {
    this.fullName = fullName;
    this.age = age;
    this.address = new StreetAddress(street);
    return this;
  }*/
}
```

### Package infrastructure.persistence.jpa.repositories

En el package `repositories` crear el interface `ProfileRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;

import java.util.Optional;

@Repository
public interface ProfileRepository extends JpaRepository<Profile, Long> {
  boolean existsByFullName(String fullName);
  boolean existsByFullNameAndIdIsNot(String fullName, Long id);
  Optional<Profile> findByFullName(String fullName);
}
```

### Package application.internal queryservices

En el package `queryservices` crear la clase `ProfileQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByFullNameQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileQueryService;
import pe.edu.upc.center.platform.profiles.infrastructure.persistence.jpa.repositories.ProfileRepository;

import java.util.List;
import java.util.Optional;

@Service
public class ProfileQueryServiceImpl implements ProfileQueryService {

  private final ProfileRepository profileRepository;

  public ProfileQueryServiceImpl(ProfileRepository profileRepository) {
    this.profileRepository = profileRepository;
  }

  @Override
  public List<Profile> handle(GetAllProfilesQuery query) {
    return this.profileRepository.findAll();
  }

  @Override
  public Optional<Profile> handle(GetProfileByIdQuery query) {
    return this.profileRepository.findById(query.profileId());
  }

  @Override
  public Optional<Profile> handle(GetProfileByFullNameQuery query) {
    return this.profileRepository.findByFullName(query.fullName());
  }
}
```

### Package application.internal commandservices

En el package `commandservices` crear la clase `ProfileCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.DeleteProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.UpdateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileCommandService;
import pe.edu.upc.center.platform.profiles.infrastructure.persistence.jpa.repositories.ProfileRepository;

import java.util.Optional;

@Service
public class ProfileCommandServiceImpl implements ProfileCommandService {

  private final ProfileRepository profileRepository;

  public ProfileCommandServiceImpl(ProfileRepository profileRepository) {
    this.profileRepository = profileRepository;
  }

  @Override
  public Long handle(CreateProfileCommand command) {
    var fullName = command.fullName();
    if (this.profileRepository.existsByFullName(fullName)) {
      throw new IllegalArgumentException("Profile with full name " + fullName + " already exists");
    }
    var profile = new Profile(command);
    try {
      this.profileRepository.save(profile);
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while saving profile: " + e.getMessage());
    }
    return profile.getId();
  }

  @Override
  public Optional<Profile> handle(UpdateProfileCommand command) {
    var profileId = command.profileId();
    var fullName = command.fullName();
    if (this.profileRepository.existsByFullNameAndIdIsNot(fullName, profileId)) {
      throw new IllegalArgumentException("Profile with full name " + fullName + " already exists");
    }

    // If the profile does not exist, throw an exception
    if (!this.profileRepository.existsById(profileId)) {
      throw new IllegalArgumentException("Profile with id " + profileId + " does not exist");
    }

    var profileToUpdate = this.profileRepository.findById(profileId).get();
    profileToUpdate.updateInformation(command.fullName(), command.age(), command.street());

    try {
      var updatedProfile = this.profileRepository.save(profileToUpdate);
      return Optional.of(updatedProfile);
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while updating profile: " + e.getMessage());
    }
  }

  @Override
  public void handle(DeleteProfileCommand command) {
    // If the profile does not exist, throw an exception
    if (!this.profileRepository.existsById(command.profileId())) {
      throw new IllegalArgumentException("Profile with id " + command.profileId() + " does not exist");
    }

    // Try to delete the profile, if an error occurs, throw an exception
    try {
      this.profileRepository.deleteById(command.profileId());
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while deleting profile: " + e.getMessage());
    }
  }
}
```

### Package interfaces rest resources

En el package `resources` crear el record `CreateProfileResource` con el siguiente contenido:
```java
public record CreateProfileResource(String fullName, int age, String street) {
}
```

En el package `resources` crear el record `ProfileResource` con el siguiente contenido:
```java
public record ProfileResource(Long id, String fullName, int age, String street) {
}
```

### Package interfaces rest transform

En el package `transform` crear la clase `CreateProfileCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.CreateProfileResource;

public class CreateProfileCommandFromResourceAssembler {
  public static CreateProfileCommand toCommandFromResource(CreateProfileResource resource) {
    return new CreateProfileCommand(resource.fullName(), resource.age(), resource.street());
  }
}
```

En el package `transform` crear el record `ProfileResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;

public class ProfileResourceFromEntityAssembler {
  public static ProfileResource toResourceFromEntity(Profile entity) {
    return new ProfileResource(entity.getId(), entity.getFullName(), entity.getAge(), entity.getAddress());
  }
}
```

En el package `transform` crear el record `UpdateProfileCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.commands.UpdateProfileCommand;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;

public class UpdateProfileCommandFromResourceAssembler {
  public static UpdateProfileCommand toCommandFromResource(Long profileId, ProfileResource resource) {
    return new UpdateProfileCommand(profileId, resource.fullName(), resource.age(), resource.street());
  }
}
```

### Package interfaces rest

En el package `rest` crear la clase `ProfilesController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.center.platform.profiles.domain.model.commands.DeleteProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileCommandService;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileQueryService;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.CreateProfileResource;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.CreateProfileCommandFromResourceAssembler;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.ProfileResourceFromEntityAssembler;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.UpdateProfileCommandFromResourceAssembler;

import java.util.List;
import java.util.stream.Collectors;

@CrossOrigin(origins = "*", methods = { RequestMethod.POST, RequestMethod.GET, RequestMethod.PUT, RequestMethod.DELETE })
@RestController
@RequestMapping(value = "/api/v1/profiles", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Profiles", description = "Profile Management Endpoints")
public class ProfilesController {

  private final ProfileQueryService profileQueryService;
  private final ProfileCommandService profileCommandService;

  public ProfilesController(ProfileQueryService profileQueryService, ProfileCommandService profileCommandService) {
    this.profileQueryService = profileQueryService;
    this.profileCommandService = profileCommandService;
  }

  @PostMapping
  public ResponseEntity<ProfileResource> createProfile( @RequestBody CreateProfileResource resource) {

    var createProfileCommand = CreateProfileCommandFromResourceAssembler
        .toCommandFromResource(resource);
    var profileId = this.profileCommandService.handle(createProfileCommand);

    if (profileId.equals(0L)) {
      return ResponseEntity.badRequest().build();
    }

    var getProfileByIdQuery = new GetProfileByIdQuery(profileId);
    var optionalProfile = this.profileQueryService.handle(getProfileByIdQuery);

    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(optionalProfile.get());
    return new ResponseEntity<>(profileResource, HttpStatus.CREATED);
  }

  @GetMapping
  public ResponseEntity<List<ProfileResource>> getAllProfiles() {
    var getAllProfilesQuery = new GetAllProfilesQuery();
    var profiles = this.profileQueryService.handle(getAllProfilesQuery);
    var profileResources = profiles.stream()
        .map(ProfileResourceFromEntityAssembler::toResourceFromEntity)
        .collect(Collectors.toList());
    return ResponseEntity.ok(profileResources);
  }

  @GetMapping("/{profileId}")
  public ResponseEntity<ProfileResource> getProfileById(@PathVariable Long profileId) {
    var getProfileByIdQuery = new GetProfileByIdQuery(profileId);
    var optionalProfile = this.profileQueryService.handle(getProfileByIdQuery);
    if (optionalProfile.isEmpty())
      return ResponseEntity.badRequest().build();
    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(optionalProfile.get());
    return ResponseEntity.ok(profileResource);
  }

  @PutMapping("/{profileId}")
  public ResponseEntity<ProfileResource> updateProfile(@PathVariable Long profileId, @RequestBody ProfileResource resource) {
    var updateProfileCommand = UpdateProfileCommandFromResourceAssembler.toCommandFromResource(profileId, resource);
    var optionalProfile = this.profileCommandService.handle(updateProfileCommand);

    if (optionalProfile.isEmpty())
      return ResponseEntity.badRequest().build();
    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(optionalProfile.get());
    return ResponseEntity.ok(profileResource);
  }

  @DeleteMapping("/{profileId}")
  public ResponseEntity<?> deleteProfile(@PathVariable Long profileId) {
    var deleteProfileCommand = new DeleteProfileCommand(profileId);
    this.profileCommandService.handle(deleteProfileCommand);
    return ResponseEntity.noContent().build();
  }
}
```

### Testing RESTful Api (Swagger UI)

Cargar el siguiente enlace: http://localhost:8090/swagger-ui/index.html 

### Package interfaces acl

En el package `acl` crear la clase `ProfilesContextFacade` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.DeleteProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.commands.UpdateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByFullNameQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileCommandService;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileQueryService;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.ProfileResourceFromEntityAssembler;

import java.util.Optional;

@Service
public class ProfilesContextFacade {
  private final ProfileCommandService profileCommandService;
  private final ProfileQueryService profileQueryService;

  public ProfilesContextFacade(ProfileCommandService profileCommandService, ProfileQueryService profileQueryService) {
    this.profileCommandService = profileCommandService;
    this.profileQueryService = profileQueryService;
  }

  public Optional<ProfileResource> fetchProfileById(Long profileId) {
    var getProfileByIdQuery = new GetProfileByIdQuery(profileId);
    var optionalProfile = profileQueryService.handle(getProfileByIdQuery);
    if (optionalProfile.isEmpty()) {
      return Optional.empty();
    }
    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(optionalProfile.get());
    return Optional.of(profileResource);
  }

  public Long fetchProfileIdByFullName(String fullName) {
    var getProfileByFullNameQuery = new GetProfileByFullNameQuery(fullName);
    var optionalProfile = profileQueryService.handle(getProfileByFullNameQuery);
    if (optionalProfile.isEmpty()) {
      return 0L;
    }
    return optionalProfile.get().getId();
  }

  public boolean existsProfileByFullNameAndIdIsNot(String fullName, Long id) {
    var getProfileByFullNameQuery = new GetProfileByFullNameQuery(fullName);
    var optionalProfile = profileQueryService.handle(getProfileByFullNameQuery);
    if (optionalProfile.isEmpty()) {
      return false;
    }
    return optionalProfile.get().getId() != id;
  }

  public Long createProfile(String fullName, int age, String street) {
    var CreateProfileCommand = new CreateProfileCommand(fullName, age, street);
    var profileId = profileCommandService.handle(CreateProfileCommand);
    if (profileId.equals(null)) {
      return 0L;
    }
    return profileId;
  }

  public Long updateProfile(Long profileId, String fullName, int age, String street) {
    var updateProfileCommand = new UpdateProfileCommand(profileId, fullName, age, street);
    var optionalProfile = profileCommandService.handle(updateProfileCommand);
    if (optionalProfile.isEmpty()) {
      return 0L;
    }
    return optionalProfile.get().getId();
  }

  public void deleteProfile(Long profileId) {
    var deleteProfileCommand = new DeleteProfileCommand(profileId);
    profileCommandService.handle(deleteProfileCommand);
  }
}
```




## Learning Bounded Context

### Package domain . model . queries

En el package `queries` crear el record `GetAllStudentsQuery` con el siguiente contenido:
```java
public record GetAllStudentsQuery() {
}
```

En el package `queries` crear el record `GetStudentByIdQuery` con el siguiente contenido:
```java
public record GetStudentByIdQuery(Long studentId) {
}
```

En el package `queries` crear el record `GetStudentByProfileIdQuery` con el siguiente contenido:
```java
public record GetStudentByProfileIdQuery(ProfileId profileId) {
}
```

En el package `queries` crear el record `GetStudentByStudentCodeQuery` con el siguiente contenido:
```java
public record GetStudentByStudentCodeQuery(StudentCode studentCode) {
}
```


### Package domain . model . commands

En el package `commands` crear el record `CreateStudentCommand` con el siguiente contenido:
```java
public record CreateStudentCommand(String fullName, int age, String street) {
}
```

En el package `commands` crear el record `UpdateStudentCommand` con el siguiente contenido:
```java
public record UpdateStudentCommand(StudentCode studentCode, String fullName, int age, String street) {
}
```

En el package `commands` crear el record `DeleteStudentCommand` con el siguiente contenido:
```java
public record DeleteStudentCommand(StudentCode studentCode) {
}
```


### Package domain . services

En el package `services` crear el interface `StudentCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;

import java.util.Optional;

public interface StudentCommandService {
  StudentCode handle(CreateStudentCommand command);
  Optional<Student> handle(UpdateStudentCommand command);
  void handle(DeleteStudentCommand command);
}
```

En el package `services` crear el interface `StudentQueryService` con el siguiente contenido:

```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllStudentsQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByStudentCodeQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByProfileIdQuery;

import java.util.List;
import java.util.Optional;

public interface StudentQueryService {
  List<Student> handle(GetAllStudentsQuery query);
  Optional<Student> handle(GetStudentByIdQuery query);
  Optional<Student> handle(GetStudentByStudentCodeQuery query);
  Optional<Student> handle(GetStudentByProfileIdQuery query);
}
```

### Package infrastructure.persistence.jpa.repositories

En el package `repositories` crear el interface `StudentRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;

import java.util.Optional;

@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
  Optional<Student> findByStudentCode(StudentCode studentCode);
  Optional<Student> findByProfileId(ProfileId profileId);
  boolean existsByStudentCode(StudentCode studentCode);
}
```

### Package application.internal queryservices

En el package `queryservices` crear la clase `StudentQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllStudentsQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByProfileIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByStudentCodeQuery;
import pe.edu.upc.center.platform.learning.domain.services.StudentQueryService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.StudentRepository;

import java.util.List;
import java.util.Optional;

@Service
public class StudentQueryServiceImpl implements StudentQueryService {

  private final StudentRepository studentRepository;

  public StudentQueryServiceImpl(StudentRepository studentRepository) {
    this.studentRepository = studentRepository;
  }

  @Override
  public List<Student> handle(GetAllStudentsQuery query) {
    return this.studentRepository.findAll();
  }

  @Override
  public Optional<Student> handle(GetStudentByIdQuery query) {
    return this.studentRepository.findById(query.studentId());
  }

  @Override
  public Optional<Student> handle(GetStudentByStudentCodeQuery query) {
    return this.studentRepository.findByStudentCode(query.studentCode());
  }

  @Override
  public Optional<Student> handle(GetStudentByProfileIdQuery query) {
    return this.studentRepository.findByProfileId(query.profileId());
  }
}
```

### Package application.internal outboundservices

En el package `acl` crear la clase `ExternalProfileService` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.StudentResource;
import pe.edu.upc.center.platform.profiles.interfaces.acl.ProfilesContextFacade;

import java.util.Optional;

@Service
public class ExternalProfileService {

  private final ProfilesContextFacade profilesContextFacade;

  public ExternalProfileService(ProfilesContextFacade profilesContextFacade) {
    this.profilesContextFacade = profilesContextFacade;
  }

  public Optional<ProfileId> fetchProfileIdByFullName(String fullName) {
    var profileId = profilesContextFacade.fetchProfileIdByFullName(fullName);
    if (profileId.equals(0L))
      return Optional.empty();
    return Optional.of(new ProfileId(profileId));
  }

  public boolean existsProfileByFullNameAndIdIsNot(String fullName, long id) {
    return profilesContextFacade.existsProfileByFullNameAndIdIsNot(fullName, id);
  }

  public Optional<ProfileId> createProfile(String fullName, int age, String street) {
    var profileId = profilesContextFacade.createProfile(fullName, age, street);
    if (profileId.equals(0L))
      return Optional.empty();
    return Optional.of(new ProfileId(profileId));
  }

  public Optional<ProfileId> updateProfile(Long profileId, String fullName, int age, String street) {
    var profileIdUpdated = profilesContextFacade.updateProfile(profileId, fullName, age, street);
    if (profileIdUpdated.equals(0L))
      return Optional.empty();
    return Optional.of(new ProfileId(profileIdUpdated));
  }

  public void deleteProfile(Long profileId) {
    profilesContextFacade.deleteProfile(profileId);
  }

  public Optional<StudentResource> fetchStudentResourceFromProfileId(Student student) {
    var profileResource = profilesContextFacade.fetchProfileById(student.getProfileId());
    if (profileResource.isEmpty())
      return Optional.empty();

    var studentResource = new StudentResource(student.getStudentCode().studentCode(), profileResource.get().fullName(),
        profileResource.get().age(), profileResource.get().street());
    return Optional.of(studentResource);
  }
}
```


### Package application.internal commandservices

En el package `commandservices` crear la clase `StudentCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.application.internal.outboundservices.acl.ExternalProfileService;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.StudentRepository;

import java.util.Optional;

@Service
public class StudentCommandServiceImpl implements StudentCommandService {

  private final StudentRepository studentRepository;
  private final ExternalProfileService externalProfileService;

  public StudentCommandServiceImpl(StudentRepository studentRepository, ExternalProfileService externalProfileService) {
    this.studentRepository = studentRepository;
    this.externalProfileService = externalProfileService;
  }

  @Override
  public StudentCode handle(CreateStudentCommand command) {
    var profileId = this.externalProfileService.fetchProfileIdByFullName(command.fullName());
    if (profileId.isPresent()) {
      this.studentRepository.findByProfileId(profileId.get()).ifPresent(student -> {
        throw new IllegalArgumentException("Student already exists");
      });
    }

    profileId = this.externalProfileService.createProfile(command.fullName(), command.age(), command.street());
    if (profileId.isEmpty()) {
      throw new IllegalArgumentException("Unable to create profile");
    }

    var student = new Student(profileId.get());
    this.studentRepository.save(student);
    return student.getStudentCode();
  }

  @Override
  public Optional<Student> handle(UpdateStudentCommand command) {
    var optionalStudent = this.studentRepository.findByStudentCode(command.studentCode());
    if (optionalStudent.isEmpty()) {
      throw new IllegalArgumentException("Student not found");
    }

    if (this.externalProfileService.existsProfileByFullNameAndIdIsNot(command.fullName(), optionalStudent.get().getProfileId())) {
      throw new IllegalArgumentException("Student with name " + command.fullName() + " already exists");
    }

    var profileId = this.externalProfileService.updateProfile(optionalStudent.get().getProfileId(), command.fullName(), command.age(), command.street());

    if (profileId.isEmpty()) {
      throw new IllegalArgumentException("Unable to update profile");
    }

    // Update student
    // this.studentRepository.save(optionalStudent.get());
    return optionalStudent;
  }

  @Override
  public void handle(DeleteStudentCommand command) {
    if (!this.studentRepository.existsByStudentCode(command.studentCode())) {
      throw new IllegalArgumentException("Student not found");
    }
    var optionalStudent = this.studentRepository.findByStudentCode(command.studentCode());
    this.externalProfileService.deleteProfile(optionalStudent.get().getProfileId());
    this.studentRepository.deleteById(optionalStudent.get().getId());
  }
}
```

### Package interfaces rest resources

En el package `resources` crear el record `CreateStudentResource` con el siguiente contenido:
```java
public record CreateStudentResource(String name, int age, String address) {
}
```

En el package `resources` crear el record `StudentResource` con el siguiente contenido:
```java
public record StudentResource(String id, String name, int age, String address) {
}
```

En el package `resources` crear el record `ProfileResource` con el siguiente contenido:
```java
public record ProfileResource(Long id, String fullName, int age, String street) {
}
```


### Package interfaces rest transform

En el package `transform` crear la clase `CreateStudentCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateStudentResource;

public class CreateStudentCommandFromResourceAssembler {
  public static CreateStudentCommand toCommandFromResource(CreateStudentResource resource) {
    return new CreateStudentCommand(resource.name(), resource.age(), resource.address());
  }
}
```

En el package `transform` crear el record `UpdateStudentCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.StudentResource;

public class UpdateStudentCommandFromResourceAssembler {
  public static UpdateStudentCommand toCommandFromResource(String studentCode, StudentResource resource) {
    return new UpdateStudentCommand(new StudentCode(studentCode), resource.name(), resource.age(), resource.address());
  }
}
```


### Package interfaces rest

En el package `rest` crear la clase `StudentsController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.center.platform.learning.application.internal.outboundservices.acl.ExternalProfileService;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllStudentsQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByStudentCodeQuery;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;
import pe.edu.upc.center.platform.learning.domain.services.StudentQueryService;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentCode;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateStudentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.StudentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.CreateStudentCommandFromResourceAssembler;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.UpdateStudentCommandFromResourceAssembler;

import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

@CrossOrigin(origins = "*", methods = { RequestMethod.POST, RequestMethod.GET, RequestMethod.PUT, RequestMethod.DELETE })
@RestController
@RequestMapping(value = "/api/v1/students", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Students", description = "Student Management Endpoints")
public class StudentsController {

  private final StudentCommandService studentCommandService;
  private final StudentQueryService studentQueryService;
  private final ExternalProfileService externalProfileService;

  public StudentsController(StudentCommandService studentCommandService, StudentQueryService studentQueryService,
      ExternalProfileService externalProfileService) {
    this.studentCommandService = studentCommandService;
    this.studentQueryService = studentQueryService;
    this.externalProfileService = externalProfileService;
  }

  @PostMapping
  public ResponseEntity<StudentResource> createStudent(@RequestBody CreateStudentResource resource) {
    var createStudentCommand = CreateStudentCommandFromResourceAssembler.toCommandFromResource(resource);
    var studentCode = this.studentCommandService.handle(createStudentCommand);

    if (studentCode.studentCode().isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var getStudentByStudentCodeQuery = new GetStudentByStudentCodeQuery(studentCode);
    var student = this.studentQueryService.handle(getStudentByStudentCodeQuery);

    if (student.isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var studentResource = this.externalProfileService.fetchStudentResourceFromProfileId(student.get()).get();
    return new ResponseEntity<>(studentResource, HttpStatus.CREATED);

  }

  @GetMapping
  public ResponseEntity<List<StudentResource>> getAllStudents() {
    var getAllStudentsQuery = new GetAllStudentsQuery();
    var students = this.studentQueryService.handle(getAllStudentsQuery);
    var profileResources = students.stream()
        .map(this.externalProfileService::fetchStudentResourceFromProfileId)
        .map(Optional::get)
        .collect(Collectors.toList());
    return ResponseEntity.ok(profileResources);
  }

  @GetMapping("/{studentCode}")
  public ResponseEntity<StudentResource> getStudentById(@PathVariable String studentCode) {
    var getStudentByStudentCodeQuery = new GetStudentByStudentCodeQuery(new StudentCode(studentCode));
    var optionalStudent = this.studentQueryService.handle(getStudentByStudentCodeQuery);
    if (optionalStudent.isEmpty())
      return ResponseEntity.badRequest().build();
    var studentResource = this.externalProfileService.fetchStudentResourceFromProfileId(optionalStudent.get()).get();
    return ResponseEntity.ok(studentResource);
  }

  @PutMapping("/{studentCode}")
  public ResponseEntity<StudentResource> updateStudent(@PathVariable String studentCode, @RequestBody StudentResource resource) {
    var updateProfileCommand = UpdateStudentCommandFromResourceAssembler.toCommandFromResource(studentCode, resource);
    var optionalStudent = this.studentCommandService.handle(updateProfileCommand);

    if (optionalStudent.isEmpty())
      return ResponseEntity.badRequest().build();
    var studentResource = this.externalProfileService.fetchStudentResourceFromProfileId(optionalStudent.get()).get();
    return ResponseEntity.ok(studentResource);
  }

  @DeleteMapping("/{studentCode}")
  public ResponseEntity<?> deleteProfile(@PathVariable String studentCode) {
    var deleteStudentCommand = new DeleteStudentCommand(new StudentCode(studentCode));
    this.studentCommandService.handle(deleteStudentCommand);
    return ResponseEntity.noContent().build();
  }
}
```

### Testing RESTful Api (Swagger UI)

Cargar el siguiente enlace: http://localhost:8090/swagger-ui/index.html 

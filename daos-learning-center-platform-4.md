# Learning Center Platform - Part 4

## Table of contents

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

En el package `valueobjects` crear el record `PersonName` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

@Embeddable
public record PersonName(String firstName, String lastName) {
  public PersonName() {
    this(null, null);
  }

  public PersonName {
    if (firstName == null || firstName.isBlank()) {
      throw new IllegalArgumentException("First name cannot be null or blank");
    }
    if (lastName == null || lastName.isBlank()) {
      throw new IllegalArgumentException("Last name cannot be null or blank");
    }
  }
  public String getFullName() {
    return String.format("%s %s", firstName, lastName);
  }
}
```

En el package `valueobjects` crear el record `EmailAddress` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;
import jakarta.validation.constraints.Email;

@Embeddable
public record EmailAddress(@Email String address) {
  public EmailAddress() {
    this(null);
  }
}
```

En el package `valueobjects` crear el record `StreetAddress` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

@Embeddable
public record StreetAddress(
        String street,
        String number,
        String city,
        String postalCode,
        String country
) {
  public StreetAddress() {
    this(null, null, null, null, null);
  }

  public StreetAddress(String street, String city, String postalCode, String country) {
    this(street, null, city, postalCode, country);
  }

  public String getStreetAddress() {
    return String.format("%s %s, %s, %s, %s", street, number, city, postalCode, country);
  }

  public StreetAddress {
    if (street == null || street.isBlank()) {
      throw new IllegalArgumentException("Street must not be null or blank");
    }
    if (city == null || city.isBlank()) {
      throw new IllegalArgumentException("City must not be null or blank");
    }
    if (postalCode == null || postalCode.isBlank()) {
      throw new IllegalArgumentException("Postal code must not be null or blank");
    }
    if (country == null || country.isBlank()) {
      throw new IllegalArgumentException("Country must not be null or blank");
    }
  }
}
```


### Package domain . model . aggregates

En el package `aggregates` crear la clase `Profile` con el siguiente contenido:
```java
import jakarta.persistence.*;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.EmailAddress;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.PersonName;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.StreetAddress;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

@Entity
public class Profile extends AuditableAbstractAggregateRoot<Profile> {

  @Embedded
  private PersonName name;

  @Embedded
  @AttributeOverrides({
          @AttributeOverride(name = "address", column = @Column(name = "email_address"))})
  EmailAddress email;

  @Embedded
  @AttributeOverrides({
          @AttributeOverride(name = "street", column = @Column(name = "address_street")),
          @AttributeOverride(name = "number", column = @Column(name = "address_number")),
          @AttributeOverride(name = "city", column = @Column(name = "address_city")),
          @AttributeOverride(name = "postalCode", column = @Column(name = "address_postal_code")),
          @AttributeOverride(name = "country", column = @Column(name = "address_country"))})
  private StreetAddress address;

  public Profile(String firstName, String lastName, String email, String street, String number, String city, String postalCode, String country) {
    this.name = new PersonName(firstName, lastName);
    this.email = new EmailAddress(email);
    this.address = new StreetAddress(street, number, city, postalCode, country);
  }

  public Profile(CreateProfileCommand command) {
    this.name = new PersonName(command.firstName(), command.lastName());
    this.email = new EmailAddress(command.email());
    this.address = new StreetAddress(command.street(), command.number(), command.city(), command.postalCode(), command.country());
  }

  public Profile() {
  }

  public void updateName(String firstName, String lastName) {
    this.name = new PersonName(firstName, lastName);
  }

  public void updateEmail(String email) {
    this.email = new EmailAddress(email);
  }

  public void updateAddress(String street, String number, String city, String postalCode, String country) {
    this.address = new StreetAddress(street, number, city, postalCode, country);
  }

  public String getFullName() {
    return name.getFullName();
  }

  public String getEmailAddress() {
    return email.address();
  }

  public String getStreetAddress() {
    return address.getStreetAddress();
  }
}
```
### Package domain . model . queries

En el package `queries` crear el record `GetAllProfilesQuery` con el siguiente contenido:
```java
public record GetAllProfilesQuery() {
}
```

En el package `queries` crear el record `GetProfileByEmailQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.EmailAddress;

public record GetProfileByEmailQuery(EmailAddress emailAddress) {
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
public record CreateProfileCommand(String firstName, String lastName, String email, 
                                   String street, String number, String city, 
                                   String postalCode, String country) {
}
```

### Package domain . services

En el package `services` crear el interface `ProfileCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;

import java.util.Optional;

public interface ProfileCommandService {
  Optional<Profile> handle(CreateProfileCommand command);
}
```

En el package `services` crear el interface `ProfileQueryService` con el siguiente contenido:

```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByEmailQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;

import java.util.List;
import java.util.Optional;

public interface ProfileQueryService {
  Optional<Profile> handle(GetProfileByEmailQuery query);
  Optional<Profile> handle(GetProfileByIdQuery query);
  List<Profile> handle(GetAllProfilesQuery query);
}
```

### Package infrastructure.persistence.jpa.repositories

En el package `repositories` crear el interface `ProfileRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.EmailAddress;

import java.util.Optional;

@Repository
public interface ProfileRepository extends JpaRepository<Profile, Long> {
  Optional<Profile> findByEmail(EmailAddress emailAddress);
}
```

### Package application.internal queryservices

En el package `queryservices` crear la clase `ProfileQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByEmailQuery;
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
  public Optional<Profile> handle(GetProfileByEmailQuery query) {
    return profileRepository.findByEmail(query.emailAddress());
  }

  @Override
  public Optional<Profile> handle(GetProfileByIdQuery query) {
    return profileRepository.findById(query.profileId());
  }

  @Override
  public List<Profile> handle(GetAllProfilesQuery query) {
    return profileRepository.findAll();
  }
}
```

### Package application.internal commandservices

En el package `commandservices` crear la clase `ProfileCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.EmailAddress;
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
  public Optional<Profile> handle(CreateProfileCommand command) {
    var emailAddress = new EmailAddress(command.email());
    profileRepository.findByEmail(emailAddress).map(profile -> {
      throw new IllegalArgumentException("Profile with email " + command.email() + " already exists");
    });
    var profile = new Profile(command);
    profileRepository.save(profile);
    return Optional.of(profile);
  }
}
```

### Package interfaces rest resources

En el package `resources` crear el record `CreateProfileResource` con el siguiente contenido:
```java
public record CreateProfileResource(String firstName, String lastName, String email,
    String street, String number, String city, String postalCode, String country) {
}
```

En el package `resources` crear el record `ProfileResource` con el siguiente contenido:
```java
public record ProfileResource(Long id, String fullName, String email, String streetAddress) {
}
```

### Package interfaces rest transform

En el package `transform` crear la clase `CreateProfileCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.CreateProfileResource;

public class CreateProfileCommandFromResourceAssembler {
  public static CreateProfileCommand toCommandFromResource(CreateProfileResource resource) {
    return new CreateProfileCommand(resource.firstName(), resource.lastName(), 
        resource.email(), resource.street(), resource.number(), resource.city(), 
        resource.postalCode(), resource.country());
  }
}
```

En el package `transform` crear el record `ProfileResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.profiles.domain.model.aggregates.Profile;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;

public class ProfileResourceFromEntityAssembler {
  public static ProfileResource toResourceFromEntity(Profile entity) {
    return new ProfileResource(entity.getId(), entity.getEmailAddress(), 
        entity.getFullName(), entity.getStreetAddress());
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
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetAllProfilesQuery;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByIdQuery;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileCommandService;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileQueryService;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.CreateProfileResource;
import pe.edu.upc.center.platform.profiles.interfaces.rest.resources.ProfileResource;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.CreateProfileCommandFromResourceAssembler;
import pe.edu.upc.center.platform.profiles.interfaces.rest.transform.ProfileResourceFromEntityAssembler;

import java.util.List;
import java.util.stream.Collectors;

/**
 * ProfilesController
 * <p>
 *     This class is the entry point for all the REST endpoints related to the Profile entity.
 * </p>
 */

@RestController
@RequestMapping(value = "/api/v1/profiles", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Profiles", description = "Profile Management Endpoints")
public class ProfilesController {
  private final ProfileQueryService profileQueryService;
  private final ProfileCommandService profileCommandService;

  public ProfilesController(ProfileQueryService profileQueryService, 
      ProfileCommandService profileCommandService) {
    
    this.profileQueryService = profileQueryService;
    this.profileCommandService = profileCommandService;
  }

  /**
   * Creates a new Profile
   * @param resource the resource containing the data to create the Profile
   * @return the created Profile
   */
  @PostMapping
  public ResponseEntity<ProfileResource> createProfile(
      @RequestBody CreateProfileResource resource) {
    
    var createProfileCommand = CreateProfileCommandFromResourceAssembler
        .toCommandFromResource(resource);
    var profile = profileCommandService.handle(createProfileCommand);
    if (profile.isEmpty())
      return ResponseEntity.badRequest().build();
    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(profile.get());
    return new ResponseEntity<>(profileResource, HttpStatus.CREATED);
  }

  /**
   * Gets a Profile by its id
   * @param profileId the id of the Profile to get
   * @return the Profile resource associated to given Profile id
   */
  @GetMapping("/{profileId}")
  public ResponseEntity<ProfileResource> getProfileById(@PathVariable Long profileId) {
    var getProfileByIdQuery = new GetProfileByIdQuery(profileId);
    var profile = profileQueryService.handle(getProfileByIdQuery);
    if (profile.isEmpty())
      return ResponseEntity.badRequest().build();
    var profileResource = ProfileResourceFromEntityAssembler.toResourceFromEntity(profile.get());
    return ResponseEntity.ok(profileResource);
  }

  /**
   * Gets all the Profiles
   * @return a list of all the Profile resources currently stored
   */
  @GetMapping
  public ResponseEntity<List<ProfileResource>> getAllProfiles() {
    var getAllProfilesQuery = new GetAllProfilesQuery();
    var profiles = profileQueryService.handle(getAllProfilesQuery);
    var profileResources = profiles.stream()
        .map(ProfileResourceFromEntityAssembler::toResourceFromEntity)
        .collect(Collectors.toList());
    return ResponseEntity.ok(profileResources);
  }
}
```

### Package interfaces acl

En el package `acl` crear la clase `ProfilesContextFacade` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.profiles.domain.model.commands.CreateProfileCommand;
import pe.edu.upc.center.platform.profiles.domain.model.queries.GetProfileByEmailQuery;
import pe.edu.upc.center.platform.profiles.domain.model.valueobjects.EmailAddress;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileCommandService;
import pe.edu.upc.center.platform.profiles.domain.services.ProfileQueryService;

/**
 * Service Facade for the Profile context.
 *
 * <p>
 * It is used by the other contexts to interact with the Profile context.
 * It is implemented as part of an anti-corruption layer (ACL) to be consumed by other contexts.
 * </p>
 *
 */
@Service
public class ProfilesContextFacade {
  private final ProfileCommandService profileCommandService;
  private final ProfileQueryService profileQueryService;

  public ProfilesContextFacade(ProfileCommandService profileCommandService,
      ProfileQueryService profileQueryService) {
    this.profileCommandService = profileCommandService;
    this.profileQueryService = profileQueryService;
  }

  /**
   * Creates a new Profile
   *
   * @param firstName the first name
   * @param lastName the last name
   * @param email the email
   * @param street the street address
   * @param number the number
   * @param city the city
   * @param state the state
   * @param zipCode the zip code
   * @return the profile id
   */
  public Long createProfile(String firstName, String lastName, String email, String street,
      String number, String city, String state, String zipCode) {

    var createProfileCommand = new CreateProfileCommand(firstName, lastName, email, street,
        number, city, state, zipCode);
    var profile = profileCommandService.handle(createProfileCommand);
    if (profile.isEmpty())
      return 0L;
    return profile.get().getId();
  }

  /**
   * Fetches the profile id by email
   *
   * @param email the email
   * @return the profile id
   */
  public Long fetchProfileIdByEmail(String email) {
    var getProfileByEmailQuery = new GetProfileByEmailQuery(new EmailAddress(email));
    var profile = profileQueryService.handle(getProfileByEmailQuery);
    if (profile.isEmpty())
      return 0L;
    return profile.get().getId();
  }
}
```

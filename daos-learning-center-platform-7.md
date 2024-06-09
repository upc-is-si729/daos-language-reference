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

Abrir el archivo `application.properties` y agreagr el siguiente código al final del archivo:
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



### Package application.internal . outboundservices . hashing

En el package `hashing` crear el interface `HashingService` con el siguiente contenido:
```java
/**
 * HashingService interface
 * This interface is used to encode and match passwords
 */
public interface HashingService {
  /**
   * Encode a password
   * @param rawPassword the password to encode
   * @return String the encoded password
   */
  String encode(CharSequence rawPassword);

  /**
   * Match a raw password with an encoded password
   * @param rawPassword the raw password
   * @param encodedPassword the encoded password
   * @return boolean true if the raw password matches the encoded password, false otherwise
   */
  boolean matches(CharSequence rawPassword, String encodedPassword);
}
```

### Package application.internal . outboundservices . tokens

En el package `tokens` crear el interface `TokenService` con el siguiente contenido:
```java
/**
 * TokenService interface
 * This interface is used to generate and validate tokens
 */
public interface TokenService {

  /**
   * Generate a token for a given username
   * @param username the username
   * @return String the token
   */
  String generateToken(String username);

  /**
   * Extract the username from a token
   * @param token the token
   * @return String the username
   */
  String getUsernameFromToken(String token);

  /**
   * Validate a token
   * @param token the token
   * @return boolean true if the token is valid, false otherwise
   */
  boolean validateToken(String token);
}
```

### Package infrastructure . authorization.sfs model

En el package `model` crear la clase `UserDetailsImpl` con el siguiente contenido:
```java
import com.fasterxml.jackson.annotation.JsonIgnore;
import lombok.EqualsAndHashCode;
import lombok.Getter;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;

import java.util.Collection;
import java.util.stream.Collectors;

/**
 * This class is responsible for providing the user details to the Spring Security framework.
 * It implements the UserDetails interface.
 */
@Getter
@EqualsAndHashCode
public class UserDetailsImpl implements UserDetails {

  private final String username;
  @JsonIgnore
  private final String password;
  private final boolean accountNonExpired;
  private final boolean accountNonLocked;
  private final boolean credentialsNonExpired;
  private final boolean enabled;
  private final Collection<? extends GrantedAuthority> authorities;

  public UserDetailsImpl(String username, String password, 
      Collection<? extends GrantedAuthority> authorities) {
    this.username = username;
    this.password = password;
    this.authorities = authorities;
    this.accountNonExpired = true;
    this.accountNonLocked = true;
    this.credentialsNonExpired = true;
    this.enabled = true;
  }

  /**
   * This method is responsible for building the UserDetailsImpl object from the User object.
   * @param user The user object.
   * @return The UserDetailsImpl object.
   */
  public static UserDetailsImpl build(User user) {
    var authorities = user.getRoles().stream()
        .map(role -> role.getName().name())
        .map(SimpleGrantedAuthority::new)
        .collect(Collectors.toList());
    
    return new UserDetailsImpl(user.getUsername(), user.getPassword(), authorities);
  }
}
```

En el package `model` crear la clase `UsernamePasswordAuthenticationTokenBuilder` con el siguiente contenido:
```java
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;

/**
 * This class is used to build the UsernamePasswordAuthenticationToken object
 * that is used to authenticate the user.
 */
public class UsernamePasswordAuthenticationTokenBuilder {

  /**
   * This method is responsible for building the UsernamePasswordAuthenticationToken object.
   * @param principal The user details.
   * @param request The HTTP request.
   * @return The UsernamePasswordAuthenticationToken object.
   * @see UsernamePasswordAuthenticationToken
   * @see UserDetails
   */
  public static UsernamePasswordAuthenticationToken build(UserDetails principal, 
      HttpServletRequest request) {
    
    var usernamePasswordAuthenticationToken = new UsernamePasswordAuthenticationToken(principal, 
        null, principal.getAuthorities());
    usernamePasswordAuthenticationToken.setDetails(
        new WebAuthenticationDetailsSource().buildDetails(request));
    return usernamePasswordAuthenticationToken;
  }
}
```

### Package infrastructure . persistence.jpa.repositories

En el package `repositories` crear el interface `RoleRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.domain.model.valueobjects.Roles;

import java.util.Optional;

/**
 * This interface is responsible for providing the Role entity related operations.
 * It extends the JpaRepository interface.
 */
@Repository
public interface RoleRepository extends JpaRepository<Role, Long> {

  /**
   * This method is responsible for finding the role by name.
   * @param name The role name.
   * @return The role object.
   */
  Optional<Role> findByName(Roles name);

  /**
   * This method is responsible for checking if the role exists by name.
   * @param name The role name.
   * @return True if the role exists, false otherwise.
   */
  boolean existsByName(Roles name);
}
```

En el package `repositories` crear el interface `UserRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;

import java.util.Optional;

/**
 * This interface is responsible for providing the User entity related operations.
 * It extends the JpaRepository interface.
 */
@Repository
public interface UserRepository extends JpaRepository<User, Long>
{
  /**
   * This method is responsible for finding the user by username.
   * @param username The username.
   * @return The user object.
   */
  Optional<User> findByUsername(String username);

  /**
   * This method is responsible for checking if the user exists by username.
   * @param username The username.
   * @return True if the user exists, false otherwise.
   */
  boolean existsByUsername(String username);
}
```

### Package infrastructure . hashing.bcrypt

En el package `bcrypt` crear el interface `BCryptHashingService` con el siguiente contenido:
```java
import org.springframework.security.crypto.password.PasswordEncoder;
import pe.edu.upc.center.platform.iam.application.internal.outboundservices.hashing.HashingService;
import pe.edu.upc.center.platform.iam.infrastructure.hashing.bcrypt.services.HashingServiceImpl;

/**
 * This interface is a marker interface for the BCrypt hashing service.
 * It extends the {@link HashingService} and {@link PasswordEncoder} interfaces.
 * This interface is used to inject the BCrypt hashing service in the {@link HashingServiceImpl} class.
 */
public interface BCryptHashingService extends HashingService, PasswordEncoder {
}
```

### Package infrastructure . hashing.bcrypt . services

En el package `services` crear la clase `HashingServiceImpl` con el siguiente contenido:
```java
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.infrastructure.hashing.bcrypt.BCryptHashingService;

/**
 * This class implements the {@link BCryptHashingService} interface.
 * It is used to hash passwords using the BCrypt algorithm.
 */
@Service
public class HashingServiceImpl implements BCryptHashingService {
  private final BCryptPasswordEncoder passwordEncoder;

  HashingServiceImpl() {
    this.passwordEncoder = new BCryptPasswordEncoder();
  }

  /**
   * Hash a password using the BCrypt algorithm
   * @param rawPassword the password to hash
   * @return String the hashed password
   */
  @Override
  public String encode(CharSequence rawPassword) {
    return passwordEncoder.encode(rawPassword);
  }

  /**
   * Check if a raw password matches a hashed password
   * @param rawPassword the raw password
   * @param encodedPassword the hashed password
   * @return boolean true if the raw password matches the hashed password, false otherwise
   */
  @Override
  public boolean matches(CharSequence rawPassword, String encodedPassword) {
    return passwordEncoder.matches(rawPassword, encodedPassword);
  }
}
```

### Package infrastructure . tokens.jwt

En el package `jwt` crear el interface `BearerTokenService` con el siguiente contenido:
```java
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.security.core.Authentication;
import pe.edu.upc.center.platform.iam.application.internal.outboundservices.tokens.TokenService;
import pe.edu.upc.center.platform.iam.infrastructure.tokens.jwt.services.TokenServiceImpl;

/**
 * This interface is a marker interface for the JWT token service.
 * It extends the {@link TokenService} interface.
 * This interface is used to inject the JWT token service in the {@link TokenServiceImpl} class.
 */
public interface BearerTokenService extends TokenService {

  /**
   * This method is responsible for extracting the JWT token from the HTTP request.
   * @param token the HTTP request
   * @return String the JWT token
   */
  String getBearerTokenFrom(HttpServletRequest token);

  /**
   * This method is responsible for generating a JWT token from an authentication object.
   * @param authentication the authentication object
   * @return String the JWT token
   * @see Authentication
   */
  String generateToken(Authentication authentication);
}
```

### Package infrastructure . tokens.jwt . services

En el package `services` crear la clase `TokenServiceImpl` con el siguiente contenido:
```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import io.jsonwebtoken.security.SignatureException;
import jakarta.servlet.http.HttpServletRequest;
import org.apache.commons.lang3.time.DateUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.Authentication;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;
import pe.edu.upc.center.platform.iam.infrastructure.tokens.jwt.BearerTokenService;

import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.util.Date;
import java.util.function.Function;

/**
 * Token service implementation for JWT tokens.
 * This class is responsible for generating and validating JWT tokens.
 * It uses the secret and expiration days from the application.properties file.
 */
@Service
public class TokenServiceImpl implements BearerTokenService {
  private final Logger LOGGER = LoggerFactory.getLogger(TokenServiceImpl.class);

  private static final String AUTHORIZATION_PARAMETER_NAME = "Authorization";
  private static final String BEARER_TOKEN_PREFIX = "Bearer ";

  private static final int TOKEN_BEGIN_INDEX = 7;

  @Value("${authorization.jwt.secret}")
  private String secret;

  @Value("${authorization.jwt.expiration.days}")
  private int expirationDays;

  /**
   * This method generates a JWT token from an authentication object
   * @param authentication the authentication object
   * @return String the JWT token
   * @see Authentication
   */
  @Override
  public String generateToken(Authentication authentication) {
    return buildTokenWithDefaultParameters(authentication.getName());
  }

  /**
   * This method generates a JWT token from a username
   * @param username the username
   * @return String the JWT token
   */
  public String generateToken(String username) {
    return buildTokenWithDefaultParameters(username);
  }

  /**
   * This method generates a JWT token from a username and a secret.
   * It uses the default expiration days from the application.properties file.
   * @param username the username
   * @return String the JWT token
   */
  private String buildTokenWithDefaultParameters(String username) {
    var issuedAt = new Date();
    var expiration = DateUtils.addDays(issuedAt, expirationDays);
    var key = getSigningKey();
    return Jwts.builder()
        .subject(username)
        .issuedAt(issuedAt)
        .expiration(expiration)
        .signWith(key)
        .compact();
  }

  /**
   * This method extracts the username from a JWT token
   * @param token the token
   * @return String the username
   */
  @Override
  public String getUsernameFromToken(String token) {
    return extractClaim(token, Claims::getSubject);
  }

  /**
   * This method validates a JWT token
   * @param token the token
   * @return boolean true if the token is valid, false otherwise
   */
  @Override
  public boolean validateToken(String token) {
    try {
      Jwts.parser().verifyWith(getSigningKey()).build().parseSignedClaims(token);
      LOGGER.info("Token is valid");
      return true;
    }  catch (SignatureException e) {
      LOGGER.error("Invalid JSON Web Token Signature: {}", e.getMessage());
    } catch (MalformedJwtException e) {
      LOGGER.error("Invalid JSON Web Token: {}", e.getMessage());
    } catch (ExpiredJwtException e) {
      LOGGER.error("JSON Web Token is expired: {}", e.getMessage());
    } catch (UnsupportedJwtException e) {
      LOGGER.error("JSON Web Token is unsupported: {}", e.getMessage());
    } catch (IllegalArgumentException e) {
      LOGGER.error("JSON Web Token claims string is empty: {}", e.getMessage());
    }
    return false;
  }

  /**
   * Extract a claim from a token
   * @param token the token
   * @param claimsResolvers the claims resolver
   * @param <T> the type of the claim
   * @return T the claim
   */
  private <T> T extractClaim(String token, Function<Claims, T> claimsResolvers) {
    final Claims claims = extractAllClaims(token);
    return claimsResolvers.apply(claims);
  }

  /**
   * Extract all claims from a token
   * @param token the token
   * @return Claims the claims
   */
  private Claims extractAllClaims(String token) {
    return Jwts.parser()
        .verifyWith(getSigningKey())
        .build()
        .parseSignedClaims(token)
        .getPayload();
  }

  /**
   * Get the signing key
   * @return SecretKey the signing key
   */
  private SecretKey getSigningKey() {
    byte[] keyBytes = secret.getBytes(StandardCharsets.UTF_8);
    return Keys.hmacShaKeyFor(keyBytes);
  }

  private boolean isTokenPresentIn(String authorizationParameter) {
    return StringUtils.hasText(authorizationParameter);
  }

  private boolean isBearerTokenIn(String authorizationParameter) {
    return authorizationParameter.startsWith(BEARER_TOKEN_PREFIX);
  }

  private String extractTokenFrom(String authorizationHeaderParameter) {
    return authorizationHeaderParameter.substring(TOKEN_BEGIN_INDEX);
  }

  private String getAuthorizationParameterFrom(HttpServletRequest request) {
    return request.getHeader(AUTHORIZATION_PARAMETER_NAME);
  }

  @Override
  public String getBearerTokenFrom(HttpServletRequest request) {
    String parameter = getAuthorizationParameterFrom(request);
    if (isTokenPresentIn(parameter) && isBearerTokenIn(parameter))
      return extractTokenFrom(parameter);
    return null;
  }
}
```

### Package infrastructure . authorization.sfs . pipeline

En el package `pipeline` crear la clase `BearerAuthorizationRequestFilter` con el siguiente contenido:
```java
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.lang.NonNull;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.web.filter.OncePerRequestFilter;
import pe.edu.upc.center.platform.iam.infrastructure.authorization.sfs.model.UsernamePasswordAuthenticationTokenBuilder;
import pe.edu.upc.center.platform.iam.infrastructure.tokens.jwt.BearerTokenService;

import java.io.IOException;

/**
 * Bearer Authorization Request Filter.
 * <p>
 * This class is responsible for filtering requests and setting the user authentication.
 * It extends the OncePerRequestFilter class.
 * </p>
 * @see OncePerRequestFilter
 */
public class BearerAuthorizationRequestFilter extends OncePerRequestFilter {

  private static final Logger LOGGER 
      = LoggerFactory.getLogger(BearerAuthorizationRequestFilter.class);
  private final BearerTokenService tokenService;

  @Qualifier("defaultUserDetailsService")
  private final UserDetailsService userDetailsService;

  public BearerAuthorizationRequestFilter(BearerTokenService tokenService, 
      UserDetailsService userDetailsService) {
    this.tokenService = tokenService;
    this.userDetailsService = userDetailsService;
  }

  /**
   * This method is responsible for filtering requests and setting the user authentication.
   * @param request The request object.
   * @param response The response object.
   * @param filterChain The filter chain object.
   */
  @Override
  protected void doFilterInternal(@NonNull HttpServletRequest request, 
      @NonNull HttpServletResponse response, @NonNull FilterChain filterChain) 
      throws ServletException, IOException {
    
    try {
      String token = tokenService.getBearerTokenFrom(request);
      LOGGER.info("Token: {}", token);
      if (token != null && tokenService.validateToken(token)) {
        String username = tokenService.getUsernameFromToken(token);
        var userDetails = userDetailsService.loadUserByUsername(username);
        SecurityContextHolder.getContext()
            .setAuthentication(
                UsernamePasswordAuthenticationTokenBuilder.build(userDetails, request));
      } 
      else {
        LOGGER.info("Token is not valid");
      }

    } catch (Exception e) {
      LOGGER.error("Cannot set user authentication: {}", e.getMessage());
    }
    filterChain.doFilter(request, response);
  }
}
```

En el package `pipeline` crear la clase `UnauthorizedRequestHandlerEntryPoint` con el siguiente contenido:
```java
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.stereotype.Component;

import java.io.IOException;

/**
 * Unauthorized Request Handler.
 * <p>
 * This class is responsible for handling unauthorized requests.
 * It is used by the Spring Security framework to handle unauthorized requests.
 * It implements the AuthenticationEntryPoint interface.
 * </p>
 * @see AuthenticationEntryPoint
 */

@Component
public class UnauthorizedRequestHandlerEntryPoint implements AuthenticationEntryPoint {

  private static final Logger LOGGER 
      = LoggerFactory.getLogger(UnauthorizedRequestHandlerEntryPoint.class);

  /**
   * This method is called by the Spring Security framework when an unauthorized request is detected.
   * @param request The request that caused the exception
   * @param response The response that will be sent to the client
   * @param authenticationException The exception that caused the invocation
   */
  @Override
  public void commence(HttpServletRequest request, HttpServletResponse response, 
      AuthenticationException authenticationException) throws IOException, ServletException {
    
    LOGGER.error("Unauthorized request: {}", authenticationException.getMessage());
    response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Unauthorized request detected");
  }
}
```

### Package infrastructure . authorization.sfs . services

En el package `services` crear la clase `UserDetailsServiceImpl` con el siguiente contenido:
```java
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.infrastructure.authorization.sfs.model.UserDetailsImpl;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.UserRepository;

/**
 * This class is responsible for providing the user details to the Spring Security framework.
 * It implements the UserDetailsService interface.
 */
@Service(value = "defaultUserDetailsService")
public class UserDetailsServiceImpl implements UserDetailsService {

  private final UserRepository userRepository;

  public UserDetailsServiceImpl(UserRepository userRepository) {
    this.userRepository = userRepository;
  }

  /**
   * This method is responsible for loading the user details from the database.
   * @param username The username.
   * @return The UserDetails object.
   * @throws UsernameNotFoundException If the user is not found.
   */
  @Override
  public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    var user = userRepository.findByUsername(username)
        .orElseThrow(
            () -> new UsernameNotFoundException("User not found with username: " + username));
    return UserDetailsImpl.build(user);
  }
}
```

### Package infrastructure . authorization.sfs . configuration

En el package `configuration` crear la clase `WebSecurityConfiguration` con el siguiente contenido:
```java
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import pe.edu.upc.center.platform.iam.infrastructure.authorization.sfs.pipeline.BearerAuthorizationRequestFilter;
import pe.edu.upc.center.platform.iam.infrastructure.hashing.bcrypt.BCryptHashingService;
import pe.edu.upc.center.platform.iam.infrastructure.tokens.jwt.BearerTokenService;

/**
 * Web Security Configuration.
 * <p>
 * This class is responsible for configuring the web security.
 * It enables the method security and configures the security filter chain.
 * It includes the authentication manager, the authentication provider, 
 * the password encoder and the authentication entry point.
 * </p>
 */
@Configuration
@EnableMethodSecurity
public class WebSecurityConfiguration {

  private final UserDetailsService userDetailsService;
  private final BearerTokenService tokenService;
  private final BCryptHashingService hashingService;
  
  private final AuthenticationEntryPoint unauthorizedRequestHandler;

  /**
   * This method creates the Bearer Authorization Request Filter.
   * @return The Bearer Authorization Request Filter
   */
  @Bean
  public BearerAuthorizationRequestFilter authorizationRequestFilter() {
    return new BearerAuthorizationRequestFilter(tokenService, userDetailsService);
  }

  /**
   * This method creates the authentication manager.
   * @param authenticationConfiguration The authentication configuration
   * @return The authentication manager
   */
  @Bean
  public AuthenticationManager authenticationManager(
      AuthenticationConfiguration authenticationConfiguration) throws Exception {
    return authenticationConfiguration.getAuthenticationManager();
  }

  /**
   * This method creates the authentication provider.
   * @return The authentication provider
   */
  @Bean
  public DaoAuthenticationProvider authenticationProvider() {
    var authenticationProvider = new DaoAuthenticationProvider();
    authenticationProvider.setUserDetailsService(userDetailsService);
    authenticationProvider.setPasswordEncoder(hashingService);
    return authenticationProvider;
  }

  /**
   * This method creates the password encoder.
   * @return The password encoder
   */
  @Bean
  public PasswordEncoder passwordEncoder() {
    return hashingService;
  }

  /**
   * This method creates the security filter chain.
   * It also configures the http security.
   *
   * @param http The http security
   * @return The security filter chain
   */
  @Bean
  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.csrf(csrfConfigurer -> csrfConfigurer.disable())
        .exceptionHandling(exceptionHandling -> exceptionHandling.authenticationEntryPoint(unauthorizedRequestHandler))
        .sessionManagement(customizer -> customizer.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
        .authorizeHttpRequests(
            authorizeRequests -> authorizeRequests.requestMatchers(
                "/api/v1/authentication/**", "/v3/api-docs/**", "/swagger-ui.html",
                "/swagger-ui/**", "/swagger-resources/**", "/webjars/**")
                .permitAll()
                .anyRequest()
                .authenticated());
    http.authenticationProvider(authenticationProvider());
    http.addFilterBefore(authorizationRequestFilter(), UsernamePasswordAuthenticationFilter.class);
    return http.build();
  }

  /**
   * This is the constructor of the class.
   * @param userDetailsService The user details service
   * @param tokenService The token service
   * @param hashingService The hashing service
   * @param authenticationEntryPoint The authentication entry point
   */
  public WebSecurityConfiguration(
      @Qualifier("defaultUserDetailsService") UserDetailsService userDetailsService, 
      BearerTokenService tokenService, BCryptHashingService hashingService, 
      AuthenticationEntryPoint authenticationEntryPoint) {
    
    this.userDetailsService = userDetailsService;
    this.tokenService = tokenService;
    this.hashingService = hashingService;
    this.unauthorizedRequestHandler = authenticationEntryPoint;
  }
}
```


### Package application.internal queryservices

En el package `queryservices` crear la clase `RoleQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllRolesQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetRoleByNameQuery;
import pe.edu.upc.center.platform.iam.domain.services.RoleQueryService;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.RoleRepository;

import java.util.List;
import java.util.Optional;

/**
 * RoleQueryServiceImpl class
 * This class is used to handle the role queries
 */
@Service
public class RoleQueryServiceImpl implements RoleQueryService {
  private final RoleRepository roleRepository;

  /**
   * RoleQueryServiceImpl constructor
   * @param roleRepository the role repository
   */
  public RoleQueryServiceImpl(RoleRepository roleRepository) {
    this.roleRepository = roleRepository;
  }

  /**
   * Handle the get all roles query
   * @param query the get all roles query
   * @return List<Role> the list of roles
   */
  @Override
  public List<Role> handle(GetAllRolesQuery query) {
    return roleRepository.findAll();
  }

  /**
   * Handle the get role by name query
   * @param query the get role by name query
   * @return Optional<Role> the role
   */
  @Override
  public Optional<Role> handle(GetRoleByNameQuery query) {
    return roleRepository.findByName(query.name());
  }
}
```

En el package `queryservices` crear la clase `UserQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllUsersQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByIdQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByUsernameQuery;
import pe.edu.upc.center.platform.iam.domain.services.UserQueryService;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.UserRepository;

import java.util.List;
import java.util.Optional;

/**
 * Implementation of {@link UserQueryService} interface.
 */
@Service
public class UserQueryServiceImpl implements UserQueryService {
  private final UserRepository userRepository;

  /**
   * Constructor.
   *
   * @param userRepository {@link UserRepository} instance.
   */
  public UserQueryServiceImpl(UserRepository userRepository) {
    this.userRepository = userRepository;
  }

  /**
   * This method is used to handle {@link GetAllUsersQuery} query.
   * @param query {@link GetAllUsersQuery} instance.
   * @return {@link List} of {@link User} instances.
   * @see GetAllUsersQuery
   */
  @Override
  public List<User> handle(GetAllUsersQuery query) {
    return userRepository.findAll();
  }

  /**
   * This method is used to handle {@link GetUserByIdQuery} query.
   * @param query {@link GetUserByIdQuery} instance.
   * @return {@link Optional} of {@link User} instance.
   * @see GetUserByIdQuery
   */
  @Override
  public Optional<User> handle(GetUserByIdQuery query) {
    return userRepository.findById(query.userId());
  }

  /**
   * This method is used to handle {@link GetUserByUsernameQuery} query.
   * @param query {@link GetUserByUsernameQuery} instance.
   * @return {@link Optional} of {@link User} instance.
   * @see GetUserByUsernameQuery
   */
  @Override
  public Optional<User> handle(GetUserByUsernameQuery query) {
    return userRepository.findByUsername(query.username());
  }
}
```

### Package application.internal commandservices

En el package `commandservices` crear la clase `RoleCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.domain.model.commands.SeedRolesCommand;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.domain.model.valueobjects.Roles;
import pe.edu.upc.center.platform.iam.domain.services.RoleCommandService;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.RoleRepository;

import java.util.Arrays;

/**
 * Implementation of {@link RoleCommandService} to handle {@link SeedRolesCommand}
 */
@Service
public class RoleCommandServiceImpl implements RoleCommandService {

  private final RoleRepository roleRepository;

  public RoleCommandServiceImpl(RoleRepository roleRepository) {
    this.roleRepository = roleRepository;
  }

  /**
   * This method will handle the {@link SeedRolesCommand} and will create the roles if not exists
   * @param command {@link SeedRolesCommand}
   * @see SeedRolesCommand
   */
  @Override
  public void handle(SeedRolesCommand command) {
    Arrays.stream(Roles.values())
        .forEach(role -> { 
          if(!roleRepository.existsByName(role)) {
            roleRepository.save(new Role(Roles.valueOf(role.name())));
          }
        } );
  }
}
```

En el package `commandservices` crear la clase `UserCommandServiceImpl` con el siguiente contenido:
```java
import org.apache.commons.lang3.tuple.ImmutablePair;
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.application.internal.outboundservices.hashing.HashingService;
import pe.edu.upc.center.platform.iam.application.internal.outboundservices.tokens.TokenService;
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignInCommand;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignUpCommand;
import pe.edu.upc.center.platform.iam.domain.services.UserCommandService;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.RoleRepository;
import pe.edu.upc.center.platform.iam.infrastructure.persistence.jpa.repositories.UserRepository;

import java.util.Optional;

/**
 * User command service implementation
 * <p>
 *     This class implements the {@link UserCommandService} interface and provides the implementation for the
 *     {@link SignInCommand} and {@link SignUpCommand} commands.
 * </p>
 */
@Service
public class UserCommandServiceImpl implements UserCommandService {

  private final UserRepository userRepository;
  private final HashingService hashingService;
  private final TokenService tokenService;

  private final RoleRepository roleRepository;

  public UserCommandServiceImpl(UserRepository userRepository, HashingService hashingService, 
      TokenService tokenService, RoleRepository roleRepository) {
    
    this.userRepository = userRepository;
    this.hashingService = hashingService;
    this.tokenService = tokenService;
    this.roleRepository = roleRepository;
  }

  /**
   * Handle the sign-in command
   * <p>
   *     This method handles the {@link SignInCommand} command and returns the user and the token.
   * </p>
   * @param command the sign-in command containing the username and password
   * @return and optional containing the user matching the username and the generated token
   * @throws RuntimeException if the user is not found or the password is invalid
   */
  @Override
  public Optional<ImmutablePair<User, String>> handle(SignInCommand command) {
    var user = userRepository.findByUsername(command.username());
    if (user.isEmpty())
      throw new RuntimeException("User not found");
    if (!hashingService.matches(command.password(), user.get().getPassword()))
      throw new RuntimeException("Invalid password");
    
    var token = tokenService.generateToken(user.get().getUsername());
    return Optional.of(ImmutablePair.of(user.get(), token));
  }

  /**
   * Handle the sign-up command
   * <p>
   *     This method handles the {@link SignUpCommand} command and returns the user.
   * </p>
   * @param command the sign-up command containing the username and password
   * @return the created user
   */
  @Override
  public Optional<User> handle(SignUpCommand command) {
    if (userRepository.existsByUsername(command.username()))
      throw new RuntimeException("Username already exists");
    var roles = command.roles().stream()
        .map(role -> 
            roleRepository.findByName(role.getName())
                .orElseThrow(() -> new RuntimeException("Role name not found")))
        .toList();
    var user = new User(command.username(), hashingService.encode(command.password()), roles);
    userRepository.save(user);
    return userRepository.findByUsername(command.username());
  }
}
```

### Package application.internal eventhandlers

En el package `eventhandlers` crear la clase `ApplicationReadyEventHandler` con el siguiente contenido:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.iam.domain.model.commands.SeedRolesCommand;
import pe.edu.upc.center.platform.iam.domain.services.RoleCommandService;

import java.sql.Timestamp;

/**
 * ApplicationReadyEventHandler class
 * This class is used to handle the ApplicationReadyEvent
 */
@Service
public class ApplicationReadyEventHandler {
  private final RoleCommandService roleCommandService;
  private static final Logger LOGGER 
      = LoggerFactory.getLogger(ApplicationReadyEventHandler.class);

  public ApplicationReadyEventHandler(RoleCommandService roleCommandService) {
    this.roleCommandService = roleCommandService;
  }

  /**
   * Handle the ApplicationReadyEvent
   * This method is used to seed the roles
   * @param event the ApplicationReadyEvent the event to handle
   */
  @EventListener
  public void on(ApplicationReadyEvent event) {
    var applicationName = event.getApplicationContext().getId();
    LOGGER.info("Starting to verify if roles seeding is needed for {} at {}", 
        applicationName, currentTimestamp());
    
    var seedRolesCommand = new SeedRolesCommand();
    roleCommandService.handle(seedRolesCommand);
    LOGGER.info("Roles seeding verification finished for {} at {}", 
        applicationName, currentTimestamp());
  }

  private Timestamp currentTimestamp() {
    return new Timestamp(System.currentTimeMillis());
  }
}
```

### Package interfaces rest resources

En el package `resources` crear el record `AuthenticatedUserResource` con el siguiente contenido:
```java
public record AuthenticatedUserResource(Long id, String username, String token) {
}
```

En el package `resources` crear el record `RoleResource` con el siguiente contenido:
```java
public record RoleResource(Long id, String name) {
}
```

En el package `resources` crear el record `SignInResource` con el siguiente contenido:
```java
public record SignInResource(String username, String password) {
}
```

En el package `resources` crear el record `SignUpResource` con el siguiente contenido:
```java
import java.util.List;

public record SignUpResource(String username, String password, List<String> roles) {
}
```

En el package `resources` crear el record `UserResource` con el siguiente contenido:
```java
import java.util.List;

public record UserResource(Long id, String username, List<String> roles) {
}
```

### Package interfaces rest transform

En el package `transform` crear la clase `AuthenticatedUserResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.AuthenticatedUserResource;

public class AuthenticatedUserResourceFromEntityAssembler {

  public static AuthenticatedUserResource toResourceFromEntity(User user, String token) {
    return new AuthenticatedUserResource(user.getId(), user.getUsername(), token);
  }
}
```

En el package `transform` crear la clase `RoleResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.RoleResource;

public class RoleResourceFromEntityAssembler {

  public static RoleResource toResourceFromEntity(Role role) {
    return new RoleResource(role.getId(), role.getStringName());
  }
}
```

En el package `transform` crear la clase `SignInCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.commands.SignInCommand;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.SignInResource;

public class SignInCommandFromResourceAssembler {

  public static SignInCommand toCommandFromResource(SignInResource signInResource) {
    return new SignInCommand(signInResource.username(), signInResource.password());
  }
}
```

En el package `transform` crear la clase `SignUpCommandFromResourceAssembler` con el siguiente contenido:
```java
import java.util.ArrayList;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignUpCommand;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.SignUpResource;

public class SignUpCommandFromResourceAssembler {

  public static SignUpCommand toCommandFromResource(SignUpResource resource) {
    var roles = resource.roles() != null 
        ? resource.roles().stream().map(name -> Role.toRoleFromName(name)).toList() 
        : new ArrayList<Role>();
    return new SignUpCommand(resource.username(), resource.password(), roles);
  }
}
```

En el package `transform` crear la clase `UserResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.iam.domain.model.aggregates.User;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.UserResource;

public class UserResourceFromEntityAssembler {

  public static UserResource toResourceFromEntity(User user) {
    var roles = user.getRoles().stream()
        .map(Role::getStringName)
        .toList();
    return new UserResource(user.getId(), user.getUsername(), roles);
  }
}
```

### Package interfaces rest

En el package `rest` crear la clase `RolesController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import java.util.List;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllRolesQuery;
import pe.edu.upc.center.platform.iam.domain.services.RoleQueryService;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.RoleResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.RoleResourceFromEntityAssembler;

/**
 *  Roles Controller
 *  This controller is responsible for handling all the requests related to roles
 */
@RestController
@RequestMapping(value = "/ap/v1/roles", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Roles", description = "Role Management Endpoints")
public class RolesController {

  private final RoleQueryService roleQueryService;

  public RolesController(RoleQueryService roleQueryService) {
    this.roleQueryService = roleQueryService;
  }

  /**
   * Get all roles
   * @return List of role resources
   * @see RoleResource
   */
  @GetMapping
  public ResponseEntity<List<RoleResource>> getAllRoles() {
    var getAllRolesQuery = new GetAllRolesQuery();
    var roles = roleQueryService.handle(getAllRolesQuery);
    var roleResources = roles.stream()
        .map(RoleResourceFromEntityAssembler::toResourceFromEntity)
        .toList();
    return ResponseEntity.ok(roleResources);
  }
}
```

En el package `rest` crear la clase `UsersController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import java.util.List;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetAllUsersQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByIdQuery;
import pe.edu.upc.center.platform.iam.domain.services.UserQueryService;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.UserResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.UserResourceFromEntityAssembler;

/**
 * This class is a REST controller that exposes the users resource.
 * It includes the following operations:
 * - GET /api/v1/users: returns all the users
 * - GET /api/v1/users/{userId}: returns the user with the given id
 **/
@RestController
@RequestMapping(value = "/api/v1/users", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Users", description = "User Management Endpoints")
public class UsersController {

  private final UserQueryService userQueryService;

  public UsersController(UserQueryService userQueryService) {
    this.userQueryService = userQueryService;
  }

  /**
   * This method returns all the users.
   *
   * @return a list of user resources.
   * @see UserResource
   */
  @GetMapping
  public ResponseEntity<List<UserResource>> getAllUsers() {
    var getAllUsersQuery = new GetAllUsersQuery();
    var users = userQueryService.handle(getAllUsersQuery);
    var userResources = users.stream()
        .map(UserResourceFromEntityAssembler::toResourceFromEntity)
        .toList();
    return ResponseEntity.ok(userResources);
  }

  /**
   * This method returns the user with the given id.
   *
   * @param userId the user id.
   * @return the user resource with the given id
   * @throws RuntimeException if the user is not found
   * @see UserResource
   */
  @GetMapping(value = "/{userId}")
  public ResponseEntity<UserResource> getUserById(@PathVariable Long userId) {
    var getUserByIdQuery = new GetUserByIdQuery(userId);
    var user = userQueryService.handle(getUserByIdQuery);
    if (user.isEmpty()) {
      return ResponseEntity.notFound().build();
    }
    var userResource = UserResourceFromEntityAssembler.toResourceFromEntity(user.get());
    return ResponseEntity.ok(userResource);
  }
}
```

En el package `rest` crear la clase `AuthenticationController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pe.edu.upc.center.platform.iam.domain.services.UserCommandService;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.AuthenticatedUserResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.SignInResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.SignUpResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.resources.UserResource;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.AuthenticatedUserResourceFromEntityAssembler;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.SignInCommandFromResourceAssembler;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.SignUpCommandFromResourceAssembler;
import pe.edu.upc.center.platform.iam.interfaces.rest.transform.UserResourceFromEntityAssembler;

/**
 * AuthenticationController
 * <p>
 *     This controller is responsible for handling authentication requests.
 *     It exposes two endpoints:
 *     <ul>
 *         <li>POST /api/v1/auth/sign-in</li>
 *         <li>POST /api/v1/auth/sign-up</li>
 *     </ul>
 * </p>
 */
@RestController
@RequestMapping(value = "/api/v1/authentication", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Authentication", description = "Authentication Endpoints")
public class AuthenticationController {

  private final UserCommandService userCommandService;

  public AuthenticationController(UserCommandService userCommandService) {
    this.userCommandService = userCommandService;
  }

  /**
   * Handles the sign-in request.
   * @param signInResource the sign-in request body.
   * @return the authenticated user resource.
   */
  @PostMapping("/sign-in")
  public ResponseEntity<AuthenticatedUserResource> signIn(
      @RequestBody SignInResource signInResource) {
    
    var signInCommand = SignInCommandFromResourceAssembler
        .toCommandFromResource(signInResource);
    var authenticatedUser = userCommandService.handle(signInCommand);
    if (authenticatedUser.isEmpty()) {
      return ResponseEntity.notFound().build();
    }
    
    var authenticatedUserResource = AuthenticatedUserResourceFromEntityAssembler
        .toResourceFromEntity(
            authenticatedUser.get().getLeft(), authenticatedUser.get().getRight());
    return ResponseEntity.ok(authenticatedUserResource);
  }

  /**
   * Handles the sign-up request.
   * @param signUpResource the sign-up request body.
   * @return the created user resource.
   */
  @PostMapping("/sign-up")
  public ResponseEntity<UserResource> signUp(@RequestBody SignUpResource signUpResource) {
    var signUpCommand = SignUpCommandFromResourceAssembler
        .toCommandFromResource(signUpResource);
    var user = userCommandService.handle(signUpCommand);
    if (user.isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var userResource = UserResourceFromEntityAssembler.toResourceFromEntity(user.get());
    return new ResponseEntity<>(userResource, HttpStatus.CREATED);
  }
}
```

### Package interfaces acl

En el package `acl` crear la clase `IamContextFacade` con el siguiente contenido:
```java
import org.apache.logging.log4j.util.Strings;
import pe.edu.upc.center.platform.iam.domain.model.commands.SignUpCommand;
import pe.edu.upc.center.platform.iam.domain.model.entities.Role;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByIdQuery;
import pe.edu.upc.center.platform.iam.domain.model.queries.GetUserByUsernameQuery;
import pe.edu.upc.center.platform.iam.domain.services.UserCommandService;
import pe.edu.upc.center.platform.iam.domain.services.UserQueryService;

import java.util.ArrayList;
import java.util.List;

/**
 * IamContextFacade
 * <p>
 *     This class is a facade for the IAM context. It provides a simple interface for other 
 *     bounded contexts to interact with the
 *     IAM context.
 *     This class is a part of the ACL layer.
 * </p>
 *
 */
public class IamContextFacade {

  private final UserCommandService userCommandService;
  private final UserQueryService userQueryService;

  public IamContextFacade(UserCommandService userCommandService,
      UserQueryService userQueryService) {
    this.userCommandService = userCommandService;
    this.userQueryService = userQueryService;
  }

  /**
   * Creates a user with the given username and password.
   * @param username The username of the user.
   * @param password The password of the user.
   * @return The id of the created user.
   */
  public Long createUser(String username, String password) {
    var signUpCommand = new SignUpCommand(username, password, List.of(Role.getDefaultRole()));
    var result = userCommandService.handle(signUpCommand);
    if (result.isEmpty()) return 0L;
    return result.get().getId();
  }

  /**
   * Creates a user with the given username, password and roles.
   * @param username The username of the user.
   * @param password The password of the user.
   * @param roleNames The names of the roles of the user. When a role does not exist, 
   *                  it is ignored.
   * @return The id of the created user.
   */
  public Long createUser(String username, String password, List<String> roleNames) {
    var roles = roleNames != null
        ? roleNames.stream().map(Role::toRoleFromName).toList()
        : new ArrayList<Role>();
    var signUpCommand = new SignUpCommand(username, password, roles);
    var result = userCommandService.handle(signUpCommand);
    if (result.isEmpty())
      return 0L;
    return result.get().getId();
  }

  /**
   * Fetches the id of the user with the given username.
   * @param username The username of the user.
   * @return The id of the user.
   */
  public Long fetchUserIdByUsername(String username) {
    var getUserByUsernameQuery = new GetUserByUsernameQuery(username);
    var result = userQueryService.handle(getUserByUsernameQuery);
    if (result.isEmpty())
      return 0L;
    return result.get().getId();
  }

  /**
   * Fetches the username of the user with the given id.
   * @param userId The id of the user.
   * @return The username of the user.
   */
  public String fetchUsernameByUserId(Long userId) {
    var getUserByIdQuery = new GetUserByIdQuery(userId);
    var result = userQueryService.handle(getUserByIdQuery);
    if (result.isEmpty())
      return Strings.EMPTY;
    return result.get().getUsername();
  }
}
```


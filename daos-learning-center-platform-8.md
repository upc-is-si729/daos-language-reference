# Learning Center Platform - Part 8

## Table of contents

## Identity and Access Management (IAM) Bounded Context

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

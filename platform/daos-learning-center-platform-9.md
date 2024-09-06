# Learning Center Platform - Part 9

## Table of contents

## Identity and Access Management (IAM) Bounded Context

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
import org.springframework.web.cors.CorsConfiguration;
import pe.edu.upc.center.platform.iam.infrastructure.authorization.sfs.pipeline.BearerAuthorizationRequestFilter;
import pe.edu.upc.center.platform.iam.infrastructure.hashing.bcrypt.BCryptHashingService;
import pe.edu.upc.center.platform.iam.infrastructure.tokens.jwt.BearerTokenService;

import java.util.List;

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
    http.cors(corsConfigurer -> corsConfigurer.configurationSource( request -> {
      var cors = new CorsConfiguration();
      cors.setAllowedOrigins(List.of("*"));
      cors.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE"));
      cors.setAllowedHeaders(List.of("*"));
      return cors;
    } ));
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


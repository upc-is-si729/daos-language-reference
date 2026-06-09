# Project Sensibo Climate Control v2610

## Configuración inicial

### Base de datos

**Abrir** el `pgAdmin`, ingrese la contraseña `12345678` y **crear** la base de datos: `sensibo`.

## Creación del project

**Cargar** el navegador y **pegar** el siguiente enlace:

https://start.spring.io/#!type=maven-project&language=java&platformVersion=4.0.6&packaging=jar&configurationFileFormat=properties&jvmVersion=26&groupId=com.sensibo.platform&artifactId=pc211990u202610123&packageName=com.sensibo.platform.u202610123&dependencies=data-jpa,validation,web,devtools,postgresql,lombok,springdoc-openapi

**Generar** el project Spring, Guardarlo en la carpeta `IdeaProjects\1ASI0729\` y luego abrirlo en `IntelliJ IDEA`.

### Configuración del SDK 

**Hacer** click en `File` luego en `Project Structure`. **Seleccionar** `Project` de `Project Settings` y seleccionar la versión `26` de Java para el `SDK`, en el caso de no estar instalado, descárguelo.  Luego **Seleccionar** `SDKs` de `Platform Settings` y seleccionar la versión seleccionada previamente, finalmente hacer click en `OK`.

### Archivo pom.xml

**Abrir** el archivo `pom.xml` y **modificar** la versión de Java en `<properties>`, tal como se muestra a continuación:

```xml
<properties>
    <java.version>26</java.version>
</properties>
```

Luego **agregar** las siguientes dependencias:

```xml
<!-- https://mvnrepository.com/artifact/io.github.encryptorcode/pluralize -->
<dependency>
    <groupId>io.github.encryptorcode</groupId>
    <artifactId>pluralize</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Proyecto LearningCenterPlatformApplication

**Abrir** el archivo `Pc211990u202610123Application.java` ubicado en el package `com.sensibo.platform.u202610123` y **agregar** el siguiente import al final de los imports:

```java
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
```

**Agregar** la anotación `@EnableJpaAuditing` debajo de la anotación `@SpringBootApplication`, la clase debe quedar:
```java
@SpringBootApplication
@EnableJpaAuditing
public class Pc211990u202610123Application { }
```

### Archivo application.properties

**Abrir** el archivo `application.properties` ubicado en la carpeta `src\main\resources` y **reemplazar** el contenido con el siguiente código:
```ini
# Spring DataSource Configuration
###    JDBC : SGDB :// HOST : PORT / DB
spring.datasource.url: jdbc:postgresql://localhost:5432/sensibo
spring.datasource.username: postgres
spring.datasource.password: 12345678
spring.datasource.driver-class-name: org.postgresql.Driver

# Spring Data JPA Configuration
spring.jpa.database: postgresql
spring.jpa.show-sql: true

# Spring Data JPA Hibernate Configuration
spring.jpa.hibernate.ddl-auto: update
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.format_sql: true
spring.jpa.properties.hibernate.dialect: org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.naming.physical-strategy=com.sensibo.platform.u202610123.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port: 8096

# Elements take their values from maven pom.xml build-related information
documentation.application.description=@project.description@
documentation.application.version=@project.version@

# i18n message bundles
spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

## Shared package

Copiar el `shared` del proyecto Learning Center

### Package infrastructure documentation.openapi.configuration

En el package `configuration` **actualizar** la clase `OpenApiConfiguration` con el siguiente contenido: 

```java
@Configuration
public class OpenApiConfiguration {
    // Properties
    @Value("${spring.application.name}")
    String applicationName;

    @Value("${documentation.application.description}")
    String applicationDescription;

    @Value("${documentation.application.version}")
    String applicationVersion;

    // Methods

    /**
     * Builds the OpenAPI document used by Swagger UI and client generation tools.
     *
     * @return configured OpenAPI descriptor
     */
    @Bean
    public OpenAPI learningPlatformOpenApi() {

        // General configuration
        var openApi = new OpenAPI();
        openApi
                .info(new Info()
                        .title(this.applicationName)
                        .description(this.applicationDescription)
                        .version(this.applicationVersion)
                        .contact(new Contact()
                                .name("Sensibo Climate Control")
                                .email("support@sensibo.com")
                                .url("https://sensibo.com/support"))
                        .license(new License()
                                .name("Apache 2.0")
                                .url("https://www.apache.org/licenses/LICENSE-2.0.html")))
                .externalDocs(new ExternalDocumentation()
                        .description("Sensibo Climate Control Platform wiki Documentation")
                        .url("https://sensibo.wiki.github.io/docs"));

        // Add server configurations
        openApi.servers(List.of(
                new Server()
                        .url("http://localhost:8096")
                        .description("Local Development Environment"),
                new Server()
                        .url("https://staging-api.sensibo.com")
                        .description("Staging Environment"),
                new Server()
                        .url("https://api.sensibo.com")
                        .description("Production Environment")
        ));

        return openApi;
    }
}
```

## registry Bounded Context

### Project structur for registry Bounded Context

**Crear** la estructura para el Bounded Context `registry`:

El siguiente archivo contiene la estructura completa de un Bounded Context: 
[Bounded Context Structure Template](bounded.zip)

### Package domain . model . valueobjects

En el package `valueobjects` **crear** el enum `DeviceTypes` con el siguiente contenido:
```java
import java.util.Arrays;

public enum DeviceTypes {
  SMART_AC_CONTROLLER(1),
  ROOM_SENSOR(2),
  AIR_QUALITY_MONITOR(3),
  DOOR_WINDOW_SENSOR(4);

  private final int id;

  DeviceTypes(int id) { this.id = id; }

  public int getId() { return id; }

  public static DeviceTypes fromValue(int id) {
    return Arrays.stream(DeviceTypes.values())
        .filter(dt -> dt.id == id)
        .findFirst()
        .orElseThrow(() ->
            new IllegalArgumentException("[DeviceTypes] Invalid value for DeviceTypes: " + id));
  }
  public static DeviceTypes fromString(String name) {
    return  Arrays.stream(DeviceTypes.values())
        .filter(dt -> dt.name().equalsIgnoreCase(name))
        .findFirst()
        .orElseThrow(() ->
            new IllegalArgumentException("[DeviceTypes] Invalid string for DeviceTypes: " + name));
  }
}
```


En el package `valueobjects` **crear** el record `MacAddress` con el siguiente contenido:
```java
import java.util.Objects;

public record MacAddress(String value) {

  private static final String MAC_REGEX = "^([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}$";

  public MacAddress {
    if (Objects.isNull(value) || value.isBlank()) {
      throw new IllegalArgumentException("[MacAddress] value cannot be null or blank");
    }
    if (!value.matches(MAC_REGEX)) {
      throw new IllegalArgumentException("[MacAddress] Invalid MAC address format. Expected AA:BB:CC:DD:EE:FF");
    }
  }

  public MacAddress() { this(null); }
}
```

En el package `valueobjects` **crear** el record `SensiboUserId` con el siguiente contenido:
```java
import java.util.Objects;

public record SensiboUserId(Long value) {

  public SensiboUserId {
    if (Objects.isNull(value)) {
      throw new IllegalArgumentException("[SensiboUserId] value cannot be null");
    }
    if (value <= 0) {
      throw new IllegalArgumentException("[SensiboUserId] value must be greater than zero");
    }
  }

  // JPA default constructor
  public SensiboUserId() { this(null); }
}
```

En el package `valueobjects` **crear** el record `SerialNumber` con el siguiente contenido:
```java
import java.util.Objects;
import java.util.UUID;

public record SerialNumber(String value) {

  public SerialNumber {
    if (Objects.isNull(value) || value.isBlank()) {
      throw new IllegalArgumentException("[SerialNumber] value cannot be null or blank");
    }
    if (value.length() != 36) {
      throw new IllegalArgumentException("[SerialNumber] must be 36 characters long");
    }
    if (!value.matches("[a-f0-9]{8}-([a-f0-9]{4}-){3}[a-f0-9]{12}")) {
      throw new IllegalArgumentException("[SerialNumber] must be a valid UUID");
    }
  }

  public SerialNumber() {
    this(UUID.randomUUID().toString());
  }

}
```

En el package `valueobjects` **crear** el record `Version` con el siguiente contenido:
```java
import java.util.Objects;

public record Version(String value) {

  private static final String VERSION_REGEX = "^\\d+\\.\\d+\\.\\d+$";

  public Version {
    if (Objects.isNull(value) || value.isBlank()) {
      throw new IllegalArgumentException("[Version] value cannot be null or blank");
    }
    if (!value.matches(VERSION_REGEX)) {
      throw new IllegalArgumentException("[Version] Invalid version format. Expected X.Y.Z");
    }
  }

  public Version() { this(null); }
}
```

### Package domain . model . aggregates

En el package `aggregates` **crear** la clase `Device` con el siguiente contenido:
```java
import com.sensibo.platform.u202610123.registry.domain.model.valueobjects.*;
import com.sensibo.platform.u202610123.shared.domain.model.aggregates.AbstractDomainAggregateRoot;
import lombok.Getter;
import lombok.Setter;

import java.time.LocalDate;

@Getter
public class Device extends AbstractDomainAggregateRoot {

  @Setter
  private Long id;

  private SensiboUserId userId;

  private DeviceTypes deviceType;

  @Setter
  private String modelName;

  private SerialNumber serialNumber;

  private MacAddress macAddress;

  private Version firmwareVersion;

  @Setter
  private LocalDate installationDate;

  @Setter
  private String roomLocation;
}
```
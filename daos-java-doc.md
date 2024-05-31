# Java Language reference

## Table of contents

- [Java Language reference](#java-language-reference)
  - [Table of contents](#table-of-contents)
  - [record](#record)
  - [AbstractAggregateRoot](#abstractaggregateroot)
  - [Patrón CQRS](#patrón-cqrs)
  - [handle](#handle)

## record

Los `record` en Java son una característica introducida en Java 16 como parte de las JEP 395. Son una nueva clase de tipo que ayuda a modelar datos inmutables en Java. Los `record` son útiles cuando necesitas una clase que solo contenga datos y no comportamiento adicional.

Un `record` en Java tiene las siguientes características:

1. Es una clase `final`, por lo que no puede ser extendida.
2. Todos sus atributos son `final`, lo que significa que son inmutables una vez asignados.
3. Implementa automáticamente los métodos `equals()`, `hashCode()` y `toString()`.
4. Proporciona un constructor compacto, que permite la inicialización de los campos de manera concisa.

Por ejemplo, el siguiente `record` define un objeto de datos simple con dos campos, `firstName` y `lastName`:

```java
public record Name(String firstName, String lastName) {}
```

Este `record` genera automáticamente un constructor, métodos `equals()`, `hashCode()` y `toString()`, y métodos de acceso (getters) para `firstName` y `lastName`.

Los `record` son especialmente útiles para objetos de transferencia de datos (DTO), objetos de valor y cualquier otro objeto que sirva principalmente para transportar datos sin comportamiento adicional.

**JPA (Java Persistence API)** requiere un constructor sin argumentos para poder instanciar objetos.

```java
public record PersonName(
    String firstName,
    String lastName) {
  public PersonName() {
    this(null, null);
  }
}
```

Adicionalmente, se debe implementar un bloque de inicialización de instancia para validar los atributos del `record`, que se ejecuta cada vez que se crea una instancia. En el siguiente ejemplo, se verifica si `firstName` o `lastName` son null o están vacíos para lanzar una excepción `IllegalArgumentException`:

```java
public record PersonName(
    String firstName,
    String lastName) {
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
}
```

## AbstractAggregateRoot

La clase `AbstractAggregateRoot` es parte del módulo Spring Data. Se utiliza en el contexto de Domain-Driven Design (DDD) para representar un agregado raíz.

Un agregado raíz es una entidad en un agregado (un grupo de entidades y objetos de valor que se tratan como una sola unidad para fines de persistencia) que sirve como la única puerta de entrada para cualquier cambio en todo el agregado.

La clase `AbstractAggregateRoot` proporciona métodos para registrar eventos de dominio que se pueden utilizar para interactuar con otras partes de la aplicación de una manera desacoplada. Estos eventos se pueden publicar cuando se produce algún cambio de estado en el agregado raíz.

Por ejemplo:

```java
public class Profile extends AbstractAggregateRoot<Profile> {
    // ...
}
```

## Patrón CQRS

El patrón CQRS (Command Query Responsibility Segregation) es un patrón de arquitectura que separa las operaciones de lectura (queries) de las operaciones de escritura (commands) en un sistema. Esto permite optimizar cada operación de manera independiente y puede mejorar la escalabilidad y el rendimiento de las aplicaciones.

Aquí hay un ejemplo simple de cómo podrías implementar CQRS en Java con Spring Boot y JPA. 

Primero, definiríamos un record para el `Comamnd`. Los commands son las operaciones de escritura en CQRS.

```java
public record CreateProfileCommand(
  String firstName, 
  String lastName, 
  String email 
) {
}
```

A continuación, necesitamos un manejador de comandos que se encargue de ejecutar el comando.

```java
public interface ProfileCommandService {
    Long handle(CreateProfileCommand command);
}
```

Para las operaciones de lectura, definiríamos un `record` para nuestras Queries.

```java
public record GetProfileByEmailQuery(EmailAddress emailAddress) {
}
```

Finalmente, necesitamos un manejador de consultas que se encargue de ejecutar la consulta.

```java
public interface ProfileQueryService {
    Optional<Profile> handle(GetProfileByEmailQuery query);
}
```

## handle

En el patrón CQRS (Command Query Responsibility Segregation), `handle` es un método que se utiliza para manejar comandos y consultas.

En el caso de los comandos, como se ve en la interfaz `ProfileCommandService`, el método `handle` toma un comando (en este caso, `CreateProfileCommand`) como argumento. Este comando representa una solicitud para realizar una operación de escritura o actualización en el sistema. El método `handle` procesa este comando, realizando la operación solicitada y devolviendo cualquier resultado necesario (en este caso, un `Long` que probablemente representa el ID del perfil creado).

```java
public interface ProfileCommandService {
    Long handle(CreateProfileCommand command);
}
```

Por otro lado, en el caso de las consultas, como se ve en la interfaz `ProfileQueryService`, el método `handle` toma una consulta (como `GetProfileByEmailQuery`) como argumento. Esta consulta representa una solicitud para leer información del sistema. El método `handle` procesa esta consulta, recuperando la información solicitada y devolviéndola (en este caso, un `Optional<Profile>` que representa el perfil solicitado, si existe).

```java
public interface ProfileQueryService {
    Optional<Profile> handle(GetProfileByEmailQuery query);
}
```

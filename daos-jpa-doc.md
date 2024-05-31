# Java Persistence API (JPA) Language reference

## Table of contents

- [Java Persistence API (JPA) Language reference](#java-persistence-api-jpa-language-reference)
  - [Table of contents](#table-of-contents)
  - [@Embeddable](#embeddable)
  - [@Embedded](#embedded)
  - [@Column](#column)
  - [@EntityListeners](#entitylisteners)
  - [@AttributeOverrides](#attributeoverrides)
  - [@AttributeOverride](#attributeoverride)
  - [@CreatedDate](#createddate)
  - [@LastModifiedDate](#lastmodifieddate)
  - [@MappedSuperclass](#mappedsuperclass)

## @Embeddable

La anotación `@Embeddable` se utiliza para indicar que una clase puede ser **incrustada** en otras entidades. Los atributos de la clase anotada con `@Embeddable` pueden ser mapeados directamente a la tabla de la entidad que la contiene.

Es útil cuando tienes un grupo de atributos que se repiten en varias entidades, en lugar de repetir estos atributos en cada entidad, puedes definir una clase con estos campos y anotarla con `@Embeddable`. Luego, puedes usar esta clase en tus entidades.

Por ejemplo, si tienes una clase `Address` con los atributos `street`, `city`, `state`, y `zipCode`, puedes anotarla con `@Embeddable` y luego usarla en tus entidades `User` y `Company`:

```java
@Embeddable
public class Address {
    private String street;
    private String city;
    private String state;
    private String zipCode;
}
```

Y luego en tus entidades:

```java
@Entity
public class User {
    @Id
    private Long id;
    private String name;
    private String email;

    @Embedded
    private Address address;
}

@Entity
public class Company {
    @Id
    private Long id;
    private String name;

    @Embedded
    private Address address;
}
```
Los atributos de `Address` se mapearán directamente a las tablas `User` y `Company` en la base de datos.

## @Embedded

La anotación `@Embedded` se utiliza para indicar que una entidad o un valor embebido debe ser mapeado a la entidad que contiene la anotación.

Un valor **embebido** es una clase que no tiene su **propia identidad**, y sus atributos son mapeados directamente a la entidad que lo contiene. Esto es útil cuando tienes un grupo de atributos que quieres agrupar, pero no quieres crear una entidad separada para ellos.

Por ejemplo:

```java
@Embedded
private PersonName name;
```

Es importante mencionar que los valores embebidos pueden tener su propio ciclo de vida, pero siempre dependen de la entidad que los contiene. Si la entidad que contiene el valor embebido es eliminada, el valor embebido también será eliminado.

## @Column

La anotación `@Column` se utiliza para especificar los detalles de la columna para el atributo o propiedad que se mapea a la tabla, tiene varios atributos que puedes utilizar para personalizar el mapeo del atributo:

- `name`: Para especificar el nombre del campo de la tabla.
- `nullable`: Para especificar si el campo puede tener valores nulos.
- `unique`: Para especificar si los valores del campo deben ser únicos.
- `length`: Para especificar la longitud máxima del campo (solo es relevante para las cadenas).
- `columnDefinition`: Para especificar la definición del campo, lo que permite utilizar tipos de campos específicos de la base de datos.

Por ejemplo:

```java
@Column(name = "email")
String address;
```

Aquí, `@Column(name = "email")` especifica que el atributo `address` se mapeará al campo `email` en la tabla.

## @EntityListeners

La anotación `@EntityListeners` se utiliza para especificar los **listeners de eventos de ciclo de vida de una entidad**, que son clases que definen métodos de devolución de llamada que se invocan cuando ocurre un **evento**. como la creación, actualización y eliminación de una entidad.

Por ejemplo:

```java
@EntityListeners(AuditingEntityListener.class)
```

`AuditingEntityListener.class` es un listener de eventos que Spring Data JPA proporciona para admitir la auditoría automática. Este listener se encarga de establecer automáticamente los atributos `createdAt` y `updatedAt` en tu entidad cuando se crea o se actualiza.


## @AttributeOverrides

La anotación `@AttributeOverrides` se utiliza cuando se desea sobrescribir el mapeo de uno o más atributos de una clase o record embebido.

Por ejemplo:

```java
@Embedded
@AttributeOverrides({
  @AttributeOverride(name = "street", column = @Column(name = "address_street")),
  @AttributeOverride(name = "number", column = @Column(name = "address_number"))
})
private StreetAddress address;
```

`StreetAddress` es un record embebido que contiene los atributos `street` y `number`, La anotación `@AttributeOverrides` se utiliza para cambiar el nombre de los campos a los que se mapean estos atributos en la tabla. Por ejemplo, el atributo `street` de `StreetAddress` se mapeará a la columna `address_street` en la tabla.

## @AttributeOverride

La anotación `@AttributeOverride` se utiliza para sobrescribir el mapeo de un atributo de un valor embebido.

Puedes usar `@AttributeOverride` si solo necesitas sobrescribir el nombre de una sola columna. `@AttributeOverrides` solo es necesario cuando necesitas sobrescribir varios nombres de columnas.

## @CreatedDate

La anotación `@CreatedDate` se utiliza para realizar un seguimiento automático de la fecha de creación de una entidad. Cuando se guarda una entidad por primera vez, Spring Data JPA establecerá automáticamente el valor del campo anotado con `@CreatedDate` a la fecha y hora actuales.

Ejemplo:
```java
@CreatedDate
private Date createdAt;
```

`createdAt` es un atributo que registra la fecha y hora en que se creó. Cuando se guarda un nuevo dato en la tabla, Spring Data JPA automáticamente establecerá `createdAt` a la fecha y hora actuales.

Es importante mencionar que para que `@CreatedDate` funcione, necesitas habilitar la auditoría de JPA. Esto se hace a través de la anotación `@EnableJpaAuditing` en tu clase de configuración principal o en una de configuración específica para JPA.

Además, necesitas agregar `@EntityListeners(AuditingEntityListener.class)` a la entidad que deseas auditar:

```java
@EntityListeners(AuditingEntityListener.class)
@Entity
public class Profile extends AbstractAggregateRoot<Profile> {
    // ...
}
```

## @LastModifiedDate

La anotación `@LastModifiedDate` se utiliza para realizar un seguimiento automático de la fecha de última modificación de una entidad. Cada vez que se actualiza una entidad, Spring Data JPA establecerá automáticamente el valor del campo anotado con `@LastModifiedDate` a la fecha y hora actuales.

Ejemplo:
```java
@LastModifiedDate
private Date updatedAt;
```

`updatedAt` es un campo que registra la fecha y hora en que se actualizó por última vez. Spring Data JPA automáticamente establecerá `updatedAt` a la fecha y hora actuales.

## @MappedSuperclass

En Java, la anotación `@MappedSuperclass` se utiliza para designar una clase que será una superclase base para otras entidades, pero que no es una entidad por sí misma. 

Esta anotación es útil cuando tienes ciertos atributos o comportamientos que se repiten en varias entidades. En lugar de duplicar el código en cada entidad, puedes definir una clase con `@MappedSuperclass`, y luego extender esa clase en tus entidades.

Las clases anotadas con `@MappedSuperclass` no pueden ser consultadas directamente y no pueden ser utilizadas en operaciones de EntityManager o Query como parámetros de tipo. 

Ejemplo:

```java
@MappedSuperclass
public abstract class BaseEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    // ... otros campos comunes
    // ... getters y setters
}

@Entity
public class SomeEntity extends BaseEntity {
    // ... campos específicos de SomeEntity
    // ... getters y setters
}
```

En el ejemplo, `SomeEntity` heredará el atributo `id` de `BaseEntity`.

# Bean Validation Language reference

## Table of contents

- [Bean Validation Language reference](#bean-validation-language-reference)
  - [Table of contents](#table-of-contents)
  - [@Email](#email)


## @Email

La anotación `@Email` se utiliza para validar que una cadena de texto tenga un formato de correo electrónico válido. Si el valor del campo no es un correo electrónico válido, la validación fallará.

Por ejemplo:

```java
@Email
String address;
```

Aquí, `@Email` asegura que el valor de `address` sea un correo electrónico válido. Si intentas asignar un valor que no sea un correo electrónico válido a `address`, la validación fallará y se lanzará una excepción de validación.

Es importante mencionar que `@Email` solo verifica que la cadena tenga un formato de correo electrónico válido. No verifica que el correo electrónico exista o que pueda recibir correos electrónicos.
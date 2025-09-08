# Hello Angular Project

## Creación del proyecto

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y `chown`, y luego ingresar la contraseña del Administrador (`d3v3l0p3rUPC`).
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/1asi0729/2025-20` o en otra de su preferencia.

> [!CAUTION]
> **En el caso de estar en un equipo Windows:**
> - Debe ubicarse en la carpeta `IdeaProjects/` o en otra de su preferencia.

A continuación se detalla las intrucciones para crear un nuevo `workspace` e `initial starter app` de Angular. Más información en: https://angular.dev/tools/cli/setup-local

### Creación de un workspace y un initial application

**Cargar** el `Terminal` del sistema Operativo, ubicarse en la carpeta de su preferencia de acuerdo al Sistema Operativo y **ejecutar** el siguiente CLI command:

```bash
ng new daos-hello-angular
```

Despues de ejecutar el CLI command, le mostrará diferentes opciones y debe escoger las siguientes:

_? Which stylesheet format would you like to use?_, **Seleccionar** la siguiente opción:

```
CSS  [ https://developer.mozilla.org/docs/Web/CSS
```

_? Do you want to enable Server-Side Rendering (SSR) and Static Site Generation 
(SSG/Prerendering)?_, **digitar**:

```
N
```

Se creará el proyecto e iniciará la instalación de packages.

### Instalación de Angular Material

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y luego ingresar la contraseña del Administrador.

A continuación se detalla las intrucciones para instalar `Angular Material` al proyecto Angular. Más información en: https://material.angular.io/guide/getting-started

**Ingresar** a la carpeta creada con el mismo nombre que el proyecto **ejecutando** el siguiente command:
```
cd daos-hello-angular
```

**Agregar** Angula material a la aplicación, **ejecute** el siguiente CLI command:

```
ng add @angular/material
```

Despues de ejecutar el CLI command, le mostrará diferentes opciones y debe escoger las siguientes:

_The package @angular/material@19.2.19 will be installed and executed.
Would you like to proceed?_, **digitar**:
```
Y
```

_? Choose a prebuilt theme name, or "custom" for a custom theme:_, **seleccionar**:
```
Azure/Blue   [Preview: https://material.angular.io?theme=azure-blue]
```

_? Set up global Angular Material typography styles?_, **digitar**:
```
Y
```

### Cambiar el propietario del proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**


**Ejecutar** los siguientes commands:
```
cd ..
```

```
sudo chown -R alumnos ./daos-hello-angular
```

```
ls -l
```


## Desarrollo del proyecto

**Cargar** el IntelliJ IDEA y **abrir** el proyecto ubicado en la carpeta de su preferencia.

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command:
```
ng serve --port 4201
```

### Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 greetings
      - 📁 components
      - 📁 model
      - 📁 pages
      - 📁 services
```

### Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes: `developer-home`:
```bash
ng generate component public/pages/developer-home --skip-tests=true
```

`developer-registration`:
```bash
ng generate component learning/components/developer-registration --skip-tests=true
```

`developer-greeting`:
```bash
ng generate component learning/components/developer-greeting --skip-tests=true
```

# Hello Angular Project

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `npm`, `ng` y `chown`, y luego ingresar la contraseña del Administrador (`d3v3l0p3rUPC`).
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/1asi0729/2026-20` o en otra de su preferencia.

> [!CAUTION]
> **En el caso de estar en un equipo Windows:**
> - Debe ubicarse en la carpeta `IdeaProjects/` o en otra de su preferencia.

## Instalación y/o Actualización de Angular CLI

Para instalar `Angular CLI` en tu equipo, necesitas instalar Node.js https://nodejs.org/.  `Angular CLI` usa Node y está asociado al package manager `npm`, para instalar y ejecutar JavaScript fuera del navegador.

A continuación se detalla la intrucción para instalar la herramienta `Command Line Interface (CLI)` de Angular. Más información en: https://angular.dev/installation

```bash
npm install -g @angular/cli@latest
```

Verifica la versión del Angular ejecutando la siguiente instrucción:
```bash
ng version
```

## Creación del proyecto

A continuación se detalla las intrucciones para crear un nuevo `workspace` e `initial starter app` de Angular. Más información en: https://angular.dev/tools/cli/setup-local

### Creación de un workspace y un initial application

**Cargar** el `Terminal` del sistema Operativo, ubicarse en la carpeta de su preferencia de acuerdo al Sistema Operativo y **ejecutar** el siguiente CLI command:

```bash
ng new hello-angular-developer-v2620
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

_? Which AI tools do you want to configure with Angular best practices?_ https://angular.dev/ai/develop-with-ai

`(Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)`, **Seleccionar**:

```
(*) GitHub Copilot  [ https://code.visualstudio.com/docs/copilot/copilot-customization#_custom-instructions ]
(*) JetBrains AI Assistant [ https://www.jetbrains.com/help/junie/customize-guidelines.html                        ]
```

Se creará el proyecto e iniciará la instalación de packages.

### Instalación de Angular Material

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y luego ingresar la contraseña del Administrador.

A continuación se detalla las intrucciones para instalar `Angular Material` al proyecto Angular. Más información en: https://material.angular.io/guide/getting-started

**Ingresar** a la carpeta creada con el mismo nombre que el proyecto **ejecutando** el siguiente command:
```
cd hello-angular-developer-v2620
```

**Agregar** Angula material a la aplicación, **ejecute** el siguiente CLI command:

```
ng add @angular/material
```

Despues de ejecutar el CLI command, le mostrará diferentes opciones y debe escoger las siguientes:

_The package @angular/material@^22.1.5 will be installed and executed._ 
_Would you like to proceed? (Y/n)_, **digitar**:
```
Y
```

_? Select a pair of starter prebuilt color palettes for your Angular Material theme_, **seleccionar**:
```
Azure/Blue   [Preview: https://material.angular.io?theme=azure-blue]
```

### Instalación del paquete UUID de NPM

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `npm` y luego ingresar la contraseña del Administrador.

A continuación se detalla las intrucciones para instalar el paquete `UUID` de NPM al proyecto Angular. Más información en: https://www.npmjs.com/package/uuid

**Instalar** el paquete ejecutando el siguiente command:

```
npm install uuid
```

### Cambiar el propietario del proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**

**Ejecutar** los siguientes commands:
```
cd ..
```

```
sudo chown -R alumnos ./hello-angular-developer-v2620
```

```
ls -l
```


## Desarrollo del proyecto

**Cargar** el IntelliJ IDEA y **abrir** el proyecto `hello-angular-developer-v2620` ubicado en la carpeta donde la creo.

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command:
```
ng serve --port 4200
```

### Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 greetings
      - 📁 domain
        - 📁 model
      - 📁 presentation
        - 📁 components
          - 📁 developer-greeting
          - 📁 developer-registration      
    - 📂 shared
      - 📁 domain
```

### Creación del archivo uuid

**Crear** el archivo `uuid.ts` en la carpeta `shared/domain`.

**Agregar** el siguiente código al archivo `uuid.ts`:
```ts
import { v7 as uuidv7 } from 'uuid';

export function generateUuid(): string {
  return uuidv7();
}
```

### Creación del model developer tipo entity

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `developer`:

```bash
ng generate class greetings/domain/model/developer --skip-tests=true
```

**Agregar** los siguientes `import` al archivo `developer.ts`, ubicado en la carpeta `greetings/domain/model`:

```ts
import {generateUuid} from '../../../shared/domain/uuid';
```

**Agregar** los siguentes atributos, constructor y métodos a la clase `Developer` del archivo `developer.ts`:

```ts
readonly #id: string | null;
readonly #firstName: string;
readonly #lastName: string;

private static readonly MINIMUM_NAME_LENGTH = 2;
private static readonly ANONYMOUS_LABEL = 'Anonymous Developer';

constructor(firstName: string = '', lastName: string = '') {
  this.#firstName = firstName.trim();
  this.#lastName = lastName.trim();
  this.#id = Developer.isValidForRegistration(this.#firstName, this.#lastName) ? generateUuid() : null;
}

get id(): string | null {
  return this.#id;
}

get firstName(): string {
  return this.#firstName;
}

get lastName(): string {
  return this.#lastName;
}

get fullName(): string {
  return !this.isRegistered ? Developer.ANONYMOUS_LABEL : `${this.#firstName} ${this.#lastName}`.trim();
}

get isRegistered(): boolean {
  return this.#id !== null;
}

static isValidName(name: string): boolean {
  return name.trim().length >= Developer.MINIMUM_NAME_LENGTH;
}

static isValidForRegistration(firstName: string, lastName: string): boolean {
  return this.isValidName(firstName) && this.isValidName(lastName);
}

equals(other: Developer | null | undefined): boolean {
  return other === null || other === undefined ? false : this.#id === other.id;
}

static DEFAULT_DEVELOPER = new Developer();
```

### Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes: 

`developer-greeting`:
```bash
ng generate component greetings/presentation/components/developer-greeting --skip-tests=true
```

`developer-registration`:
```bash
ng generate component greetings/presentation/components/developer-registration --skip-tests=true
```

### Información de Angular Signals

**¿Qué son los signals?**: 

Un `signal` es un contenedor que envuelve un valor y notifica a los usuarios interesados ​​cuando este cambia. Los `Signals` pueden contener cualquier valor, desde primitivos hasta estructuras de datos complejas.

El valor de un `signal` se lee llamando a su función getter, lo que permite a Angular rastrear dónde se utiliza. Los `Signals` pueden ser de solo lectura o de escritura.

Más información en: https://angular.dev/guide/signals



### Modificación del App Component

**Agregar** los siguientes `import` al archivo `app.ts`, ubicado en la carpeta `/src/app`:

```ts
import {ChangeDetectionStrategy} from '@angular/core';
import {DeveloperGreeting} from './greetings/presentation/components/developer-greeting/developer-greeting';
import {Developer} from './greetings/domain/model/developer';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `App`, ubicado en el archivo `app.ts`

```ts
DeveloperGreeting
```

**Agregar** el siguiente `key:value` en el decorator `@Component` de la clase `App`, ubicado en el archivo `app.ts`

```ts
changeDetection: ChangeDetectionStrategy.OnPush
```

**Agregar** el siguiente código a la clase `App`:

```ts
protected readonly title = signal('hello-angular-developer-v2620');
protected registeredDeveloper = signal<Developer>(Developer.DEFAULT_DEVELOPER);

protected updateRegisteredDeveloperInfo(developer: Developer): void {
  this.registeredDeveloper.set(developer);
}

protected resetRegisteredDeveloperInfo(): void {
  this.registeredDeveloper.set(Developer.DEFAULT_DEVELOPER);
}
```

**Reemplazar** el contenido del archivo `app.html` con el siguiente código, ubicado en la carpeta `/src/app`:

```html
<!-- Greeting component displaying the current developer state -->
<app-developer-greeting [developer]="registeredDeveloper()"/>
<router-outlet />
```

### Información sobre comunicación entre componentes.

En Angular, input y output son los mecanismos para establecer la comunicación entre componentes.

Siguen el patrón de arquitectura de flujo de datos unidireccional (Unidirectional Data Flow):
- **input** (Entrada): Pasa datos desde un componente padre hacia un componente hijo (de arriba hacia abajo).
- **output** (Salida): Emite eventos desde un componente hijo hacia un componente padre (de abajo hacia arriba).

#### input() (Signal Inputs)

Sirve para que un componente hijo reciba datos pasados por el padre. A partir de las versiones modernas de Angular, las propiedades de entrada son Signals de solo lectura (InputSignal).
Para qué sirve:
- Configurar parámetros en componentes reutilizables (ej. el texto de un botón, el ID de un usuario, una lista de datos).
- Reaccionar a cambios en las propiedades de entrada usando computed() o effect().

### Modificación del DeveloperGreeting Component

**Agregar** los siguientes `import` al archivo `developer-greeting.ts`, ubicado en la carpeta `/src/app/greetings/presentation/components/developer-greeting`:

```ts
import {computed, input, ChangeDetectionStrategy} from '@angular/core';
import {Developer} from '../../../domain/model/developer';
```

**Agregar** el siguiente `key:value` en el decorator `@Component` de la clase `DeveloperGreeting`:

```ts
changeDetection: ChangeDetectionStrategy.OnPush
```

**Agregar** el atributo developer tipo `InputSignal` en la clase `DeveloperGreeting`:

```ts
developer: InputSignal<Developer> = input<Developer>(Developer.DEFAULT_DEVELOPER);
```

**Agregar** los atributos `Signal` en la clase `DeveloperGreeting`:

```ts
protected fullName = computed(() => this.developer().fullName);
protected developerIsRegistered = computed(() => this.developer().isRegistered);
protected developerId = computed(() => this.developer().id);
```

**Reemplazar** el contenido del archivo `developer-greeting.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/presentation/components/developer-greeting`:

```html
<!-- Greeting paragraph with conditional registered message -->
<p>
    Hello {{ fullName() }}.
    @if(developerIsRegistered())  {
        <span>Now You are an Angular Developer with ID: {{ developerId() }}!</span>
    }
</p>
```

**Reemplazar** el contenido del archivo `developer-greeting.css` con el siguiente código, ubicado en la carpeta `/src/app/greetings/presentation/components/developer-greeting`:

```css
/* Styles for the greeting paragraph */
p {
  font-size: 1.2em;
  color: darkslategray; /* Dark text for readability */
}

/* Ensures the conditional span stays inline */
span {
  display: inline;
}
```

### Visualizando el resultado del código

**Cargar** el Navegador y **abrir** la siguiente dirección:

```
http://localhost:4200/
```

### Modificación del App Component

**Agregar** el siguiente `import` al archivo `app.ts`, ubicado en la carpeta `/src/app`:

```ts
import {DeveloperRegistration} from './greetings/presentation/components/developer-registration/developer-registration';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `App`, ubicado en el archivo `app.ts`

```ts
DeveloperRegistration
```

**Agregar** la etiqueta `app-developer-registration` al archivo `app.html` antes que la etiqueta `app-developer-greeting`, ubicado en la carpeta `/src/app`:

```html
<!-- Registration form component with event bindings -->
<app-developer-registration
  (developerRegistered)="updateRegisteredDeveloperInfo($event)"
  (registrationDeferred)="resetRegisteredDeveloperInfo()"/>
```

### Información de Angular Signals

Un **Signal** es el primitivo fundamental de reactividad en Angular. En esencia, es un **envoltorio alrededor de un valor (wrapper)** que notifica a Angular inmediatamente cuando ese valor cambia, permitiendo actualizar únicamente la parte del HTML que depende de él.

Tipos principales de Signals
- **Signal Writable** (Escribible): Es el valor primario que puedes leer y modificar dinámicamente.
- **Computed Signal** (Calculado): Un valor derivado que se actualiza automáticamente cuando cambia cualquiera de los Signals que consume. Es de solo lectura y memoriza su resultado.
- **Effect** (Efecto): Una función que se ejecuta automáticamente cuando cambia algún Signal dentro de su contexto (ideal para llamadas a API, logging o sincronización con el DOM).

### Información sobre comunicación entre componentes.

#### output() (Signal Outputs)

Sirve para que el componente hijo notifique al padre cuando ocurre una acción o evento dentro de él (por ejemplo: dar clic a un botón, enviar un formulario o seleccionar un elemento).
Para qué sirve:
- Notificar eventos del usuario u operaciones completadas hacia el contexto superior.
- Mantener los componentes de UI desvinculados de la lógica global (el hijo solo avisa "ocurrió esto", el padre decide qué hacer).

### Modificación del DeveloperRegistration Component

**Agregar** los siguientes `import` al archivo `developer-registration.ts`, ubicado en la carpeta `/src/app/greetings/components/developer-registration`:

```ts
import {computed, output, signal, Signal, ChangeDetectionStrategy} from '@angular/core';
import {FormsModule, ReactiveFormsModule} from '@angular/forms';
import {Developer} from '../../../domain/model/developer';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `DeveloperRegistration`:

```ts
ReactiveFormsModule, FormsModule
```

**Agregar** el siguiente `key:value` en el decorator `@Component` de la clase `DeveloperGreeting`:

```ts
changeDetection: ChangeDetectionStrategy.OnPush
```

**Agregar** el siguiente atributo tipo `static` a la clase `DeveloperRegistration`:

```ts
static readonly EMPTY_NAME: string = '';
```

**Agregar** los signal firstName y lastName tipo `WritableSignal` en la clase `DeveloperRegistration`:

```ts
protected firstName: WritableSignal<string> = signal<string>(DeveloperRegistration.EMPTY_NAME);
protected lastName: WritableSignal<string> = signal<string>(DeveloperRegistration.EMPTY_NAME);
```

**Agregar** los signal isFormValid, isFirstNameValid y isLastNameValid tipo `Signal` que es el resultado de un `computed` en la clase `DeveloperRegistration`:

```ts
protected isFormValid: Signal<boolean> = computed(() =>
  Developer.isValidForRegistration(this.firstName(), this.lastName())
);

protected isFirstNameValid: Signal<boolean> = computed(() =>
  this.firstName().trim().length === 0 || Developer.isValidName(this.firstName())
);

protected isLastNameValid: Signal<boolean> = computed(() =>
  this.lastName().trim().length === 0 || Developer.isValidName(this.lastName())
);
```

**Agregar** los output developerRegistered y registrationDeferred tipo `OutputEmitterRef` en la clase `DeveloperRegistration`:

```ts
public developerRegistered: OutputEmitterRef<Developer> = output<Developer>();
public registrationDeferred: OutputEmitterRef<void> = output<void>();
```

**Agregar** los siguientes métodos a la clase `DeveloperRegistration`:

```ts
protected submitRegistrationRequest(): void {
  if (this.isFormValid()) {
    this.developerRegistered.emit(new Developer(this.firstName(), this.lastName()));
    this.clearFields();
  }
}

protected deferRegistration(): void {
  this.clearFields();
  this.registrationDeferred.emit();
}

protected clearFields(): void {
  this.firstName.set(DeveloperRegistration.EMPTY_NAME);
  this.lastName.set(DeveloperRegistration.EMPTY_NAME);
}
```

**Reemplazar** el contenido del archivo `developer-registration.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/components/developer-registration`:

```html
<!-- Heading for the registration form -->
<h1>New Developer</h1>

<!-- Form to input developer details, submits on Register -->
<form (ngSubmit)="submitRegistrationRequest()">
  <!-- First Name input with label -->
  <label for="first-name">First Name: </label>
  <input id="first-name" type="text" [(ngModel)]="firstName" name="firstName">
  <!-- Validation errors for firstName -->
  @if (!isFirstNameValid()) {
    <div class="error">First name must be at least 2 characters.</div>
  }

  <!-- Last Name input with label -->
  <label for="last-name">Last Name: </label>
  <input id="last-name" type="text" [(ngModel)]="lastName" name="lastName">
  <!-- Validation errors for lastName -->
  @if (!isLastNameValid()) {
    <div class="error">Last name must be at least 2 characters.</div>
  }
  <!-- Button group for form actions -->
  <div class="button-group">
    <button type="submit" [disabled]="!isFormValid()">Register</button>
    <button type="button" (click)="deferRegistration()">Later</button>
    <button type="button" (click)="clearFields()">Clear</button>
  </div>
</form>
```

**Reemplazar** el contenido del archivo `developer-registration.css` con el siguiente código, ubicado en la carpeta `/src/app/greetings/components/developer-registration`:

```css
/* Styles for the registration form layout */
form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 300px;
}

/* Bold labels for input fields */
label {
  font-weight: bold;
}

/* Input field styling with a light border */
input {
  padding: 5px;
  border: 1px solid silver; /* Light gray border */
  border-radius: 4px;
}

/* Container for action buttons */
.button-group {
  display: flex;
  gap: 10px;
}

/* Base button styles */
button {
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: white; /* White text for contrast */
}

/* Register button: green when enabled */
button[type="submit"] {
  background-color: green;
}

/* Disabled Register button: grayed out */
button[type="submit"]:disabled {
  background-color: gray;
  cursor: not-allowed;
}

/* Later button: goldenrod for deferring */
button:nth-child(2) {
  background-color: goldenrod;
}

/* Hover effect for Later button */
button:nth-child(2):hover {
  background-color: darkgoldenrod;
}

/* Clear button: red for resetting */
button:nth-child(3) {
  background-color: red;
}

/* Hover effect for Clear button */
button:nth-child(3):hover {
  background-color: darkred;
}

/* Error message styling */
.error {
  color: red; /* Red text for visibility */
  font-size: 0.9em;
}

```


### Visualizando el resultado del código

**Cargar** el Navegador y **abrir** la siguiente dirección:

```
http://localhost:4200/
```
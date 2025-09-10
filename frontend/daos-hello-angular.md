# Hello Angular Project

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `npm`, `ng` y `chown`, y luego ingresar la contraseña del Administrador (`d3v3l0p3rUPC`).
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/1asi0729/2025-20` o en otra de su preferencia.

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
ng new daos-hello-angular-v2520
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

_? Do you want to create a 'zoneless' application without zone.js? (y/N)_, **digitar**:

```
Y
```

_? Which AI tools do you want to configure with Angular best practices? https://angular.dev/ai/develop-with-ai_ `(Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)`, **Seleccionar**:

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
cd daos-hello-angular-v2520
```

**Agregar** Angula material a la aplicación, **ejecute** el siguiente CLI command:

```
ng add @angular/material
```

Despues de ejecutar el CLI command, le mostrará diferentes opciones y debe escoger las siguientes:

_The package @angular/material@20.2.2 will be installed and executed.
Would you like to proceed? (Y/n)_, **digitar**:
```
Y
```

_? Choose a prebuilt theme name, or "custom" for a custom theme:_, **seleccionar**:
```
Azure/Blue   [Preview: https://material.angular.io?theme=azure-blue]
```


### Cambiar el propietario del proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**

**Ejecutar** los siguientes commands:
```
cd ..
```

```
sudo chown -R alumnos ./daos-hello-angular-v2520
```

```
ls -l
```


## Desarrollo del proyecto

**Cargar** el IntelliJ IDEA y **abrir** el proyecto `daos-hello-angular-v2520` ubicado en la carpeta donde la creo.

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
      - 📁 components
      - 📁 model
      - 📁 pages
      - 📁 services
```


### Creación del model developer tipo entity

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `student`:

```bash
ng generate class greetings/model/developer --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Developer` del archivo `developer.entity.ts`, ubicado en la carpeta `/src/app/greetings/model`:

```ts
private readonly _firstName: string;
private readonly _lastName: string;

constructor(firstName?: string, lastName?: string) {
  this._firstName = firstName?.trim() ?? '';
  this._lastName = lastName?.trim() ?? '';
}
get fullName(): string {
  return `${this._firstName} ${this._lastName}`.trim();
}
isEmpty(): boolean {
  return this._firstName === '' && this._lastName === '';
}
```

### Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes: `developer-home`:
```bash
ng generate component greetings/pages/developer-home --skip-tests=true
```

`developer-registration`:
```bash
ng generate component greetings/components/developer-registration --skip-tests=true
```

`developer-greeting`:
```bash
ng generate component greetings/components/developer-greeting --skip-tests=true
```

### Modificación del App Component

**Agregar** los siguientes `import` al archivo `app.ts`, ubicado en la carpeta `/src/app`:

```ts
import { DeveloperHome } from './greetings/pages/developer-home/developer-home';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `AppComponent`, ubicado en el archivo `app.component.ts`

```ts
DeveloperHome
```

**Reemplazar** el contenido del archivo `app.html` con el siguiente código, ubicado en la carpeta `/src/app`:

```html
<app-developer-home/>
<router-outlet />
```

### Información de Angular Signals

**¿Qué son los signals?**: 

Un `signal` es un contenedor que envuelve un valor y notifica a los usuarios interesados ​​cuando este cambia. Los `Signals` pueden contener cualquier valor, desde primitivos hasta estructuras de datos complejas.

El valor de un `signal` se lee llamando a su función getter, lo que permite a Angular rastrear dónde se utiliza. Los `Signals` pueden ser de solo lectura o de escritura.

Más información en: https://angular.dev/guide/signals


### Modificación del DeveloperHome Component

**Agregar** los siguientes `import` al archivo `developer-home.ts`, ubicado en la carpeta `/src/app/greetings/pages/developer-home`:

```ts
import { signal, WritableSignal } from '@angular/core';
import { DeveloperRegistration } from '../../components/developer-registration/developer-registration';
import { DeveloperGreeting } from '../../components/developer-greeting/developer-greeting';
import { Developer } from '../../model/developer.entity';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `DeveloperHome`, ubicado en el archivo `developer-home.ts`

```ts
DeveloperRegistration, DeveloperGreeting
```

**Agregar** los atributos firstName y lastName tipo `WritableSignal` a la clase `DeveloperHome`:

```ts
public developer: WritableSignal<Developer>;
public language: WritableSignal<string>;
```

**Agregar** el siguiente `constructor` a la clase `DeveloperHome`:

```ts
constructor() {
  this.developer = signal(new Developer('Jorge', 'Silva'));
  this.language = signal('TypeScript');
}
```

**Agregar** los siguientes métodos a la clase `DeveloperHome`:

```ts
public updateRegisteredDeveloper(developer: Developer, language: string): void {
  this.developer.set(developer);
  this.language.set(language);
}
public resetRegisteredDeveloper(): void {
  this.developer = signal(new Developer());
  this.language = signal('');
}
```

**Reemplazar** el contenido del archivo `developer-home.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/pages/developer-home`:

```html
<app-developer-registration/>
<app-developer-greeting
  [developer]="developer()"
  [language]="language()"/>
```

**Reemplazar** el contenido del archivo `developer-home.css` con el siguiente código, ubicado en la carpeta `/src/app/greetings/pages/developer-home`:

```css
:host {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

/* Spacing below the registration form */
app-developer-registration {
  margin-bottom: 20px;
}
```

### Modificación del DeveloperGreeting Component

**Agregar** los siguientes `import` al archivo `developer-greeting.ts`, ubicado en la carpeta `/src/app/greetings/components/developer-greeting`:

```ts
import { computed } from '@angular/core';
import { input, InputSignal } from '@angular/core';
import { Developer } from '../../model/developer.entity';
```

**Agregar** los atributos developer y language tipo `InputSignal` en la clase `DeveloperGreeting`:

```ts
public developer: InputSignal<Developer> = input.required<Developer>();
public language =  input.required<string>();
```

**Agregar** los atributos fullName y isRegistered tipo `Signal` en la clase `DeveloperGreeting`:

```ts
readonly fullName: Signal<string> = computed(() => {
  if (this.developer().isEmpty() && this.language().trim() == '')
    return 'Anonymous Developer';
  return `${this.developer().fullName}`;
});

readonly isRegistered = computed(() =>
  !this.developer().isEmpty() && this.language().trim() !== ''
);
```

**Reemplazar** el contenido del archivo `developer-greeting.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/components/developer-greeting`:

```html
<p>Hello {{ fullName() }}.
  <br>
  @if(isRegistered())  {
    <span>Now You are a Developer in </span>
    {{ language() }} Language!
  }
</p>
```

**Reemplazar** el contenido del archivo `developer-greeting.css` con el siguiente código, ubicado en la carpeta `/src/app/greetings/components/developer-greeting`:

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

### Modificación del DeveloperHome Component

**Reemplazar** el `constructor` de la clase `DeveloperHome` con el siguiente código:

```ts
constructor() {
  this.developer = signal(new Developer());
  this.language = signal('');
}
```

**Reemplazar** el contenido del archivo `developer-home.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/pages/developer-home`:

```html
<app-developer-registration
  (registerDeveloper)="updateRegisteredDeveloper($event.developer, $event.language)"
  (dropDeveloper)="resetRegisteredDeveloper()"/>
<app-developer-greeting
  [developer]="developer()"
  [language]="language()"/>
```

### Modificación del DeveloperRegistration Component

**Agregar** los siguientes `import` al archivo `developer-registration.ts`, ubicado en la carpeta `/src/app/greetings/components/developer-registration`:

```ts
import { output, OutputEmitterRef } from '@angular/core';
import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';
import { Developer } from '../../model/developer.entity';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `DeveloperRegistration`:

```ts
ReactiveFormsModule
```

**Agregar** los output registerDeveloper y dropDeveloper tipo `OutputEmitterRef` en la clase `DeveloperRegistration`:

```ts
public registerDeveloper = output<{developer: Developer, language: string}>();
public dropDeveloper: OutputEmitterRef<void> = output<void>();
```

**Agregar** el atributo developerFormGroup tipo `FormGroup` en la clase `DeveloperRegistration`:

```ts
public developerFormGroup = new FormGroup({
  firstNameFC: new FormControl('', [Validators.required, Validators.minLength(2)]),
  lastNameFC: new FormControl('', [Validators.required, Validators.minLength(2)]),
  languageFC: new FormControl('', [Validators.required, Validators.minLength(2)])
});
```

**Agregar** los siguientes métodos a la clase `DeveloperRegistration`:

```ts
public submitRegistrationRequest(): void {
  if (this.developerFormGroup.valid) {
    const firstName = this.developerFormGroup.value.firstNameFC?.valueOf() ?? '';
    const lastName = this.developerFormGroup.value.lastNameFC?.valueOf() ?? '';
    const language = this.developerFormGroup.value.languageFC?.valueOf() ?? '';
    const developer = new Developer(firstName, lastName);

    this.registerDeveloper.emit({ developer, language });
    this.developerFormGroup.reset();
  }
}

public dropRegistration(): void {
  this.dropDeveloper.emit();
  this.developerFormGroup.reset();
}

public clearForm(): void {
  this.developerFormGroup.reset();
}
```

**Reemplazar** el contenido del archivo `developer-registration.html` con el siguiente código, ubicado en la carpeta `/src/app/greetings/components/developer-registration`:

```html
<h1>New Developer</h1>

<!-- Form to input developer details, submits on Register -->
<form [formGroup]="developerFormGroup" (ngSubmit)="submitRegistrationRequest()">
  <!-- First Name input with label -->
  <label for="first-name">First Name: </label>
  <input id="first-name" type="text" formControlName="firstNameFC">
    <!-- Validation errors for firstName -->
    @if (developerFormGroup.get('firstNameFC')?.touched && developerFormGroup.get('firstNameFC')?.invalid) {
      @if (developerFormGroup.get('firstNameFC')?.errors?.['required']) {
        <div class="error">First name is required.</div>
      }
      @if (developerFormGroup.get('firstNameFC')?.errors?.['minlength']) {
        <div class="error">First name must be at least 2 characters.</div>
      }
    }

  <!-- Last Name input with label -->
  <label for="last-name">Last Name: </label>
  <input id="last-name" type="text" formControlName="lastNameFC">
    <!-- Validation errors for lastName -->
    @if (developerFormGroup.get('lastNameFC')?.touched && developerFormGroup.get('lastNameFC')?.invalid) {
      @if (developerFormGroup.get('lastNameFC')?.errors?.['required']) {
        <div class="error">Last name is required.</div>
      }
      @if (developerFormGroup.get('lastNameFC')?.errors?.['minlength']) {
        <div class="error">Last name must be at least 2 characters.</div>
      }
    }

  <!-- Language input with label -->
  <label for="language">Programming Language : </label>
  <input id="language" type="text" formControlName="languageFC">
    <!-- Validation errors for language -->
    @if (developerFormGroup.get('languageFC')?.touched && developerFormGroup.get('languageFC')?.invalid) {
      @if (developerFormGroup.get('languageFC')?.errors?.['required']) {
        <div class="error">Language is required.</div>
      }
      @if (developerFormGroup.get('languageFC')?.errors?.['minlength']) {
        <div class="error">Language must be at least 2 characters.</div>
      }
    }

  <!-- Button group for form actions -->
  <div class="button-group">
    <button type="submit" [disabled]="developerFormGroup.invalid">Register</button>
    <button type="button" (click)="dropRegistration()">Unregister</button>
    <button type="button" (click)="clearForm()">Clear Form</button>
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
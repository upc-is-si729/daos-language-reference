# Learning Center Project - Identity and Access Management (IAM) Bounded Context

## Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 iam
      - 📁 components
      - 📁 model
      - 📁 pages
      - 📁 services
```

## Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes: `authentication-section`, `sign-in`, `sign-up` (_Ejecute los comandos uno a la vez_):

```bash
ng generate component iam/components/authentication-section --skip-tests=true
ng generate component iam/pages/sign-in --skip-tests=true
ng generate component iam/pages/sign-up --skip-tests=true
```

## Creación del model sign-in y sign-up tipo request y response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear los modelos `sign-in` y `sign-up`:

```bash
ng generate class iam/model/sign-in --type=request --skip-tests=true
ng generate class iam/model/sign-in --type=response --skip-tests=true
ng generate class iam/model/sign-up --type=request --skip-tests=true
ng generate class iam/model/sign-up --type=response --skip-tests=true
```

**Reemplazar** el contenido del archivo `sign-in.request.ts` ubicado en la carpeta `/src/app/iam/model`, con la siguiente clase `SignInRequest`:

```ts
export class SignInRequest {
  public username: string;
  public password: string;

  constructor(username: string, password: string) {
    this.password = password;
    this.username = username;
  }
}
```

**Reemplazar** el contenido del archivo `sign-in.response.ts` ubicado en la carpeta `/src/app/iam/model`, con la siguiente clase `SignInResponse` :

```ts
export class SignInResponse {
  public id: number;
  public username: string;
  public token: string;

  constructor(id: number, username: string, token: string) {
    this.token = token;
    this.username = username;
    this.id = id;
  }
}
```

**Reemplazar** el contenido del archivo `sign-up.request.ts` ubicado en la carpeta `/src/app/iam/model`, con la clase `SignUpRequest`:

```ts
export class SignUpRequest {
  public username: string;
  public password: string;

  constructor(username: string, password: string) {
    this.password = password;
    this.username = username;
  }
}
```

**Reemplazar** el contenido del archivo `sign-up.response.ts`, ubicado en la carpeta `/src/app/iam/model`, con la clase `SignUpResponse` :

```ts
export class SignUpResponse {
  public id: number;
  public username: string;

  constructor(id: number, username: string) {
    this.username = username;
    this.id = id;
  }
}
```


## Creación del Service authentication

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `authentication`:

```bash
ng generate service iam/services/authentication --skip-tests=true
```

**Reemplazar** el contenido del archivo `autehntication.service.ts` ubicado en la carpeta `/src/app/iam/services`, con el siguiente código de la clase `AuthenticationService`:

```ts
import { Injectable } from '@angular/core';
import {environment} from "../../../environments/environment";
import {HttpClient, HttpHeaders} from "@angular/common/http";
import {BehaviorSubject} from "rxjs";
import {Router} from "@angular/router";
import {SignInRequest} from "../model/sign-in.request";
import {SignInResponse} from "../model/sign-in.response";
import {SignUpRequest} from "../model/sign-up.request";
import {SignUpResponse} from "../model/sign-up.response";

@Injectable({
  providedIn: 'root'
})
export class AuthenticationService {

  basePath: string = `${environment.serverBasePath}`;
  httpOptions = { headers: new HttpHeaders({'Content-type': 'application/json'}) };

  private signedIn: BehaviorSubject<boolean> = new BehaviorSubject<boolean>(false);
  private signedInUserId: BehaviorSubject<number> = new BehaviorSubject<number>(0);
  private signedInUsername: BehaviorSubject<string> = new BehaviorSubject<string>('');

  constructor(private router: Router, private http: HttpClient) { }

  get isSignedIn() {
    return this.signedIn.asObservable();
  }

  get currentUserId() {
    return this.signedInUserId.asObservable();
  }

  get currentUsername() {
    return this.signedInUsername.asObservable();
  }

  /**
   * Sign up a new user.
   * @param signUpRequest The sign up request.
   * @returns The sign up response.
   */
  signUp(signUpRequest: SignUpRequest) {
    return this.http.post<SignUpResponse>(`${this.basePath}/authentication/sign-up`, signUpRequest, this.httpOptions)
      .subscribe({
        next: (response) => {
          console.log(`Signed up as ${response.username} with id: ${response.id}`);
          this.router.navigate(['/sign-in']).then();
        },
        error: (error) => {
          console.error(`Error while signing up: ${error}`);
          this.router.navigate(['/sign-up']).then();
        }
      });
  }

  /**
   * Sign in a user.
   * @param signInRequest The sign in request.
   * @returns The sign in response.
   */
  signIn(signInRequest: SignInRequest) {
    console.log(signInRequest);
    return this.http.post<SignInResponse>(`${this.basePath}/authentication/sign-in`, signInRequest, this.httpOptions)
      .subscribe({
        next: (response) => {
          this.signedIn.next(true);
          this.signedInUserId.next(response.id);
          this.signedInUsername.next(response.username);
          localStorage.setItem('token', response.token);
          console.log(`Signed in as ${response.username} with token ${response.token}`);
          this.router.navigate(['/']).then();
        },
        error: (error) => {
          this.signedIn.next(false);
          this.signedInUserId.next(0);
          this.signedInUsername.next('');
          console.error(`Error while signing in: ${error}`);
          this.router.navigate(['/sign-in']).then();
        }
      });
  }

  /**
   * Sign out a user.
   *
   * This method signs out a user by clearing the token from local storage and navigating to the sign-in page.
   */
  signOut() {
    this.signedIn.next(false);
    this.signedInUserId.next(0);
    this.signedInUsername.next('');
    localStorage.removeItem('token');
    this.router.navigate(['/sign-in']).then();
  }
}
```

## Creación del guard authentication

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el guard `authentication`:

```bash
ng generate guard iam/services/authentication --skip-tests=true
```

**Reemplazar** el contenido del archivo `authentication.guard.ts` ubicado en la carpeta `/src/app/iam/services`, con el siguiente código del const `authenticationGuard`:

```ts
import {CanActivateFn, Router} from '@angular/router';
import {inject} from "@angular/core";
import {AuthenticationService} from "./authentication.service";
import {map, take} from "rxjs";

export const authenticationGuard: CanActivateFn = () => {
  const authenticationService = inject(AuthenticationService);
  const router = inject(Router);
  
  return authenticationService.isSignedIn.pipe(
    take(1), map(isSignedIn => {
      if (isSignedIn)
        return true;
      else {
        router.navigate(['/sign-in']).then();
        return false;
      }
    })
  );
};
```

## Creación del interceptor authentication

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el interceptor `authentication`:

```bash
ng generate interceptor iam/services/authentication --skip-tests=true
```

**Reemplazar** el contenido del archivo `authentication.interceptor.ts` ubicado en la carpeta `/src/app/iam/services`, con el siguiente código del const `authenticationInterceptor`:

```ts
import {HttpEvent, HttpHandlerFn, HttpInterceptorFn, HttpRequest} from '@angular/common/http';
import {Observable} from "rxjs";

export const authenticationInterceptor: HttpInterceptorFn = ( request: HttpRequest<any>, next: HttpHandlerFn): Observable<HttpEvent<unknown>> => {
  const token = localStorage.getItem('token');
  const handledRequest = token
    ? request.clone({ headers: request.headers.set('Authorization', `Bearer ${token}`)} )
    : request;
  console.log(handledRequest);
  return next(handledRequest);
}
```

## Configuración del Routes

**Agregar** los siguientes `import` al archivo `app.routes.ts` ubicado en la carpeta `/src/app`:

```ts
import {authenticationGuard} from "./iam/services/authentication.guard";
import {SignInComponent} from "./iam/pages/sign-in/sign-in.component";
import {SignUpComponent} from "./iam/pages/sign-up/sign-up.component";
```

**Agregar** los siguientes valores a la constante `routes` del archivo `app.routes.ts`:

```ts
  { path: 'sign-in', component: SignInComponent },
  { path: 'sign-up', component: SignUpComponent},
```

**Agregar** la propiedad `canActivate` con el valor `authenticationGuard` al path `learning/students`, quedando el path de la siguiente manera:

```ts
  { path: 'learning/students', component: StudentManagementComponent, canActivate: [authenticationGuard] },
```


## Creación del component base: BaseFormComponent

**Crear** la carpeta `components` dentro de la carpeta `/src/app/shared`.

**Crear** el archivo `base-form.component.ts` dentro de la carpeta `components` con el siguiente código de la clase `BaseFormComponent`:

```ts
import {FormGroup} from "@angular/forms";

export class BaseFormComponent {

  /**
   * Check if a control is invalid
   * @param form The form
   * @param controlName The control name
   * @returns True if the control is invalid, false otherwise
   * @protected
   */
  protected isInvalidControl(form: FormGroup, controlName: string) {
    return form.controls[controlName].invalid && form.controls[controlName].touched;
  }

  /**
   * Get the error messages for a control
   * @param form The form
   * @param controlName The control name
   * @returns The error messages
   * @protected
   */
  protected errorMessagesForControl(form: FormGroup, controlName: string) {
    const control = form.controls[controlName];
    let errorMessages = "";
    let errors = control.errors;
    if (!errors)
      return errorMessages;
    Object.keys(errors).forEach((errorKey) =>
      errorMessages += this.errorMessageForControl(controlName, errorKey)
    );
    return errorMessages;
  }

  /**
   * Get the error message for a control
   * @param controlName The control name
   * @param errorKey The error key
   * @returns The error message
   * @private
   */
  private errorMessageForControl(controlName: string, errorKey: string) {
    switch (errorKey) {
      case 'required':
        return `The field ${controlName} is required`;
      default:
        return `The field ${controlName}is invalid`;
    }
  }
}

```

## Modificación de componentes

### Modificación del componente: AuthenticationSectionComponent

**Reemplazar** el contenido del archivo `authentication-section.component.ts` ubicado en la carpeta `/src/app/iam/components/authentication-section` con el siguiente código de la clase `AuthenticationSectionComponent` :

```ts
import { Component } from '@angular/core';
import {Router} from "@angular/router";
import {AuthenticationService} from "../../services/authentication.service";
import {NgIf} from "@angular/common";
import {MatButton} from "@angular/material/button";

@Component({
  selector: 'app-authentication-section',
  standalone: true,
  imports: [
    NgIf,
    MatButton
  ],
  templateUrl: './authentication-section.component.html',
  styleUrl: './authentication-section.component.css'
})
export class AuthenticationSectionComponent {

  currentUserName: string = '';
  isSignedIn: boolean = false;

  constructor(private router: Router, private authenticationService: AuthenticationService) {
    this.authenticationService.currentUsername.subscribe(
      (username) => this.currentUserName = username
    );
    this.authenticationService.isSignedIn.subscribe(
      (isSignedIn) => this.isSignedIn = isSignedIn
    );
  }

  /**
   * Event Handler for the sign-in button.
   */
  onSignIn() {
    // Navigate to the sign-in page.
    this.router.navigate(['/sign-in']).then();
  }

  /**
   * Event Handler for the sign-up button.
   */
  onSignUp() {
    // Navigate to the sign-up page.
    this.router.navigate(['/sign-up']).then();
  }

  /**
   * Event Handler for the sign-out button.
   */
  onSignOut() {
    // Sign out the user.
    this.authenticationService.signOut();
  }
}
```

**Reemplazar** el contenido del archivo `authentication-section.component.html`, ubicado en la carpeta `/src/app/iam/components/authentication-section` con el siguiente código:

```html
<!-- Authentication Section information content depends on signed-in status -->
<div *ngIf="isSignedIn; then authenticated else anonymous"></div>
<!-- Authenticated Scenario  Information and Available Actions -->
<ng-template #authenticated>
  <span class="mat-body"> Welcome, {{ currentUserName }}</span> | <button mat-button (click)="onSignOut()">Sign Out</button>
</ng-template>
<!-- Anonymous Scenario Available Actions -->
<ng-template #anonymous>
  <button mat-button (click)="onSignIn()">Sign In</button> | <button mat-button (click)="onSignUp()">Sign Up</button>
</ng-template>
```

### Modificación del componente: SignUpComponent

**Reemplazar** el contenido del archivo `sign-up.component.ts` ubicado en la carpeta `/src/app/iam/pages/sign-up` con el siguiente código de la clase `SignUpComponent`:

```ts
import {Component, OnInit} from '@angular/core';
import {BaseFormComponent} from "../../../shared/components/base-form.component";
import {FormBuilder, FormGroup, ReactiveFormsModule, Validators} from "@angular/forms";
import {AuthenticationService} from "../../services/authentication.service";
import {SignUpRequest} from "../../model/sign-up.request";
import {MatCard, MatCardContent, MatCardHeader, MatCardTitle} from "@angular/material/card";
import {MatError, MatFormField} from "@angular/material/form-field";
import {MatInput} from "@angular/material/input";
import {MatButton} from "@angular/material/button";
import {NgIf} from "@angular/common";

@Component({
  selector: 'app-sign-up',
  standalone: true,
  imports: [
    MatCard,
    MatCardHeader,
    MatCardContent,
    MatFormField,
    ReactiveFormsModule,
    MatInput,
    MatButton,
    MatCardTitle,
    MatError,
    NgIf
  ],
  templateUrl: './sign-up.component.html',
  styleUrl: './sign-up.component.css'
})
export class SignUpComponent extends BaseFormComponent implements OnInit {

  form!: FormGroup;
  submitted = false;

  constructor(private builder: FormBuilder, private authenticationService: AuthenticationService) {
    super();
  }

  ngOnInit(): void {
    this.form = this.builder.group({
      username: ['', Validators.required],
      password: ['', Validators.required]
    });
  }

  onSubmit() {
    if (this.form.invalid) return;
    let username = this.form.value.username;
    let password = this.form.value.password;
    const signUpRequest = new SignUpRequest(username, password);
    this.authenticationService.signUp(signUpRequest);
    this.submitted = true;
  }
}
```

**Reemplazar** el contenido del archivo `sign-up.component.html` ubicado en la carpeta `/src/app/iam/pages/sign-up` con el siguiente código:

```ts
<div class="sign-up-content">
  <mat-card>
    <mat-card-header>
      <mat-card-title>Sign Up</mat-card-title>
    </mat-card-header>
    <mat-card-content>
      <form [formGroup]="form" (submit)="onSubmit()">
        <mat-form-field>
          <input matInput placeholder="Username" id="username" name="username" required formControlName="username">
          <mat-error *ngIf="isInvalidControl(form,'username')">{{ errorMessagesForControl(form,'username')}}</mat-error>
        </mat-form-field>
        <mat-form-field>
          <input matInput type="password" placeholder="Password" id="password" name="password" formControlName="password">
          <mat-error *ngIf="isInvalidControl(form,'password')">{{ errorMessagesForControl(form,'password')}}</mat-error>
        </mat-form-field>
        <button mat-raised-button color="primary">Sign Up</button>
      </form>
    </mat-card-content>
  </mat-card>
</div>
```


**Reemplazar** el contenido del archivo `sign-up.component.css` ubicado en la carpeta `/src/app/iam/pages/sign-up` con el siguiente código:

```css
.sign-up-content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
```

### Modificación del componente: SignInComponent

**Reemplazar** el contenido del archivo `sign-in.component.ts` ubicado en la carpeta `/src/app/iam/pages/sign-in` con el siguiente código de la clase `SignInComponent`:

```ts
import {Component, OnInit} from '@angular/core';
import {FormBuilder, FormGroup, ReactiveFormsModule, Validators} from "@angular/forms";
import {AuthenticationService} from "../../services/authentication.service";
import {BaseFormComponent} from "../../../shared/components/base-form.component";
import {SignInRequest} from "../../model/sign-in.request";
import {MatCard, MatCardContent, MatCardHeader, MatCardTitle} from "@angular/material/card";
import {MatError, MatFormField} from "@angular/material/form-field";
import {MatInput} from "@angular/material/input";
import {MatButton} from "@angular/material/button";
import {NgIf} from "@angular/common";

@Component({
  selector: 'app-sign-in',
  standalone: true,
  imports: [
    MatCard,
    MatCardHeader,
    MatCardContent,
    MatFormField,
    ReactiveFormsModule,
    MatInput,
    MatButton,
    MatCardTitle,
    MatError,
    NgIf
  ],
  templateUrl: './sign-in.component.html',
  styleUrl: './sign-in.component.css'
})
export class SignInComponent extends BaseFormComponent implements OnInit {

  form!: FormGroup;
  submitted = false;

  constructor(private builder: FormBuilder, private authenticationService: AuthenticationService) {
    super();
  }

  ngOnInit(): void {
    this.form = this.builder.group({
      username: ['', Validators.required],
      password: ['', Validators.required]
    });
  }

  onSubmit() {
    if (this.form.invalid)
      return;
    let username = this.form.value.username;
    let password = this.form.value.password;
    const signInRequest = new SignInRequest(username, password);
    this.authenticationService.signIn(signInRequest);
    this.submitted = true;
  }
}

```

**Reemplazar** el contenido del archivo `sign-in.component.html` ubicado en la carpeta `/src/app/iam/pages/sign-in` con el siguiente código:

```ts
<div class="sign-in-content">
  <mat-card>
    <mat-card-header>
      <mat-card-title>Sign In</mat-card-title>
    </mat-card-header>
    <mat-card-content>
      <form [formGroup]="form" (submit)="onSubmit()">
        <mat-form-field>
          <input matInput placeholder="Username" id="username" name="username" required formControlName="username">
          <mat-error *ngIf="isInvalidControl(form,'username')">{{ errorMessagesForControl(form,'username')}}</mat-error>
        </mat-form-field>
        <mat-form-field>
          <input matInput type="password" placeholder="Password" id="password" name="password" required formControlName="password">
          <mat-error *ngIf="isInvalidControl(form,'password')">{{ errorMessagesForControl(form,'password')}}</mat-error>
        </mat-form-field>
        <button mat-raised-button color="primary">Sign In</button>
      </form>
    </mat-card-content>
  </mat-card>
</div>
```

**Reemplazar** el contenido del archivo `sign-in.component.css` ubicado en la carpeta `/src/app/iam/pages/sign-in` con el siguiente código:

```css
.sign-in-content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
```

### Modificar el AppComponent

**Agregar** el siguiente `import` al archivo `app.component.ts`, ubicado en la carpeta `/src/app`:

```ts
import { AuthenticationSectionComponent } from "./iam/components/authentication-section/authentication-section.component";
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `AppComponent`, ubicado en el archivo `app.component.ts`

```ts
AuthenticationSectionComponent
```

**Agregar** el siguiente código despues del `for` de los options en el archivo: `app.component.html`:

```ts
| <app-authentication-section/>
```

## Configuración del appConfig

A continuación se detalla las instrucciones para agregar el `authenticationInterceptor` al config.

**Agregar** el siguiente import al archivo `app.config.ts` ubicado en la carpeta `/src/app`:

```ts
import { withInterceptors } from "@angular/common/http";
import { authenticationInterceptor } from "./iam/services/authentication.interceptor";
```

**Agregar** la function `withInterceptors([authenticationInterceptor])` como parámetro del `provideHttpClient`, tal como se muestra a continuación:

```ts
provideHttpClient( withInterceptors([authenticationInterceptor]) ),
```



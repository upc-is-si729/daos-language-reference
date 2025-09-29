# Learning Center Project

## Creación del proyecto

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y `chown`, y luego ingresar la contraseña del Administrador (`d3v3l0p3rUPC`).
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/1asi0729/2025-10` o en otra de su preferencia.

> [!CAUTION]
> **En el caso de estar en un equipo Windows:**
> - Debe ubicarse en la carpeta `IdeaProjects/` o en otra de su preferencia.

A continuación se detalla las intrucciones para crear un nuevo `workspace` e `initial starter app` de Angular. Más información en: https://angular.dev/tools/cli/setup-local

### Creación de un workspace y un initial application

**Cargar** el `Terminal` del sistema Operativo, ubicarse en la carpeta de su preferencia de acuerdo al Sistema Operativo y **ejecutar** el siguiente CLI command:

```bash
ng new daos-1asi0729-learning-center
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
cd daos-1asi0729-learning-center
```

**Agregar** Angula material a la aplicación, **ejecute** el siguiente CLI command:

```
ng add @angular/material
```

Despues de ejecutar el CLI command, le mostrará diferentes opciones y debe escoger las siguientes:

_Would you like to proceed?_, **digitar**:
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

### Instalando Internationalization en/es

A continuación se detalla las intrucciones para instalar los libraries de internacionalización (i18n) para Angular: **@ngx-translate**. Más información en: https://github.com/ngx-translate/core

**Ejecutar** el siguiente command:

```
npm install @ngx-translate/core @ngx-translate/http-loader --save
```

### Instalando Json-sever

A continuación se detalla las intrucciones para instalar un `full fake REST API` sin codificación denominado: **JSON Server**. Más información en: https://github.com/typicode/json-server/tree/v0

**Ejecutar** el siguiente command:

```
npm install -g json-server@0.17.4
```

### Cambiar el propietario del proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**


**Ejecutar** los siguientes commands:
```
cd ..
```

```
sudo chown -R alumnos ./daos-1asi0729-learning-center
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

### Creación de los Archivos de idiomas

**Crear** las carpetas: `assets` e `i18n` en la carpeta :file_folder: `public` ubicado en la raiz del proyecto:

```markdown
- 📂 public
  - 📁 assets
    - 📁 i18n
```

**Crear** los archivos `en.json` y `es.json` en la carpeta :file_folder: `i18n` con el siguiente contenido:

#### en.json
```json
{
  "filter": {
    "label": "Filter",
    "placeholder": "Enter text to filter"
  },
  "title": {
    "students": "Students",
    "add": "New Student",
    "edit": "Edit Student"
  },
  "student": {
    "id": "No.",
    "name": "Name",
    "age": "Age",
    "address": "Address",
    "actions": "Actions"
  },
  "footer": {
    "rights": "Copyright © 2025 {{ api }}, All rights reserved",
    "intro": "Developed ",
    "author": "by {{ team }} Team"
  }
}

```

#### es.json
```json
{
  "filter": {
    "label": "Filtrar",
    "placeholder": "Digite texto a filtrar"
  },
  "title": {
    "students": "Estudiantes",
    "add": "Nuevo estudiante",
    "edit": "Editar estudiante"
  },
  "student": {
    "id": "Núm.",
    "name": "Nombre",
    "age": "Edad",
    "address": "Dirección",
    "actions": "Acciones"
  },
  "footer": {
    "rights": "Derechos de autor © 2025 {{ api }}, Todos los derechos reservados",
    "intro": "Desarrollado ",
    "author": "por el Equipo {{ team }}"
  }
}

```

### Configuración de JSON-Server

**Crear** la carpeta `server` en la carpeta raiz del proyecto:

```markdown
- 📂 daos-1asi0729-learning-center
  - 📁 server
```

**Crear** los archivo `db.json` y `routes.json` en la carpeta `server` con el siguiente contenido:

#### db.json
```json
{
  "students": [
    {
      "id": 1,
      "name": "John Doe",
      "age": 26,
      "address": "Nowhere"
    },
    {
      "id": 2,
      "name": "Jason Bourne",
      "age": 34,
      "address": "Unknown"
    },
    {
      "id": 3,
      "name": "William Norton",
      "age": 38,
      "address": "One Secret Place"
    }
  ]
}
```

#### routes.json
```json
{
  "/api/v1/*": "/$1"
}
```

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** el siguiente command para iniciar el `json-server`:

```
json-server --watch server/db.json --routes server/routes.json
```

**Cargar** el navegador e **ingrese** la siguiente Url: http://localhost:3000/api/v1/students



### Configuración de environments

A continuación se detalla las intrucciones para definir diferentes `named build configurations`  para el proyecto, como `development` y `staging` con diferentes valores predeterminados. Más información en: https://angular.dev/tools/cli/environments#configure-environment-specific-defaults

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** el siguiente CLI command para crear el directorio `environments` y archivos de configuración, ubicado en la carpeta `src`:

```bash
ng generate environments
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.development.ts` ubicado en la carpeta `/src/environments`:

```ts
production: false,
// Server Base Path for Fake REST API
serverBasePath: 'http://localhost:3000/api/v1',
// Server Base Path for Spring Boot REST API
//serverBasePath: 'http://localhost:8090/api/v1',
studentsEndpointPath: '/students'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
// Server Base Path for Fake REST API
serverBasePath: 'http://localhost:3000/api/v1',
// Server Base Path for Spring Boot REST API
//serverBasePath: 'http://localhost:8090/api/v1',
studentsEndpointPath: '/students'
```

### Información del HttpClient y provideHttpClient

**HttpClient**: 

Realiza HTTP requests. Este servicio está disponible como un injectable class, con métodos para realizar HTTP requests. Cada request method tiene múltiples signatures y el return type varía según el signatures que el llamado (principalmente los valores de observe y responseType).

Más información en: https://angular.dev/api/common/http/HttpClient

**provideHttpClient**:

Configura el servicio HttpClient de Angular para que esté disponible para injection.

Más información en: https://angular.dev/api/common/http/provideHttpClient


### Configuración del appConfig

A continuación se detalla las instrucciones para agregar los providers: `provideHttpClient` y `provideTranslateService` al config.

**Agregar** los siguientes imports al archivo `app.config.ts` ubicado en la carpeta `/src/app`:

```ts
import { HttpClient, provideHttpClient } from "@angular/common/http";
import { provideTranslateService, TranslateLoader } from "@ngx-translate/core";
import { TranslateHttpLoader } from "@ngx-translate/http-loader";
```

**Agregar** la const `httpLoaderFactory` despues de los `imports`.

```ts
const httpLoaderFactory: (http: HttpClient) =>
  TranslateLoader = (http: HttpClient) =>
  new TranslateHttpLoader(http, './assets/i18n/', '.json');
```

**Agregar** los siguientes métodos al array `providers` de la constante `appConfig` del archivo `app.config.ts` :
```ts
provideHttpClient(),
provideTranslateService({
  loader: {
    provide: TranslateLoader,
    useFactory: httpLoaderFactory,
    deps: [HttpClient]
  },
  defaultLanguage: 'en',
})
```

### Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 learning
      - 📁 components
      - 📁 model
      - 📁 pages
      - 📁 services
    - 📂 public
      - 📁 components
      - 📁 pages
    - 📂 shared
      - 📁 services
```

### Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes: `student-create-and-edit`, `student-management`, `about`, `home`, `language-switcher` y `page-not-found` (Ejecute los comandos uno a la vez):
```bash
ng generate component learning/components/student-create-and-edit --skip-tests=true
ng generate component learning/pages/student-management --skip-tests=true
ng generate component public/components/language-switcher --skip-tests=true
ng generate component public/pages/about --skip-tests=true
ng generate component public/pages/home --skip-tests=true
ng generate component public/pages/page-not-found --skip-tests=true
```

### Configuración del Routes

**Agregar** los siguientes `import` al archivo `app.routes.ts` ubicado en la carpeta `/src/app`:

```ts
import { HomeComponent } from "./public/pages/home/home.component";
import { AboutComponent } from "./public/pages/about/about.component";
import { PageNotFoundComponent } from "./public/pages/page-not-found/page-not-found.component";
import { StudentManagementComponent } from "./learning/pages/student-management/student-management.component";
```

**Agregar** los siguientes valores a la constante `routes` del archivo `app.routes.ts`:

```ts
{ path: 'home', component: HomeComponent },
{ path: 'about', component: AboutComponent },
{ path: 'learning/students', component: StudentManagementComponent },
{ path: '', redirectTo: 'home', pathMatch: 'full' },
{ path: '**', component: PageNotFoundComponent }
```

## Modificación del LanguageSwitcherComponent

**Agregar** los siguientes `import` al archivo `language-switcher.component.ts`, ubicado en la carpeta `/src/app/public/components/language-switcher`:

```ts
import { TranslateService } from "@ngx-translate/core";
import { MatButtonToggleModule } from '@angular/material/button-toggle';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `LanguageSwitcherComponent` ubicado en el archivo `language-switcher.component.ts`.

```
MatButtonToggleModule
```

**Reemplazar** el contenido de la clase `LanguageSwitcherComponent` con el siguiente código, ubicado en el archivo `language-switcher.component.ts`:

```ts
currentLang: string = 'en';
languages: string[] = ['en', 'es'];

constructor(private translate: TranslateService) {
  this.currentLang = translate.currentLang;
}

useLanguage(language: string) : void {
  this.translate.use(language);
}
```
**Reemplazar** el contenido del archivo `language-switcher.component.html` con el siguiente código, ubicado en la carpeta `/src/app/public/components/language-switcher`:

```html
<mat-button-toggle-group [value]="currentLang" appearance="standard" aria-label="Preferred Language" name="language">
  @for (language of languages; track language) {
    <mat-button-toggle (click)="useLanguage(language)"
                       [aria-label]="language"
                       [value]="language">{{ language.toUpperCase() }}
    </mat-button-toggle>
  }
</mat-button-toggle-group>
```

### Modificación del AppComponent

**Agregar** los siguientes `import` al archivo `app.component.ts`, ubicado en la carpeta `/src/app`:

```ts
import { OnInit, ViewChild } from '@angular/core';
import { RouterLink } from '@angular/router';
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatSidenav, MatSidenavModule } from '@angular/material/sidenav';
import { MatDividerModule } from "@angular/material/divider";
import { MatListModule } from "@angular/material/list";
import { BreakpointObserver } from "@angular/cdk/layout";
import { TranslateService } from "@ngx-translate/core";
import { LanguageSwitcherComponent } from "./public/components/language-switcher/language-switcher.component";
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `AppComponent`, ubicado en el archivo `app.component.ts`

```ts
RouterLink, MatToolbarModule, MatButtonModule, MatIconModule, 
MatSidenavModule, MatDividerModule, MatListModule, LanguageSwitcherComponent
```

**Aplicar** implementación de la interface `OnInit` a la clase `AppComponent`, agregando la siguiente instrucción a la clase `AppComponent`:

```ts
implements OnInit 
```

**Agregar** los atributos `sidenav` y `options` a la clase `AppComponent`, ubicado en el archivo `app.component.ts`:

```ts
@ViewChild(MatSidenav, {static: true}) sidenav!: MatSidenav;
options = [
  { icon: 'home', path: '/home', title: 'Home'},
  { icon: 'person', path: '/learning/students', title: 'Students'},
  { icon: 'info', path:'/about', title: 'About'}
];
```

**Agregar** el siguiente `constructor` a la clase `AppComponent`, ubicado en el archivo `app.component.ts`:

```ts
constructor(private translate: TranslateService, private observer: BreakpointObserver) {
  translate.setDefaultLang('en');
  translate.use('en');
}
```

**Agregar** el método `ngOnInit()` a la clase `AppComponent`, ubicado en el archivo `app.component.ts`:

```ts
ngOnInit(): void {
  this.observer.observe(['(max-width: 1280px)']) // Observa el ancho de la pantalla
    .subscribe((response) => {  // Se suscribe a los cambios en el ancho de la pantalla
      if (response.matches) { // Si el ancho de la pantalla es menor a 1280px
        this.sidenav.mode = 'over'; // Se despliega sobre el contenido
        this.sidenav.close(); // Se cierra
      } else {
        this.sidenav.mode = 'side'; // Se despliega al lado del contenido
        this.sidenav.open();  // Se abre
      }
    });
}
```

**Reemplazar** el contenido del archivo `app.component.html` con el siguiente código, ubicado en la carpeta `/src/app`:

```html
<mat-toolbar color="primary">
  @if (sidenav.mode === 'over') {
    <button mat-icon-button (click)="sidenav.toggle()" >
      @if (sidenav.opened) {
        <mat-icon>close</mat-icon>
      } @else {
        <mat-icon>menu</mat-icon>
      }
    </button>
  }
  <mat-icon matListItemIcon>rocket_launch</mat-icon>
  <span>Learning Center</span>
  <span class="mat-spacer"></span>
  @for(option of options; track option.title) {
    <a mat-button [routerLink]="option.path">{{ option.title }}</a>
  }
  <button mat-icon-button>
    <mat-icon>share</mat-icon>
  </button>
  <span>
    <app-language-switcher/>
  </span>
</mat-toolbar>

<mat-sidenav-container>
  <mat-sidenav>
    <mat-nav-list>
      @for(option of options; track option.title) {
        <mat-list-item (click)="sidenav.mode === 'over' && sidenav.toggle()" [routerLink]="option.path">
          <mat-icon matListItemIcon>{{ option.icon }}</mat-icon>
          {{ option.title }}
        </mat-list-item>
      }
    </mat-nav-list>
  </mat-sidenav>
  <mat-sidenav-content>
    <div class="content">
      <router-outlet />
    </div>
  </mat-sidenav-content>
</mat-sidenav-container>

```

**Reemplazar** el contenido del archivo `app.component.css` con el siguiente código, ubicado en la carpeta `/src/app`:

```css
.mat-spacer {
  flex: 1 1 auto;
}
mat-sidenav {
  width: 200px;
  border-right: none;
  padding: 16px 0;
}
.content {
  height: calc(100svh - 98px);
  border-radius: 10px;
  padding: 16px;
  max-width: 1520px;
  margin: 0 auto;
  font-size: 16px;
  background: #fff;
}
mat-sidenav-container {
  height: calc(100svh - 65px);
}

```

### Analizando el endpoint de students

Dirijase al endpoint: http://localhost:3000/api/v1/students y **evaluar** el json de respuesta:

```json
{
  "id": 1,
  "name": "John Doe",
  "age": 26,
  "address": "Nowhere"
}
```

### Creación del model student tipo entity

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `student`:

```bash
ng generate class learning/model/student --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Student` del archivo `estudent.entity.ts`, ubicado en la carpeta `/src/app/learning/model`:

```ts
id: number;
name: string;
age: number;
address: string;
constructor() {
  this.id = 0;
  this.name = "";
  this.age = 0;
  this.address = "";
}
```

### Creación del Service Base

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `base`:

```bash
ng generate service shared/services/base --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `base.service.ts`, ubicado en la carpeta `/src/app/shared/services`:
```typescript
import { environment } from "../../../environments/environment";
import { HttpClient, HttpErrorResponse, HttpHeaders } from "@angular/common/http";
import { catchError, Observable, retry, throwError } from "rxjs";
```

**Reemplazar** el nombre de la clase `BaseService` del archivo `base.service.ts` por:

```ts
BaseService<T>
```

**Reemplazar** el contenido de la clase `BaseService` con el siguiente código, ubicado en el archivo `base.service.ts`:

```ts
  basePath: string = `${environment.serverBasePath}`;
  resourceEndpoint: string = '/resources';

  httpOptions = {
    headers: new HttpHeaders({
      'Content-type': 'application/json',
    })
  }

  constructor(private http: HttpClient) {  }

  handleError(error: HttpErrorResponse) {
    // Default error handling
    if (error.error instanceof ErrorEvent) {
      console.log(`An error occurred ${error.error.message}`);
    } else {
      // Unsuccessful Response Error Code returned from Backend
      console.log(`Backend returned code ${error.status}, body was ${error.error}`);
    }
    return throwError(() => new Error('Something happened with request, please try again later'));
  }

  // Create Resource
  create(item: any): Observable<T> {
    return this.http.post<T>(this.resourcePath(), JSON.stringify(item), this.httpOptions)
      .pipe(retry(2), catchError(this.handleError));
  }

  // Delete Resource
  delete(id: any) {
    return this.http.delete(`${this.resourcePath()}/${id}`, this.httpOptions)
      .pipe(retry(2), catchError(this.handleError));
  }

  // Update Resource
  update(id: any, item: any): Observable<T> {
    return this.http.put<T>(`${this.resourcePath()}/${id}`, JSON.stringify(item), this.httpOptions)
      .pipe(retry(2), catchError(this.handleError));
  }

  // Get All Resources
  getAll(): Observable<T> {
    return this.http.get<T>(this.resourcePath(), this.httpOptions)
      .pipe(retry(2), catchError(this.handleError));
  }

  private resourcePath(): string {
    return `${this.basePath}${this.resourceEndpoint}`;
  }
```
### Creación del Service Students

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `students`:

```bash
ng generate service learning/services/students --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `students.service.ts`, ubicado en la carpeta `/src/app/learning/services`:

```ts
import { BaseService } from "../../shared/services/base.service";
import { HttpClient } from "@angular/common/http";
import { Student } from "../model/student.entity";
```

**Aplicar** herencia de la clase `BaseService` a la clase `StudentsService`, agregando la siguiente instrucción a la clase `StudentsService`:

```ts
extends BaseService<Student>
```

**Reemplazar** el contructor de la clase `StudentsService` con el siguiente código, ubicado en el archivo `students.service.ts`:

```ts
constructor(http: HttpClient) {
  super(http);
  this.resourceEndpoint = '/students';
}
```

### Modificación del StudentManagementComponent

**Agregar** los siguientes import al archivo `student-management.component.ts`, ubicado en la carpeta `/src/app/learning/pages/student-management`:

```ts
import { AfterViewInit, OnInit, ViewChild} from '@angular/core';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { MatPaginator } from "@angular/material/paginator";
import { MatSort } from "@angular/material/sort";
import { MatIconModule } from "@angular/material/icon";
import { StudentsService } from "../../services/students.service";
import { Student } from "../../model/student.entity";
import { StudentCreateAndEditComponent } from "../../components/student-create-and-edit/student-create-and-edit.component";
import { NgClass } from "@angular/common";
import { TranslateModule } from "@ngx-translate/core";
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `StudentManagementComponent`, ubicado en el archivo `student-management.component.ts`:

```ts
MatPaginator, MatSort, MatIconModule, StudentCreateAndEditComponent, MatTableModule, NgClass, TranslateModule
```

**Aplicar** implementación de las interfaces `OnInit` y `AfterViewInit` a las clase `StudentManagementComponent`, agregando la siguiente instrucción a la clase `StudentManagementComponent`:

```ts
implements OnInit, AfterViewInit 
```

**Reemplazar** el contenido de la clase `StudentManagementComponent` con el siguiente código, ubicado en el archivo `student-management.component.ts`:

```ts
  // Attributes
  studentData: Student;
  dataSource!: MatTableDataSource<any>;
  displayedColumns: string[] = ['id', 'name', 'age', 'address', 'actions'];
  isEditMode: boolean;

  @ViewChild(MatPaginator, { static: false}) paginator!: MatPaginator;
  @ViewChild(MatSort, { static: false}) sort!: MatSort;

  // Constructor
  constructor(private studentService: StudentsService) {
    this.isEditMode = false;
    this.studentData = {} as Student;
    this.dataSource = new MatTableDataSource<any>();
  }

  // Private Methods
  private resetEditState(): void {
    this.isEditMode = false;
    this.studentData = {} as Student;
  }

  // CRUD Actions

  private getAllStudents(): void {
    this.studentService.getAll()
      .subscribe((response: any) => {
        this.dataSource.data = response;
      });
  };

  private createStudent(): void {
    this.studentService.create(this.studentData)
      .subscribe((response: any) => {
        this.dataSource.data.push({...response});
        // Actualiza el dataSource.data con los students actuales, para que Angular detecte el cambio y actualice la vista.
        this.dataSource.data = this.dataSource.data
          .map((student: Student) => {
            return student;
          });
      });
  };

  private updateStudent(): void {
    let studentToUpdate: Student = this.studentData;
    this.studentService.update(this.studentData.id, studentToUpdate)
      .subscribe((response: any) => {
        this.dataSource.data = this.dataSource.data
          .map((student: Student) => {
            if (student.id === response.id) {
              return response;
            }
            return student;
          });
      });
  };

  private deleteStudent(studentId: number): void {
    this.studentService.delete(studentId)
      .subscribe(() => {
        this.dataSource.data = this.dataSource.data
          .filter((student: Student) => {
            return student.id !== studentId ? student : false;
          });
      });
  };

  // UI Event Handlers

  onEditItem(element: Student) {
    this.isEditMode = true;
    this.studentData = element;
  }

  onDeleteItem(element: Student) {
    this.deleteStudent(element.id);
  }

  onCancelEdit() {
    this.resetEditState();
    this.getAllStudents();
  }

  onStudentAdded(element: Student) {
    this.studentData = element;
    this.createStudent();
    this.resetEditState();
  }

  onStudentUpdated(element: Student) {
    this.studentData = element;
    this.updateStudent();
    this.resetEditState();
  }

  // Lifecycle Hooks

  ngAfterViewInit(): void {
    this.dataSource.paginator = this.paginator;
    this.dataSource.sort = this.sort;
  }

  ngOnInit(): void {
    this.getAllStudents();
  }
```

**Reemplazar** el contenido del archivo `student-management.component.html` con el siguiente código, ubicado en la carpeta `/src/app/learning/pages/student-management`:

```html
<!-- Student catalogue -->
<div class="table-wrapper">
  <h3>{{ 'title.students' | translate }}</h3>

  <!--Add/Edit Form-->
  <app-student-create-and-edit (editCanceled)="onCancelEdit()"
                               (studentAdded)="onStudentAdded($event)"
                               (studentUpdated)="onStudentUpdated($event)"
                               [editMode] = "isEditMode"
                               [student]="studentData"/>
  <!-- Data Table -->
  <table mat-table [dataSource]="dataSource" class="mat-elevation-z8"
         matSort matSortActive="name" matSortDirection="asc">

    <ng-container matColumnDef="id">
      <th *matHeaderCellDef mat-header-cell mat-sort-header> {{ 'student.id' | translate }} </th>
      <td *matCellDef="let element" mat-cell>{{ element.id }}</td>
    </ng-container>

    <ng-container matColumnDef="name">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>{{ 'student.name' | translate }}</th>
      <td *matCellDef="let element" mat-cell>{{ element.name }}</td>
    </ng-container>

    <ng-container matColumnDef="age">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>{{ 'student.age' | translate }}</th>
      <td *matCellDef="let element" mat-cell>{{ element.age }}</td>
    </ng-container>

    <ng-container matColumnDef="address">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>{{ 'student.address' | translate }}</th>
      <td *matCellDef="let element" mat-cell>{{ element.address }}</td>
    </ng-container>

    <ng-container matColumnDef="actions">
      <th *matHeaderCellDef mat-header-cell> {{ 'student.actions' | translate }}</th>
      <td *matCellDef="let element" mat-cell>
        <a (click)="onEditItem(element)" href="javascript:void(0)"><mat-icon>edit</mat-icon></a>
        <a (click)="onDeleteItem(element)" href="javascript:void(0)"><mat-icon>delete</mat-icon></a>
      </td>
    </ng-container>

    <tr *matHeaderRowDef="displayedColumns" mat-header-row></tr>
    <tr *matRowDef="let row; columns:displayedColumns" [ngClass]="{'editable-row': studentData.id === row.id }" mat-row></tr>
  </table>
  <!-- Paginator child component-->
  <mat-paginator [pageSizeOptions]="[5, 10, 15]" [pageSize]="5" showFirstLastButtons></mat-paginator>
</div>
```

**Reemplazar** el contenido del archivo `student-management.component.css` con el siguiente código, ubicado en la carpeta `/src/app/learning/pages/student-management`:

```css
.table-wrapper {
  width: 1024px;
  margin: 50px auto;
}

table {
  width: 100%;
}

tr.editable-row {
  background-color: lightblue;
}
```


### Modificación del StudentCreateAndEdit

**Agregar** los siguientes `import` al archivo `student-create-and-edit.component.ts`, ubicado en la carpeta `/src/app/learning/components/student-create-and-edit`:

```ts
import { EventEmitter, Input, Output, ViewChild } from '@angular/core';
import { Student } from "../../model/student.entity";
import { FormsModule, NgForm } from "@angular/forms";
import { MatFormField } from "@angular/material/form-field";
import { MatInputModule } from "@angular/material/input";
import { MatButtonModule } from "@angular/material/button";
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `StudentCreateAndEditComponent`, ubicado en el archivo `student-create-and-edit.component.ts`:

```ts
MatFormField, MatInputModule, MatButtonModule, FormsModule
```

**Reemplazar** el contenido de la clase `StudentCreateAndEditComponent` con el siguiente código, ubicado en el archivo `student-create-and-edit.component.ts`:

```ts
  // Attributes
  @Input() student: Student;
  @Input() editMode: boolean = false;
  @Output() studentAdded: EventEmitter<Student> = new EventEmitter<Student>();
  @Output() studentUpdated: EventEmitter<Student> = new EventEmitter<Student>();
  @Output() editCanceled: EventEmitter<any> = new EventEmitter();
  @ViewChild('studentForm', {static: false}) studentForm!: NgForm;

  // Methods
  constructor() {
    this.student = {} as Student;
  }

  // Private methods
  private resetEditState(): void {
    this.student = {} as Student;
    this.editMode = false;
    this.studentForm.resetForm();
  }

  // Event Handlers

  onSubmit(): void {
    if (this.studentForm.form.valid) {
      let emitter: EventEmitter<Student> = this.editMode ? this.studentUpdated : this.studentAdded;
      emitter.emit(this.student);
      this.resetEditState();
    } else {
      console.error('Invalid data in form');
    }
  }

  onCancel(): void {
    this.editCanceled.emit();
    this.resetEditState();
  }
```

**Reemplazar** el contenido del archivo `student-create-and-edit.component.html` con el siguiente código, ubicado en la carpeta `/src/app/learning/components/student-create-and-edit`:

```html
<!-- Create and Edit Form -->
<h4>{{ editMode ? 'Edit Student' : 'New Student' }}</h4>
<div class="form-section">

  <form (submit)="onSubmit()" #studentForm='ngForm' class="form-row">
    <mat-form-field>
      <input matInput placeholder="Name" name="name" required [(ngModel)]="student.name">
    </mat-form-field>
    <mat-form-field id="age-form-field">
      <input matInput type="number" placeholder="Age" name="age" required [(ngModel)]="student.age">
    </mat-form-field>
    <mat-form-field>
      <input matInput placeholder="Address" name="address" required [(ngModel)]="student.address">
    </mat-form-field>
    <ng-container>
      @if (editMode === true) {
        <button mat-raised-button color="primary">Update</button>
        <button mat-raised-button color="warn" (click)="onCancel()">Cancel</button>
      }
      @else {
        <button mat-raised-button color="primary">Add</button>
      }
    </ng-container>
  </form>

</div>
```

**Reemplazar** el contenido del archivo `student-create-and-edit.component.css` con el siguiente código, ubicado en la carpeta `/src/app/learning/components/student-create-and-edit`:

```css
.form-section {
  display: table;
}

.form-row {
  display: table-cell;
  max-width: 2400px;
}

.form-row .mat-raised-button {
  margin-right: 8px;
}

.form-row .mat-form-field {
  margin-right: 8px;
}

.form-row #age-form-field {
  margin-right: 8px;
  width: 15%;
}
```





# Creación del Proyecto Learning Center
## Creación del proyecto y agregando Material Design (MacOS)

Cargar el `Terminal` de MacOS y ejecute las siguientes instrucciones:

```
cd /Users/alumnos/IdeaProjects
sudo ng new daos-ws51-learning-center
```

Which stylesheet format would you like to use? Seleccione la siguiente opción:

```
CSS  [ https://developer.mozilla.org/docs/Web/CSS
```

Do you want to enable Server-Side Rendering (SSR) and Static Site Generation 
(SSG/Prerendering)? Seleccione:

```
N
```

Luego ejecute las siguientes instrucciones:
```
cd daos-ws51-learning-center
sudo ng add @angular/material
```

Would you like to proceed?, digite:
```
Y
```

Choose a prebuilt theme name, or "custom" for a custom theme, seleccione:
```
Indigo/Pink  [ Preview: https://material.angular.io?theme=indigo-pink ]
```

Set up global Angular Material typography styles?, selecione:
```
Y
```

Include the Angular animations module?, selecione:
```
Include and enable animations
```

Luego ejecute las siguientes instrucciones:
```
cd ..
sudo chown -R alumnos ./daos-ws51-learning-center
```

## En el IDE IntelliJ IDEA Community

Cargar el IntelliJ IDEA y abra el proyecto ubicado en: `/Users/alumnos/IdeaProjects/daos-ws51-learning-center`  

### Configuración de JSON-SERVER

Cargar el `Terminal` del IDE y ejecutar la siguiente instrucción:
```
npm install json-server
```

En la carpeta principal crear la carpeta `server`

En la carpeta `server` crear el archivo `db.json` con el siguiente contenido:

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

En la carpeta `server` crear el archivo `routes.json` con el siguiente contenido:
```json
{
  "/api/v1/*": "/$1"
}

```

Cargar el `Terminal` del IDE y ejecutar la siguiente instrucción para ejecutar el `json-server`
```
npx json-server --watch server/db.json
```

### Configuración del environments

Cargar una nueva pestaña en el `Terminal` del IDE y ejecutar la siguiente instrucción
```
ng generate environments
```

Agregar los siguientes valores a la constante `environment` ubicado en el archivo `environment.development.ts` de la carpeta `environments`:
```typescript
  production: false,
  serverBasePath: 'http://localhost:3000'
```

Agregar los siguientes valores a la constante `environment` ubicado en el archivo `environment.ts` de la carpeta `environments`:
```typescript
  production: true,
  serverBasePath: undefined
```

### Creación de carpetas

En la carpeta `app` ubicado en la carpeta `src`, crear las carpetas: `learning`, `public` y `shared`.

En la carpeta `learning` crear las carpetas: `components`, `model`, `pages` y `services`-

En la carpeta `public` crear la carpeta: `pages`.

En la carpeta `shared` crear la carpeta: `services`.


### Creando componentes, model y service

En el Terminal del IDE crearemos los siguientes componentes:
```
ng generate component learning/components/student-create-and-edit --skip-tests=true
ng generate component learning/pages/student-management --skip-tests=true
ng generate component public/pages/about --skip-tests=true
ng generate component public/pages/home --skip-tests=true
ng generate component public/pages/page-not-found --skip-tests=true
```

En el Terminal del IDE crearemos el siguiente modelo:
```
ng generate class learning/model/student --type=entity
```

En el Terminal del IDE crearemos los siguientes services:
```
ng generate service learning/services/students
ng generate service shared/services/base
```

### Agregando el provide HttpClient al config

Agregar el siguiente import a la constante `appConfig` ubicado en el archivo `app.config.ts`
```typescript
import { provideHttpClient } from "@angular/common/http";
```

Agregar el método `provideHttpClient()` en la variable `providers` de la constante `appConfig` ubicado en el archivo `app.config.ts`

### Configuración del Routes

Agregar los siguientes imports a la constante `routes` ubicado en el archivo `app.routes.ts`:
```typescript
import { HomeComponent } from "./public/pages/home/home.component";
import { AboutComponent } from "./public/pages/about/about.component";
import { PageNotFoundComponent } from "./public/pages/page-not-found/page-not-found.component";
import { StudentManagementComponent } from "./learning/pages/student-management/student-management.component";
```

Agregar los siguientes valores a la constante `routes` ubicado en el archivo `app.routes.ts` de la carpeta `app`:
```typescript
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'learning/students', component: StudentManagementComponent },
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: '**', component: PageNotFoundComponent }
```

### Component AppComponent

Agregar los siguientes imports a la clase `AppComponent` ubicado en el archivo `app.component.ts`:
```typescript
import { RouterLink } from '@angular/router';
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';
import { MatToolbarModule } from '@angular/material/toolbar';
```

Agregar las clases `RouterLink, MatToolbarModule, MatButtonModule, MatIconModule` en la variable `imports` de `@Component` de la clase `AppComponent` ubicado en el archivo `app.component.ts`.

Agregar el siguiente atributo a la clase `AppComponent` ubicado en el archivo `app.component.ts`:
```typescript
  options = [
    { path: '/home', title: 'Home'},
    { path: '/learning/students', title: 'Students'},
    {path:'/about', title: 'About'}
  ]
```

Reemplazar el contenido del archivo `app.component.html` con:
```html
<mat-toolbar color="primary">
  <button mat-icon-button class="example-icon" aria-label="Example icon-button with menu icon">
    <mat-icon>menu</mat-icon>
  </button>
  <span>Learning Center</span>
  <span class="mat-spacer"></span>
  @for(option of options; track option.title) {
    <a mat-button [routerLink]="option.path">{{ option.title }}</a>
  }
</mat-toolbar>
<router-outlet/>
```

Reemplazar el contenido del archivo `app.component.css` con:
```css
.mat-spacer {
  flex: 1 1 auto;
}
```

Cargar el `Terminal` del IDE y ejecutar la siguiente instrucción:
```
ng serve
```

### Model Student

Agregar los siguentes atributos y constructor a la clase `Student` ubicado en el archivo `student.entity.ts` de la carpeta `learning/model`:
```typescript
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

### Service Base

Agregar los siguentes imports a la clase `BaseService` ubicado en el archivo `base.service.ts` de la carpeta `shared/services`:
```typescript
import { environment } from "../../../environments/environment";
import { HttpClient, HttpErrorResponse, HttpHeaders } from "@angular/common/http";
import { catchError, Observable, retry, throwError } from "rxjs";
```

Reemplazar el nombre de la clase `BaseService` por `BaseService<T>`

Reemplazar el contenido de la clase `BaseService` con los siguentes atributos, constructor y métodos a la clase `BaseService`:
```typescript
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

### Service Students

Agregar los siguentes imports a la clase `StudentsService` ubicado en el archivo `students.service.ts` de la carpeta `learning/services`:
```typescript
import { BaseService } from "../../shared/services/base.service";
import { HttpClient } from "@angular/common/http";
import { Student } from "../model/student.entity";
```

Aplicar herencia de la clase `BaseService`, agregando la siguiente instrucción a la clase `StudentsService`:
```typescript
extends BaseService<Student>
```

Reemplazar el contenido de la clase `StudentsService` con el constructor:
```typescript
  constructor(http: HttpClient) {
    super(http);
    this.resourceEndpoint = '/students';
  }
```


### Component StudentManagement

Agregar los siguientes imports a la clase `StudentManagementComponent` ubicado en el archivo `student-management.component.ts` de la carpeta `learning/pages/`:
```typescript
import { AfterViewInit, OnInit, ViewChild} from '@angular/core';
import { MatInputModule } from '@angular/material/input';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { MatPaginator } from "@angular/material/paginator";
import { MatSort } from "@angular/material/sort";
import { MatIconModule } from "@angular/material/icon";
import { StudentsService } from "../../services/students.service";
import { Student } from "../../model/student.entity";
import { StudentCreateAndEditComponent } from "../../components/student-create-and-edit/student-create-and-edit.component";
import { NgClass } from "@angular/common";
```

Agregar las clases `MatPaginator, MatSort, MatIconModule, StudentCreateAndEditComponent, MatTableModule, NgClass` en la variable `imports` de `@Component` de la clase `StudentManagementComponent`.

Aplicar implementación de interfaces, agregando la siguiente instrucción a la clase `StudentManagementComponent`:
```typescript
implements OnInit, AfterViewInit 
```

Reemplazar el contenido de la clase `StudentManagementComponent` con los siguentes atributos, constructor y métodos:
```typescript
  // Attributes
  studentData: Student;
  dataSource!: MatTableDataSource<any>;
  displayedColumns: string[] = ['id', 'name', 'age', 'address', 'actions'];
  @ViewChild(MatPaginator, { static: false}) paginator!: MatPaginator;
  @ViewChild(MatSort, { static: false}) sort!: MatSort;
  isEditMode: boolean;

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

  private getAllStudents() {
    this.studentService.getAll().subscribe((response: any) => {
      this.dataSource.data = response;
    });
  };

  private createStudent() {
    this.studentService.create(this.studentData).subscribe((response: any) => {
      this.dataSource.data.push({...response});
      this.dataSource.data = this.dataSource.data.map((student: Student) => { return student; });
    });
  };

  private updateStudent() {
    let studentToUpdate = this.studentData;
    this.studentService.update(this.studentData.id, studentToUpdate).subscribe((response: any) => {
      this.dataSource.data = this.dataSource.data.map((student: Student) => {
        if (student.id === response.id) {
          return response;
        }
        return student;
      });
    });
  };

  private deleteStudent(studentId: number) {
    this.studentService.delete(studentId).subscribe(() => {
      this.dataSource.data = this.dataSource.data.filter((student: Student) => {
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

Reemplazar el contenido del archivo `student-management.component.html` de la carpeta `learning/pages/`:
```html
<!-- Student catalogue -->
<div class="table-wrapper">
  <h1>Students</h1>

  <!--Add/Edit Form-->
  <app-student-create-and-edit (editCanceled)="onCancelEdit()"
                    (studentAdded)="onStudentAdded($event)"
                    (studentUpdated)="onStudentUpdated($event)"
                    [editMode] = "isEditMode"
                    [student]="studentData"/>
  <!-- Data Table -->
  <table mat-table [dataSource]="dataSource" class="mat-elevation-z8" matSort>
    <ng-container matColumnDef="id">
      <th *matHeaderCellDef mat-header-cell mat-sort-header> #Id</th>
      <td *matCellDef="let element" mat-cell>{{ element.id }}</td>
    </ng-container>
    <ng-container matColumnDef="name">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>Name</th>
      <td *matCellDef="let element" mat-cell>{{ element.name }}</td>
    </ng-container>
    <ng-container matColumnDef="age">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>Age</th>
      <td *matCellDef="let element" mat-cell>{{ element.age }}</td>
    </ng-container>
    <ng-container matColumnDef="address">
      <th *matHeaderCellDef mat-header-cell mat-sort-header>Address</th>
      <td *matCellDef="let element" mat-cell>{{ element.address }}</td>
    </ng-container>
    <ng-container matColumnDef="actions">
      <th *matHeaderCellDef mat-header-cell> Actions</th>
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

Reemplazar el contenido del archivo `student-management.component.css` de la carpeta `learning/pages/`:
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


### Component StudentCreateAndEdit

Agregar los siguientes imports a la clase `StudentCreateAndEditComponent` ubicado en el archivo `student-create-and-edit.component.ts` de la carpeta `learning/components`:
```typescript
import {EventEmitter, Input, Output, ViewChild} from '@angular/core';
import {Student} from "../../model/student.entity";
import {FormsModule, NgForm} from "@angular/forms";
import {MatFormField} from "@angular/material/form-field";
import {MatFormFieldControl} from "@angular/material/form-field";
import {MatInputModule} from "@angular/material/input";
import {MatButtonModule} from "@angular/material/button";
import {NgIf} from "@angular/common";
```

Agregar las clases `MatFormField, MatInputModule, MatButtonModule, FormsModule, NgIf` en la variable `imports` de `@Component` de la clase `StudentCreateAndEditComponent`.


Reemplazar el contenido de la clase `StudentCreateAndEditComponent` con los siguentes atributos, constructor y métodos:
```typescript
  // Attributes
  @Input() student: Student;
  @Input() editMode = false;
  @Output() studentAdded = new EventEmitter<Student>();
  @Output() studentUpdated = new EventEmitter<Student>();
  @Output() editCanceled = new EventEmitter();
  @ViewChild('studentForm', {static: false}) studentForm!: NgForm;

  // Methods
  constructor() {
    this.student = {} as Student;
  }

  // Private methods
  private resetEditState() {
    this.student = {} as Student;
    this.editMode = false;
    this.studentForm.resetForm();
  }

  // Event Handlers

  onSubmit() {
    if (this.studentForm.form.valid) {
      let emitter = this.editMode ? this.studentUpdated : this.studentAdded;
      emitter.emit(this.student);
      this.resetEditState();
    } else {
      console.error('Invalid data in form');
    }
  }

  onCancel() {
    this.editCanceled.emit();
    this.resetEditState();
  }
```

Reemplazar el contenido del archivo `student-create-and-edit.component.html` de la carpeta `learning/components`:
```html
<!-- Create and Edit Form -->
<h2>{{ editMode ? 'Edit Student' : 'New Student' }}</h2>
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
    <ng-container *ngIf="editMode; else elseTemplate">
      <button mat-raised-button color="primary">Update</button>
      <button mat-raised-button color="warn" (click)="onCancel()">Cancel</button>
    </ng-container>
    <ng-template #elseTemplate>
      <button mat-raised-button color="primary">Add</button>
    </ng-template>
  </form>
</div>

```

Reemplazar el contenido del archivo `student-create-and-edit.component.css` de la carpeta `learning/components`:
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





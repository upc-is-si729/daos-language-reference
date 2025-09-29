# Proyecto Learning center v2520
 
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
ng new daos-learning-center-v2520
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
cd daos-learning-center-v2520
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
sudo chown -R alumnos ./daos-learning-center-v2520
```

```
ls -l
```



## Desarrollo del proyecto

**Cargar** el IntelliJ IDEA y **abrir** el proyecto ubicado en la carpeta de su preferencia.

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command:
```
ng serve --port 4200
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
  "option": {
    "home": "Home",
    "about": "About Us",
    "categories": "Categories",
    "courses": "Courses"
  },
  "about": {
    "title": "About us",
    "content": "ACME Learning Center is an Education Platform, part of ACME Corporation."
  },
  "home": {
    "title": "Welcome",
    "content": "Welcome to ACME Learning Center."
  },
  "page-not-found": {
    "title": "Page not found",
    "content": "The path <strong>{{ invalid_path }}</strong> is not valid.",
    "go-home": "Go Home"
  },
  "footer": {
    "rights": "All rights reserved.",
    "powered-by": "Powered by",
    "and": "and"
  }
}

```

#### es.json
```json
{
  "option" : {
    "home": "Inicio",
    "about": "Acerca de",
    "categories": "Categorías",
    "courses": "Cursos"
  },
  "about": {
    "title": "Acerca de nosotros",
    "content": "ACME Learning Center es una plataforma educativa, parte de ACME Corporation."
  },
  "home": {
    "title": "Bienvenido",
    "content": "Bienvenido a ACME Learning Center."
  },
  "page-not-found": {
    "title": "Página no encontrada",
    "content": "La ruta <strong>{{ invalidPath }}</strong> es inválida.",
    "go-home": "Ir a Inicio"
  },
  "footer": {
    "rights": "Todos los derechos reservados.",
    "powered-by": "Desarrollado con",
    "and": "y"
  }
}

```

### Configuración de JSON-Server

**Crear** la carpeta `server` en la carpeta raiz del proyecto:

```markdown
- 📂 daos-learning-center-v2520
  - 📁 server
```

**Crear** los archivo `db.json` y `routes.json` en la carpeta `server` con el siguiente contenido:

#### db.json
```json
{
  "categories": [
    {
      "id": 1,
      "name": "Java"
    },
    {
      "id": 2,
      "name": "Spring Boot"
    },
    {
      "id": 3,
      "name": "TypeScript"
    },
    {
      "id": 4,
      "name": "Angular"
    },
    {
      "id": 5,
      "name": "RESTful"
    }
  ],
  "courses": [
    {
      "id": 1,
      "title": "Java for Beginners",
      "description": "An introductory course to Java programming language.",
      "categoryId": 1
    },
    {
      "id": 2,
      "title": "Advanced Java Concepts",
      "description": "A deep dive into advanced Java programming topics.",
      "categoryId": 1
    },
    {
      "id": 3,
      "title": "Spring Boot Fundamentals",
      "description": "Learn the basics of Spring Boot framework.",
      "categoryId": 2
    },
    {
      "id": 4,
      "title": "Building RESTful APIs with Spring Boot",
      "description": "Create robust RESTful APIs using Spring Boot.",
      "categoryId": 2
    },
    {
      "id": 5,
      "title": "TypeScript Essentials",
      "description": "Get started with TypeScript and its features.",
      "categoryId": 3
    },
    {
      "id": 6,
      "title": "Advanced TypeScript",
      "description": "Explore advanced concepts in TypeScript programming.",
      "categoryId": 3
    },
    {
      "id": 7,
      "title": "Angular for Beginners",
      "description": "An introductory course to Angular framework.",
      "categoryId": 4
    },
    {
      "id": 8,
      "title": "Building SPAs with Angular",
      "description": "Learn how to build Single Page Applications using Angular.",
      "categoryId": 4
    },
    {
      "id": 9,
      "title": "RESTful Web Services",
      "description": "Understand the principles of RESTful web services.",
      "categoryId": 5
    },
    {
      "id": 10,
      "title": "Consuming RESTful APIs",
      "description": "Learn how to consume RESTful APIs in your applications.",
      "categoryId": 5
    },
    {
      "id": 11,
      "title": "Full-Stack Development with Java and Angular",
      "description": "Combine Java backend with Angular frontend for full-stack development.",
      "categoryId": 1
    },
    {
      "id": 12,
      "title": "Microservices with Spring Boot",
      "description": "Build and deploy microservices using Spring Boot.",
      "categoryId": 2
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

**Cargar** el navegador e **ingrese** la siguiente Url: 
- http://localhost:3000/api/v1/categories
- http://localhost:3000/api/v1/courses


### Configuración de environments

A continuación se detalla las intrucciones para definir diferentes `named build configurations`  para el proyecto, como `development` y `staging` con diferentes valores predeterminados. Más información en: https://angular.dev/tools/cli/environments#configure-environment-specific-defaults

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** el siguiente CLI command para crear el directorio `environments` y los archivos de configuración, ubicado en la carpeta `src`:

```bash
ng generate environments
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.development.ts` ubicado en la carpeta `/src/environments`:

```ts
production: false,
platformProviderApiBaseUrl: 'http://localhost:3000/api/v1',
platformProviderCategoriesEndpointPath: '/categories',
platformProviderCoursesEndpointPath: '/courses',
logoProviderApiBaseUrl: 'https://logo.clearbit.com/'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
platformProviderApiBaseUrl: 'http://localhost:3000/api/v1',
platformProviderCategoriesEndpointPath: '/categories',
platformProviderCoursesEndpointPath: '/courses',
logoProviderApiBaseUrl: 'https://logo.clearbit.com/'
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
import { provideAppInitializer, inject } from '@angular/core';
import { provideHttpClient } from '@angular/common/http';
import { provideTranslateService, TranslateService } from '@ngx-translate/core';
import { provideTranslateHttpLoader } from '@ngx-translate/http-loader';
```

**Agregar** los siguientes métodos al array `providers` de la constante `appConfig` del archivo `app.config.ts` :
```ts
provideHttpClient(),
provideTranslateService({
  loader: provideTranslateHttpLoader({prefix: './assets/i18n/', suffix: '.json'}),
  lang: 'en',
  fallbackLang: 'en'
}),
provideAppInitializer(() => {
  const translate = inject(TranslateService);
  translate.use(translate.getBrowserLang() || "en");
})
```

### Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 learning
      - 📁 application
      - 📁 domain
        - 📁 model
      - 📁 infrastructure
      - 📁 presentation
        - 📁 views
    - 📂 shared
      - 📁 infrastructure
      - 📁 presentation
        - 📁 components
        - 📁 views
```

## Creación de los interface y class Base

## Creación de los interface Base

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Base` tipo `entity`:

```bash
ng generate interface shared/infrastructure/base-entity
```

**Reemplazar** el contenido de la interface `BaseEntity` del archivo `base-entity.ts` con el siguiente código, ubicado en la carpeta `/src/app/shared/infrastructure`:

```ts
export interface BaseEntity {
  /**
   * The unique identifier for the entity.
   */
  id: number;
}
```

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Base` tipo `response`:

```bash
ng generate interface shared/infrastructure/base-response
```

**Reemplazar** el contenido de la interface `BaseResponse` del archivo `base-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/shared/infrastructure`:

```ts
export interface BaseResponse {}

/**
 * Defines a standard structure for API resources/DTOs with a unique identifier.
 */
export interface BaseResource {
  /**
   * The unique identifier for the resource.
   */
  id: number;
}
```

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Base` tipo `assembler`:

```bash
ng generate interface shared/infrastructure/base-assembler
```

**Reemplazar** el contenido de la interface `BaseAssembler` del archivo `base-assembler.ts` con el siguiente código, ubicado en la carpeta `/src/app/shared/infrastructure`:

```ts
import {BaseResource, BaseResponse} from './base-response';
import {BaseEntity} from './base-entity';

/**
 * Defines a contract for assembler classes that convert between entities, resources, and API responses.
 *
 * @template TEntity - The entity type (e.g., Course), must
 * @template TResource - The resource type, must extend BaseResource.
 * @template TResponse - The response type, must extend BaseResponse.
 */
export interface BaseAssembler<TEntity extends BaseEntity, TResource extends BaseResource, TResponse extends BaseResponse> {
  /**
   * Converts a resource to an entity.
   * @param resource - The resource to convert.
   * @returns The converted entity.
   */
  toEntityFromResource(resource: TResource): TEntity;

  /**
   * Converts an entity to a resource.
   * @param entity - The entity to convert.
   * @returns The converted resource.
   */
  toResourceFromEntity(entity: TEntity): TResource;

  /**
   * Converts an API response to an array of entities.
   * @param response - The API response containing entities.
   * @returns An array of entities.
   */
  toEntitiesFromResponse(response: TResponse): TEntity[];
}
```

### Creación de los class Base

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el abstract class `Base` tipo `Api`:

```bash
ng generate class shared/infrastructure/base-api --skip-tests=true
```

**Reemplazar** el contenido del abstract class `BaseApi` del archivo `base-api.ts` con el siguiente código, ubicado en la carpeta `/src/app/shared/infrastructure`:

```ts
/**
 * Abstract base class for API services managing multiple endpoints within a bounded context.
 */
export abstract class BaseApi {
  // No methods defined; child classes will compose endpoint instances
}
```

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el abstract class `Base` tipo `ApiEndpoint`:

```bash
ng generate class shared/infrastructure/base-api-endpoint --skip-tests=true
```

**Reemplazar** el contenido del abstract class `BaseApiEndpoint` del archivo `base-api-endpoint.ts` con el siguiente código, ubicado en la carpeta `/src/app/shared/infrastructure`:

```ts
import {HttpClient, HttpErrorResponse} from '@angular/common/http';
import {Observable, throwError} from 'rxjs';
import {catchError, map} from 'rxjs/operators';
import {BaseEntity} from './base-entity';
import {BaseResource, BaseResponse} from './base-response';
import {BaseAssembler} from './base-assembler';

/**
 * Base class for API endpoint operations with generic CRUD functionality.
 *
 * @template TEntity - The entity type, which must extend BaseEntity.
 * @template TResource - The resource type, must extend BaseResource.
 * @template TResponse - The response type, must extend BaseResponse.
 * @template TAssembler - The assembler type implementing BaseAssembler with matching generics.
 */
export abstract class BaseApiEndpoint<
  TEntity extends BaseEntity,
  TResource extends BaseResource,
  TResponse extends BaseResponse,
  TAssembler extends BaseAssembler<TEntity, TResource, TResponse>
> {
  constructor(
    protected http: HttpClient,
    protected endpointUrl: string,
    protected assembler: TAssembler
  ) {}

  /**
   * Retrieves all entities from the API, handling both response objects and arrays.
   * @returns An Observable for an array of entities.
   */
  getAll(): Observable<TEntity[]> {
    return this.http.get<TResponse | TResource[]>(this.endpointUrl).pipe(
      map(response => {
        console.log(response);
        if (Array.isArray(response)) {
          return response.map(resource => this.assembler.toEntityFromResource(resource));
        }
        return this.assembler.toEntitiesFromResponse(response as TResponse);
      }),
      catchError(this.handleError('Failed to fetch entities'))
    );
  }

  /**
   * Retrieves a single entity by ID.
   * @param id - The ID of the entity.
   * @returns An Observable of the entity.
   */
  getById(id: number): Observable<TEntity> {
    return this.http.get<TResource>(`${this.endpointUrl}/${id}`).pipe(
      map(resource => this.assembler.toEntityFromResource(resource)),
      catchError(this.handleError('Failed to fetch entity'))
    );
  }

  /**
   * Creates a new entity.
   * @param entity - The entity to create.
   * @returns An Observable of the created entity.
   */
  create(entity: TEntity): Observable<TEntity> {
    const resource = this.assembler.toResourceFromEntity(entity);
    return this.http.post<TResource>(this.endpointUrl, resource).pipe(
      map(created => this.assembler.toEntityFromResource(created)),
      catchError(this.handleError('Failed to create entity'))
    );
  }

  /**
   * Updates an existing entity.
   * @param entity - The entity to update.
   * @param id - The ID of the entity.
   * @returns An Observable of the updated entity.
   */
  update(entity: TEntity, id: number): Observable<TEntity> {
    const resource = this.assembler.toResourceFromEntity(entity);
    return this.http.put<TResource>(`${this.endpointUrl}/${id}`, resource).pipe(
      map(updated => this.assembler.toEntityFromResource(updated)),
      catchError(this.handleError('Failed to update entity'))
    );
  }

  /**
   * Deletes an entity by ID.
   * @param id - The ID of the entity to delete.
   * @returns An Observable of void.
   */
  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.endpointUrl}/${id}`).pipe(
      catchError(this.handleError('Failed to delete entity'))
    );
  }

  /**
   * Handles HTTP errors and returns a user-friendly error message.
   * @param operation - The operation that failed.
   * @returns A function that transforms an error into an Observable.
   */
  protected handleError(operation: string) {
    return (error: HttpErrorResponse): Observable<never> => {
      let errorMessage = operation;
      if (error.status === 404) {
        errorMessage = `${operation}: Resource not found`;
      } else if (error.error instanceof ErrorEvent) {
        errorMessage = `${operation}: ${error.error.message}`;
      } else {
        errorMessage = `${operation}: ${error.statusText || 'Unexpected error'}`;
      }
      return throwError(() => new Error(errorMessage));
    };
  }
}

```


## Analizando Fake learning API

### Analizando el endpoint categories

Dirijase al endpoint local http://localhost:3000/api/v1/categories y **evalue** el json de respuesta. 

```json
[
  {
    "id": 1,
    "name": "Java"
  },
  {
    "id": 2,
    "name": "Spring Boot"
  }
]
```

### Creación de la interface Categories tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Product` tipo `response`:

```bash
ng generate interface learning/infrastructure/categories-response
```

**Reemplazar** el contenido de la interface `CategoriesResponse` del archivo `categories-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/learning/infrastructure`:

```ts
import {BaseResource, BaseResponse} from '../../shared/infrastructure/base-response';

/**
 * Represents the API response structure for a list of categories.
 */
export interface CategoriesResponse extends BaseResponse {
  /**
   * The list of categories returned by the API.
   */
  categories: CategoryResource[];
}

/**
 * Represents the API resource/DTO for a category.
 */
export interface CategoryResource extends BaseResource {
  /**
   * The unique identifier for the category.
   */
  id: number;

  /**
   * The name of the category.
   */
  name: string;
}
```

### Creación del class Category tipo entity (model)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `category`:

```bash
ng generate class learning/domain/model/category --type=entity --skip-tests=true
```

**Agregar** el siguiente `import` a la class `Category` del archivo `category.entity.ts` ubicado en la carpeta `/src/app/learning/domain/model`:

```typescript
import {BaseEntity} from '../../../shared/infrastructure/base-entity';
```

**Agregar** la interface `BaseEntity` a la clase `Category`, quedando de la siguiente manera:

```typescript
export class Category implements BaseEntity
```

**Agregar** los siguentes atributos y constructor a la clase `Category` del archivo `category.entity.ts` con el siguiente código:

```typescript
constructor(category: { id: number; name: string }) {
  this._id = category.id;
  this._name = category.name;
}

/**
 * The unique identifier for the category.
 */
private _id: number;

get id(): number {
  return this._id;
}

set id(value: number) {
  this._id = value;
}

/**
 * The name of the category.
 */
private _name: string;

get name(): string {
  return this._name;
}

set name(value: string) {
  this._name = value;
}
```

### Creación del class Category tipo Assembler (DTO Assembler)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Category` tipo `Assembler`:

```bash
ng generate class learning/infrastructure/category-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `category-assembler.ts`, ubicado en la carpeta `/src/app/learning/infrastructure`:
```typescript
import {BaseAssembler} from '../../shared/infrastructure/base-assembler';
import {Category} from '../domain/model/category.entity';
import {CategoriesResponse, CategoryResource} from './categories-response';
```

**Agregar** la interface `BaseAssembler` a la clase `CategoryAssembler`, quedando de la siguiente manera:

```typescript
export class CategoryAssembler implements BaseAssembler<Category, CategoryResource, CategoriesResponse>
```

**Reemplazar** el contenido de la clase `CategoryAssembler` con el siguiente código, ubicado en el archivo `category-assembler.ts`:

```ts
/**
 * Converts a CategoriesResponse to an array of Category entities.
 * @param response - The API response containing categories.
 * @returns An array of Category entities.
 */
toEntitiesFromResponse(response: CategoriesResponse): Category[] {
  return response.categories.map(resource => this.toEntityFromResource(resource as CategoryResource));
}

/**
 * Converts a CategoryResource to a Category entity.
 * @param resource - The resource to convert.
 * @returns The converted Category entity.
 */
toEntityFromResource(resource: CategoryResource): Category {
  return new Category({
    id: resource.id,
    name: resource.name
  });
}

/**
 * Converts a Category entity to a CategoryResource.
 * @param entity - The entity to convert.
 * @returns The converted CategoryResource.
 */
toResourceFromEntity(entity: Category): CategoryResource {
  return {
    id: entity.id,
    name: entity.name
  } as CategoryResource;
}
```


### Analizando el endpoint courses

Dirijase al endpoint local http://localhost:3000/api/v1/courses y **evalue** el json de respuesta. 

```json
[
  {
    "id": 1,
    "title": "Java for Beginners",
    "description": "An introductory course to Java programming language.",
    "categoryId": 1    
  },
  {
    "id": 2,
    "title": "Advanced Java Concepts",
    "description": "A deep dive into advanced Java programming topics.",
    "categoryId": 1
  }
]
```

### Creación de la interface Course tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Courses` tipo `response`:

```bash
ng generate interface learning/infrastructure/courses-response
```

**Reemplazar** el contenido de la interface `CoursesResponse` del archivo `courses-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/learning/infrastructure`:

```ts
import {BaseResource, BaseResponse} from '../../shared/infrastructure/base-response';

export interface CoursesResponse extends BaseResponse {
  /**
   * Array of course resources included in the response.
   */
  courses: CourseResource[];
}

/**
 * Represents a single course resource returned from the API.
 *
 * @remarks
 * This interface extends {@link BaseResource} and includes the core properties of a course.
 *
 * @property id - Unique identifier for the course.
 * @property title - Title of the course.
 * @property description - Description of the course.
 */
export interface CourseResource extends BaseResource {
  /**
   * Unique identifier for the course.
   */
  id: number;
  /**
   * Title of the course.
   */
  title: string;
  /**
   * Description of the course.
   */
  description: string;

  /**
   * Identifier for the category this course belongs to.
   */
  categoryId: number;
}
```

### Creación del class Course tipo entity (model)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `course`:

```bash
ng generate class learning/domain/model/course --type=entity --skip-tests=true
```

**Agregar** el siguiente `import` a la class `Course` del archivo `course.entity.ts` ubicado en la carpeta `/src/app/learning/domain/model`:

```typescript
import {Category} from './category.entity';
```

**Agregar** los siguentes atributos y constructor a la clase `Course` del archivo `course.entity.ts` con el siguiente código:

```typescript
private _id: number;
private _title: string;
private _description: string;
private _categoryId: number;
private _category: Category | null;

  /**
   * Creates a new instance of the Course class.
   *
   * @param course - An object containing optional properties to initialize the course.
   * @param course.id - The unique identifier for the course.
   * @param course.title - The title of the course.
   * @param course.description - The description of the course.
   * @param course.categoryId - The identifier of the category associated with the course.
   * @param course.category - (Optional) The category associated with the course.
   */
constructor(course: { id: number; title: string; description: string; categoryId: number; category?: Category | null }) {
    this._id = course.id;
    this._title = course.title;
    this._description = course.description;
    this._categoryId = course.categoryId;
    this._category = course.category ?? null;
 }

get id(): number {
    return this._id;
}
set id(value: number) {
    this._id = value;
}
get title(): string {
    return this._title;
}
set title(value: string) {
    this._title = value;
}
get description(): string {
    return this._description;
}
set description(value: string) {
    this._description = value;
}
get categoryId(): number {
    return this._categoryId;
}
set categoryId(value: number) {
    this._categoryId = value;
}
/**
 * The category associated with the course.
 * @remarks
 * This is an object reference to the {@link Category} entity. It may be null if not set.
 */
get category(): Category | null {
    return this._category;
}

/**
 * Sets the category associated with the course.
 *
 * @param value - The {@link Category} to associate with the course.
 */
set category(value: Category | null) {
    this._category = value;
}
```


### Creación del class Course tipo Assembler (DTO Assembler)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Course` tipo `Assembler`:

```bash
ng generate class learning/infrastructure/course-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `course-assembler.ts`, ubicado en la carpeta `/src/app/learning/infrastructure`:
```typescript
import {CourseResource, CoursesResponse} from './courses-response';
import {Course} from '../domain/model/course.entity';
import {BaseAssembler} from '../../shared/infrastructure/base-assembler';
```

**Agregar** la interface `BaseAssembler` a la clase `CourseAssembler`, quedando de la siguiente manera:

```typescript
export class CourseAssembler implements BaseAssembler<Course, CourseResource, CoursesResponse>
```

**Reemplazar** el contenido de la clase `CategoryAssembler` con el siguiente código, ubicado en el archivo `category-assembler.ts`:

```ts
/**
 * Converts a CoursesResponse to an array of Course entities.
 * @param response - The API response containing courses.
 * @returns An array of Course entities.
 */
toEntitiesFromResponse(response: CoursesResponse): Course[] {
  console.log(response);
  return response.courses.map(resource => this.toEntityFromResource(resource as CourseResource));
}

/**
 * Converts a CourseResource to a Course entity.
 * @param resource - The resource to convert.
 * @returns The converted Course entity.
 */
toEntityFromResource(resource: CourseResource): Course {
  return new Course({
    id: resource.id,
    title: resource.title,
    description: resource.description,
    categoryId: resource.categoryId
  });
}

/**
 * Converts a Course entity to a CourseResource.
 * @param entity - The entity to convert.
 * @returns The converted CourseResource.
 */
toResourceFromEntity(entity: Course): CourseResource {
  return {
    id: entity.id,
    title: entity.title,
    description: entity.description
  } as CourseResource;
}
```

## Creación del LearningApi Service

### Creación del class Categories tipo ApiEndpoint

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Categories` tipo `ApiEndpoint`:

```bash
ng generate class learning/infrastructure/categories-api-endpoint --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `categories-api-endpoint.ts`, ubicado en la carpeta `/src/app/learning/infrastructure`:
```typescript
import {BaseApiEndpoint} from '../../shared/infrastructure/base-api-endpoint';
import {Category} from '../domain/model/category.entity';
import {CategoriesResponse, CategoryResource} from './categories-response';
import {CategoryAssembler} from './category-assembler';
import {HttpClient} from '@angular/common/http';
import {environment} from '../../../environments/environment';
```

**Extends** la class `BaseApiEndpoint` a la clase `CategoriesApiEndpoint`, quedando de la siguiente manera:

```typescript
export class CategoriesApiEndpoint extends BaseApiEndpoint<Category, CategoryResource, CategoriesResponse, CategoryAssembler>
```

**Reemplazar** el contenido de la clase `CategoriesApiEndpoint` con el siguiente código, ubicado en el archivo `categories-api-endpoint.ts`:

```ts
/**
 * Creates an instance of CategoriesApiEndpoint.
 * @param http - The HttpClient to be used for making API requests.
 */
constructor(http: HttpClient) {
  super(http, `${environment.platformProviderApiBaseUrl}${environment.platformProviderCategoriesEndpointPath}`,
    new CategoryAssembler());
}
```

### Creación del class Courses tipo ApiEndpoint

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Courses` tipo `ApiEndpoint`:

```bash
ng generate class learning/infrastructure/courses-api-endpoint --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `courses-api-endpoint.ts`, ubicado en la carpeta `/src/app/learning/infrastructure`:
```typescript
import {BaseApiEndpoint} from '../../shared/infrastructure/base-api-endpoint';
import {Course} from '../domain/model/course.entity';
import {CourseResource, CoursesResponse} from './courses-response';
import {CourseAssembler} from './course-assembler';
import {HttpClient} from '@angular/common/http';
import {environment} from '../../../environments/environment';
```

**Extends** la class `BaseApiEndpoint` a la clase `CoursesApiEndpoint`, quedando de la siguiente manera:

```typescript
export class CoursesApiEndpoint extends BaseApiEndpoint<Course, CourseResource, CoursesResponse, CourseAssembler>
```

**Reemplazar** el contenido de la clase `CoursesApiEndpoint` con el siguiente código, ubicado en el archivo `courses-api-endpoint.ts`:

```ts
/**
 * Creates an instance of CoursesApiEndpoint.
 * @param http - The HttpClient to be used for making API requests.
 */
constructor(http: HttpClient) {
  super(http, `${environment.platformProviderApiBaseUrl}${environment.platformProviderCoursesEndpointPath}`,
    new CourseAssembler());
}
```

### Creación del class Learning tipo Api

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `learning-api`:

```bash
ng generate service learning/infrastructure/learning-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `learning-api.ts`, ubicado en la carpeta `/src/app/learning/infrastructure`:
```typescript
import {BaseApi} from '../../shared/infrastructure/base-api';
import {Course} from '../domain/model/course.entity';
import {Category} from '../domain/model/category.entity';
import {HttpClient} from '@angular/common/http';
import {CoursesApiEndpoint} from './courses-api-endpoint';
import {CategoriesApiEndpoint} from './categories-api-endpoint';
import {Observable} from 'rxjs';
```

**Extends** la class `BaseApi` a la clase `LearningApi`, quedando de la siguiente manera:

```typescript
export class LearningApi extends BaseApi
```

**Reemplazar** el contenido de la clase `LearningApi` con el siguiente código, ubicado en el archivo `learning-api.ts`:

```ts
private readonly coursesEndpoint:     CoursesApiEndpoint;
private readonly categoriesEndpoint:  CategoriesApiEndpoint;

constructor(http: HttpClient) {
  super();
  this.coursesEndpoint =    new CoursesApiEndpoint(http);
  this.categoriesEndpoint = new CategoriesApiEndpoint(http);
}

/**
 * Retrieves all courses from the API.
 * @returns An Observable for an array of Course objects.
 */
getCourses(): Observable<Course[]> {
  return this.coursesEndpoint.getAll();
}

/**
 * Retrieves a single course by ID.
 * @param id - The ID of the course.
 * @returns An Observable of the Course object.
 */
getCourse(id: number): Observable<Course> {
  return this.coursesEndpoint.getById(id);
}

/**
 * Creates a new course.
 * @param course - The course to create.
 * @returns An Observable of the created Course object.
 */
createCourse(course: Course): Observable<Course> {
  return this.coursesEndpoint.create(course);
}

/**
 * Updates an existing course.
 * @param course - The course to update.
 * @returns An Observable of the updated Course object.
 */
updateCourse(course: Course): Observable<Course> {
  return this.coursesEndpoint.update(course, course.id);
}

/**
 * Deletes a course by ID.
 * @param id - The ID of the course to delete.
 * @returns An Observable of void.
 */
deleteCourse(id: number): Observable<void> {
  return this.coursesEndpoint.delete(id);
}

/**
 * Retrieves all categories from the API.
 * @returns An Observable for an array of Category objects.
 */
getCategories(): Observable<Category[]> {
  return this.categoriesEndpoint.getAll();
}

/**
 * Retrieves a single category by ID.
 * @param id - The ID of the category.
 * @returns An Observable of the Category object.
 */
getCategory(id: number): Observable<Category> {
  return this.categoriesEndpoint.getById(id);
}

/**
 * Creates a new category.
 * @param category - The category to create.
 * @returns An Observable of the created Category object.
 */
createCategory(category: Category): Observable<Category> {
  return this.categoriesEndpoint.create(category);
}

/**
 * Updates an existing category.
 * @param category - The category to update.
 * @returns An Observable of the updated Category object.
 */
updateCategory(category: Category): Observable<Category> {
  return this.categoriesEndpoint.update(category, category.id);
}

/**
 * Deletes a category by ID.
 * @param id - The ID of the category to delete.
 * @returns An Observable of void.
 */
deleteCategory(id: number): Observable<void> {
  return this.categoriesEndpoint.delete(id);
}
```

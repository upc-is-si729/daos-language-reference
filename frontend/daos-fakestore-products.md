# Proyecto Fake-store

## Creación del proyecto y agregando Material Design

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y `chown`, y luego ingresar la contraseña del Administrador.
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/si729/2024-02` o en otra de su preferencia.

> [!CAUTION]
> **En el caso de estar en un equipo Windows:**
> - Debe ubicarse en la carpeta `IdeaProjects/` o en otra de su preferencia.

Cargar el `Terminal`, ubicarse en la carpeta de su preferencia de acuerdo al Sistema Operativo y ejecute el siguiente comando:

```
ng new daos-fakestore-products
```

A continuación, le mostrará diferentes opciones y debe escoger las siguientes:

_Which stylesheet format would you like to use?_ Seleccione la siguiente opción:

```
CSS  [ https://developer.mozilla.org/docs/Web/CSS
```

_Do you want to enable Server-Side Rendering (SSR) and Static Site Generation 
(SSG/Prerendering)?_, Seleccione:

```
N
```

### Agregando Angular Material

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y luego ingresar la contraseña del Administrador.

Ejecute los siguientes comandos:
```
cd daos-fakestore-products
ng add @angular/material
```

A continuación, le mostrará diferentes opciones y debe escoger las siguientes:

*Would you like to proceed?*, digite:
```
Y
```

*Choose a prebuilt theme name, or "custom" for a custom theme*, seleccione:
```
Azure/Blue   [Preview: https://material.angular.io?theme=azure-blue]
```

*Set up global Angular Material typography styles?*, digite:
```
Y
```

*Include the Angular animations module?*, seleccione:
```
Include and enable animations
```

### Agregando Internationalization en/es

Ejecute el siguiente comando:

```
npm install @ngx-translate/core @ngx-translate/http-loader --save
```

### Liberar el proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**


Ejecute los siguientes comandos:
```
cd ..
ls -l
sudo chown -R alumnos ./si729-ws53-fakestore-products
ls -l
```

## Desarrollo del proyecto

Cargar el IntelliJ IDEA y abra el proyecto ubicado en su carpeta de preferencia.

Cargar el Terminal del IDE y ejecutar el siguiente comando:
```
ng serve --port 4200
```

### Creación de Archivos de idiomas

En la carpeta :file_folder: `public` ubicado en la raiz del proyecto, crear las siguientes carpetas: `assets` e `i18n`:

```markdown
- 📂 public
  - 📁 assets
    - 📁 i18n
```

Dentro de la carpeta :file_folder: `i18n` crear los siguientes archivos `en.json` y `es.json` con el siguiente contenido:

#### en.json

```json
{
  "footer": {
    "intro": "Developed ",
    "author": "by {{ team }} Team"
  }
}
```

#### es.json

```json
{
  "footer": {
    "intro": "Desarrollado ",
    "author": "por el Equipo {{ team }}"
  }
}
```

### HttpClient y provideHttpClient

**HttpClient**: 
Performs HTTP requests. This service is available as an injectable class, with methods to perform HTTP requests. Each request method has multiple signatures, and the return type varies based on the signature that is called (mainly the values of observe and responseType).

Más información en: https://angular.dev/api/common/http/HttpClient

**provideHttpClient**:
Configures Angular's HttpClient service to be available for injection.

Más información en: https://angular.dev/api/common/http/provideHttpClient


### Configuración del appConfig

A continuación se detalla las instrucciones para agregar el provide HttpClient al config 

**Agregar** los siguientes imports a la constante `appConfig` del archivo `app.config.ts` ubicado en la carpeta `/src/app`:
```typescript
import { HttpClient, provideHttpClient } from "@angular/common/http";
import { importProvidersFrom } from '@angular/core';
import { TranslateLoader, TranslateModule } from "@ngx-translate/core";
import { TranslateHttpLoader } from "@ngx-translate/http-loader";
```

**Agregar** la function  `HttpLoaderFactory` despues de los imports.

```ts
export function HttpLoaderFactory(http: HttpClient) {
  return new TranslateHttpLoader(http);
}
```

**Agregar** los siguientes métodos al array `providers` de la constante `appConfig` ubicado en el archivo `app.config.ts`:
```
provideHttpClient(),
importProvidersFrom(
  TranslateModule.forRoot({
    loader: {
      provide: TranslateLoader,
      useFactory: HttpLoaderFactory,
      deps: [HttpClient]
    }
  })
)
```


### Creación de componentes

En la carpeta :file_folder: `app` ubicado dentro de la carpeta `src`, **crear** la siguiente estrutura de carpetas:

```markdown
- 📂 src
  - 📁 app
    - 📁 products
      - 📁 components
      - 📁 model
      - 📁 services
    - 📁 public
      - 📁 components
```

Cargue el Terminal del IDE y agregue un nuevo `Tab`.

**Ejecute** los siguientes comandos para la creación de los componentes: `header-content, footer-content, language-switcher, product-list` (Ejecute los comandos uno a la vez):
```ts
ng generate component public/components/header-content --skip-tests=true
ng generate component public/components/footer-content --skip-tests=true
ng generate component public/components/language-switcher --skip-tests=true
ng generate component products/components/product-list --skip-tests=true
```

### Modificando el AppComponent

**Agregar** los siguientes `imports` a la clase `AppComponent` del archivo `app.component.ts` ubicado en la carpeta `/src/app`:
```ts
import { HeaderContentComponent } from './public/components/header-content/header-content.component';
import { FooterContentComponent } from './public/components/footer-content/footer-content.component';
import { ProductListComponent } from "./products/components/product-list/product-list.component";
import { TranslateService } from "@ngx-translate/core";
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `AppComponent` ubicado en el archivo `app.component.ts`

```
HeaderContentComponent, FooterContentComponent, ProductListComponent
```

**Agregar** el siguiente `constructor` a la clase `AppComponent`:

```ts
constructor(translate: TranslateService) {
  translate.setDefaultLang('en');
  translate.use('en');
}
```

**Reemplazar** el contenido del archivo `app.component.html` con:
```html
<app-header-content></app-header-content>
<app-product-list></app-product-list>
<app-footer-content></app-footer-content>
```

## Modificación del LanguageSwitcherComponent

**Agregar** los siguientes `imports` a la clase `LanguageSwitcherComponent` del archivo `language-switcher.component.ts` ubicado en la carpeta `/src/app/public/components/language-switcher`

```ts
import { TranslateService } from "@ngx-translate/core";
import { MatButtonToggle, MatButtonToggleGroup } from "@angular/material/button-toggle";
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `LanguageSwitcherComponent` ubicado en el archivo `language-switcher.component.ts`.

```
MatButtonToggle, MatButtonToggleGroup
```

**Reemplazar** el contenido de la clase `LanguageSwitcherComponent` ubicado en el archivo `language-switcher.component.ts`, con el siguiente contenido:

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
**Reemplazar** el contenido del archivo `language-switcher.component.html ` con el siguiente código:

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

### Modificación del HeaderContentComponent

**Agregar** los siguientes `imports` a la clase `HeaderContentComponent` del archivo `header-content.component.ts` ubicado en la carpeta `/src/app/public/components/header-content`
```ts
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';
import { MatToolbarModule } from '@angular/material/toolbar';
import { LanguageSwitcherComponent } from "../language-switcher/language-switcher.component";
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `HeaderContentComponent` ubicado en el archivo `header-content.component.ts`

```
MatToolbarModule, MatButtonModule, MatIconModule, LanguageSwitcherComponent
```

**Reemplazar** el contenido del archivo `header-content.component.html` con el siguiente código:
```html
<mat-toolbar color="primary">
  <button mat-icon-button class="example-icon" aria-label="Example icon-button with menu icon">
    <mat-icon>menu</mat-icon>
  </button>
  <span>Fake Store</span>
  <span class="example-spacer"></span>
  <button mat-icon-button class="example-icon favorite-icon" aria-label="Example icon-button with heart icon">
    <mat-icon>favorite</mat-icon>
  </button>
  <button mat-icon-button class="example-icon" aria-label="Example icon-button with share icon">
    <mat-icon>share</mat-icon>
  </button>
  <span>
    <app-language-switcher/>
  </span>
</mat-toolbar>
```

**Reemplazar** el contenido del archivo `header-content.component.css` con el siguiente código:
```css
.example-spacer {
  flex: 1 1 auto;
}
```

### Modificación del FooterContentComponent

**Agregar** el siguiente `import` a la clase `FooterContentComponent` del archivo `footer-content.component.ts` ubicado en la carpeta `/src/app/public/components/footer-content`:

```ts
import { TranslateModule } from "@ngx-translate/core";
```

**Agregar** la siguiente clase en el array `imports` del `@Component` de la clase `FooterContentComponent` ubicado en el archivo `footer-content.component.ts`:

```
TranslateModule
```

**Reemplazar** el contenido del archivo `footer-content.component.html` con el siguiente codigo:

```html
<footer style="background-color: #333; color: #fff; text-align: center; padding: 10px;">
  <p>Copyright © 2024 Fake Store API, All rights reserved.</p>
  <p>
    {{ 'footer.intro' | translate }}
    {{ 'footer.author' | translate: {team: 'DAOS'} }}
  </p>
</footer>
```

### Analizando el endpoint de fakestoreapi

Dirijase a la siguiente url: https://fakestoreapi.com y accede al resource: `Products`, o dirijase al endpoint: https://fakestoreapi.com/products

Evalue el json de respuesta:
```json
{
  "id": 1,
  "title": "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
  "price": 109.95,
  "description": "Your perfect pack for everyday use and walks in the forest. Stash your laptop (up to 15 inches) in the padded sleeve, your everyday",
  "category": "men's clothing",
  "image": "https://fakestoreapi.com/img/81fPKd-2AYL._AC_SL1500_.jpg",
  "rating": {
    "rate": 3.9,
    "count": 120
  }
}
```

### Creando el Model product y rating tipo entity

**Ejecute** los siguientes comandos para la creación de los modelos: `rating` y `product` (Ejecute los comandos uno a la vez):

```bash
ng generate class products/model/rating --type=entity --skip-tests=true
ng generate class products/model/product --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Rating` del archivo `rating.entity.ts` ubicado en la carpeta `/src/app/products/model`:
```ts
rate: number;
count: number;

constructor() {
  this.rate = 0;
  this.count = 0;
}
```

**Agregar** el siguiente `import` a la clase `Product` del archivo `product.entity.ts` ubicado en la carpeta `/src/app/products/model`:
```typescript
import { Rating } from './rating.entity';
```

**Agregar** los siguentes atributos y constructor a la clase `Product` del archivo `product.entity.ts` ubicado en la carpeta `/src/app/products/model`:
```ts
id: number;
title: string;
price: number;
description: string;
category: string;
image: string;
rating: Rating;

constructor() {
  this.id = 0;
  this.title = '';
  this.price = 0;
  this.description = '';
  this.category = '';
  this.image = '';
  this.rating = new Rating();
}
```



### Creando el Service FakestoreApi

**Creación del service** `fakestore-api`: Ejecute la siguiente instrucción:
```ts
ng generate service products/services/fakestore-api --skip-tests=true
```

**Agregar** los siguentes imports a la clase `FakestoreApiService` del archivo `fakestore-api.service.ts` ubicado en la carpeta `/src/app/products/services`:
```typescript
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
```

**Reemplazar** el contenido los atributos, constructor y métodos de la clase `FakestoreApiService` del archivo `fakestore-api.service.ts`
```typescript
private baseUrl = 'https://fakestoreapi.com'

constructor(private http: HttpClient) { }

getProducts(): Observable<any> {
  return this.http.get(`${this.baseUrl}/products`);
}
```

### Modificando el Component Product List

**Agregar** los siguientes imports a la clase `ProductListComponent` del archivo `product-list.component.ts` ubicado en la carpeta `/src/app/products/components/product-list`:
```typescript
import { MatCardModule } from '@angular/material/card';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { MatInputModule } from '@angular/material/input';
import { MatFormFieldModule } from '@angular/material/form-field';
import { Product } from "../../model/product.entity";
import { FakestoreApiService } from '../../services/fakestore-api.service';
```

**Agregar** las clases `MatFormFieldModule, MatInputModule, MatTableModule, MatCardModule` en el array `imports` del `@Component` de la clase `ProductListComponent` del archivo `product-list.component.ts`

**Reemplazar** el contenido los atributos, constructor y métodos de la clase `ProductListComponent` ubicado en el archivo `product-list.component.ts`
```typescript
products: Array<Product> = [];
displayedColumns: string[] = ['id', 'title', 'price', 'description', 'category', 'image'];
dataSource: any;

constructor(private fakestoreApiService: FakestoreApiService) {
}

applyFilter(event: Event) {
  const filterValue = (event.target as HTMLInputElement).value;
  this.dataSource.filter = filterValue.trim().toLowerCase();
}

ngOnInit(): void {
  this.fakestoreApiService.getProducts().subscribe((data: any) => {
    this.products = data;
    this.dataSource = new MatTableDataSource(this.products);
  });
}
```

**Reemplazar** el contenido del archivo `product-list.component.html` con:
```html
<mat-card>
  <mat-card-content>

    <mat-form-field>
      <mat-label>Filter</mat-label>
      <input matInput (keyup)="applyFilter($event)" placeholder="Ex. ium" #input>
    </mat-form-field>

    <table mat-table [dataSource]="dataSource" class="mat-elevation-z8">

      <ng-container matColumnDef="id">
        <th mat-header-cell *matHeaderCellDef> No. </th>
        <td mat-cell *matCellDef="let element"> {{element.id}} </td>
      </ng-container>

      <ng-container matColumnDef="title">
        <th mat-header-cell *matHeaderCellDef> Title </th>
        <td mat-cell *matCellDef="let element"> {{element.title}} </td>
      </ng-container>

      <ng-container matColumnDef="price">
        <th mat-header-cell *matHeaderCellDef> Price </th>
        <td mat-cell *matCellDef="let element"> {{element.price}} </td>
      </ng-container>

      <ng-container matColumnDef="description">
        <th mat-header-cell *matHeaderCellDef> Description </th>
        <td mat-cell *matCellDef="let element"> {{element.description}} </td>
      </ng-container>

      <ng-container matColumnDef="category">
        <th mat-header-cell *matHeaderCellDef> Category </th>
        <td mat-cell *matCellDef="let element"> {{element.category}} </td>
      </ng-container>

      <ng-container matColumnDef="image">
        <th mat-header-cell *matHeaderCellDef> Image </th>
        <td mat-cell *matCellDef="let element"><img  width="30px" height="30px" src="{{element.image}}">   </td>
      </ng-container>

      <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
      <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>

      <!-- Row shown when there is no matching data. -->
      <tr class="mat-row" *matNoDataRow>
        <td class="mat-cell" colspan="4">No data matching the filter "{{input.value}}"</td>
      </tr>
    </table>

  </mat-card-content>
</mat-card>
```

**Reemplazar** el contenido del archivo `product-list.component.css` con:
```css
table {
  width: 100%;
}

.mat-mdc-form-field {
  font-size: 14px;
  width: 100%;
}
```

## Actividad

Agregar el model `Rating` en el Html de respuesta. 


# Internationalization en/es

Cargar el Terminal del IDE y ejecutar la siguiente instrucción:

```
npm install @ngx-translate/core @ngx-translate/http-loader --save
````

## Archivos de idiomas

En la carpeta :file_folder: `public` ubicado en la raiz, crear las siguientes carpetas:

```markdown
- 📂 public
  - 📁 assets
    - 📁 i18n
```

Dentro de la carpeta :file_folder: `i18n` crear los siguientes archivos `en.json` y `es.json` con el siguiente contenido:

### en.json

```json
{
  "footer": {
    "intro": "Developed ",
    "author": "by {{ author }} Team"
  }
}
```

### es.json

```json
{
  "footer": {
    "intro": "Desarrollado ",
    "author": "por el Equipo {{ author }}"
  }
}
```
## Actualizando appConfig

**Agregar** los siguientes imports a la constante `appConfig` del archivo `app.config.ts` ubicado en la carpeta `/src/app`:

```ts
import { importProvidersFrom} from '@angular/core';
import { HttpClient } from "@angular/common/http";
import { TranslateHttpLoader } from "@ngx-translate/http-loader";
import { TranslateLoader, TranslateModule } from "@ngx-translate/core";
````

**Agregar** la function  `HttpLoaderFactory` despues de los imports.

```ts
export function HttpLoaderFactory(http: HttpClient) {
  return new TranslateHttpLoader(http);
}
```

**Agregar** el método `importProvidersFrom` al array `providers` de la constante `appConfig` ubicado en el archivo `app.config.ts`

```ts
importProvidersFrom(
  TranslateModule.forRoot({
    loader: {
      provide: TranslateLoader,
      useFactory: HttpLoaderFactory,
      deps: [HttpClient]
    }
  })
)
```

## Actualizando el AppComponent

**Agregar** el siguiente `import` a la clase `AppComponent` del archivo `app.component.ts` ubicado en la carpeta `/src/app`:

```ts
import {TranslateService} from "@ngx-translate/core";
```

**Agregar** el siguiente `constructor` a la clase `AppComponent`

```ts
constructor(translate: TranslateService) {
    translate.setDefaultLang('en');
    translate.use('en');
  }
```

## Creación del LanguageSwitcherComponent

**Creación del componente** `language-switcher`: Ejecute la siguiente instrucción:

```
ng generate component public/components/language-switcher --skip-tests=true
```

**Agregar** los siguientes `imports` a la clase `LanguageSwitcherComponent` del archivo `language-switcher.component.ts` ubicado en la carpeta `/src/app/public/components/language-switcher`

```ts
import {TranslateService} from "@ngx-translate/core";
import {MatButtonToggle, MatButtonToggleGroup} from "@angular/material/button-toggle";
```

**Agregar** las clases `MatButtonToggle, MatButtonToggleGroup` en el array `imports` del `@Component` de la clase `LanguageSwitcherComponent` ubicado en el archivo `language-switcher.component.ts`.

**Reemplazar** el contenido de los atributos, constructor y métodos de la clase `LanguageSwitcherComponent` ubicado en el archivo `language-switcher.component.ts`.

```ts
currentLang = 'en';
languages = ['en', 'es'];

constructor(private translate: TranslateService) {
this.currentLang = translate.currentLang;
}

useLanguage(language: string) {
this.translate.use(language);
}
```
**Reemplazar** el contenido del archivo `language-switcher.component.html ` con:

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

## Actualizando el HeaderContentComponent

**Agregar** el siguiente `import` a la clase `HeaderContentComponent` del archivo `header-content.component.ts` ubicado en la carpeta `/src/app/public/components/header-content`:

```ts
import {LanguageSwitcherComponent} from "../language-switcher/language-switcher.component";
```
**Agregar** la clase `LanguageSwitcherComponent` en el array `imports` del `@Component` de la clase `HeaderContentComponent`.

**Agregar** el elemento `<span>` despues del elemento `</button>` en el archivo `header-content.component.html`:

```html
<span>
  <app-language-switcher/>
</span>
```

## Actualizando el FooterContentComponent

**Agregar** el siguiente `import` a la clase `FooterContentComponent` del archivo `footer-content.component.ts` ubicado en la carpeta `/src/app/public/components/footer-content`:

```ts
import {TranslateModule} from "@ngx-translate/core";
```

**Agregar** la clase `TranslateModule` en el array `imports` del `@Component` de la clase `FooterContentComponent` ubicado en el archivo `header-content.component.ts`.

**Reemplazar** el contenido del archivo `footer-content.component.html` con:

```html
<footer style="background-color: #333; color: #fff; text-align: center; padding: 10px;">
  <p>Copyright © 2024 Fake Store API, All rights reserved.</p>
  <p>
    {{ 'footer.intro' | translate }}
    {{ 'footer.author' | translate: {author: 'DAOS'} }}
  </p>
</footer>
```


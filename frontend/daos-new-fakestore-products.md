# Proyecto Fake-store
 
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
ng new daos-fakestore-products-v2520
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
cd daos-fakestore-products-v2520
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
sudo chown -R alumnos ./daos-fakestore-products-v2520
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
  "filter": {
    "label": "Filter",
    "placeholder": "Enter text to filter"
  },
  "product": {
    "id": "No.",
    "title": "Title",
    "price": "Price",
    "description": "Description",
    "category": "Category",
    "image": "Image",
    "rate": "Rate",
    "count": "Count"
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
  "product": {
    "id": "Núm.",
    "title": "Título",
    "price": "Precio",
    "description": "Descripción",
    "category": "Categoria",
    "image": "Imagen",
    "rate": "calificación",
    "count": "Cantidad"
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
- 📂 daos-fakestore-products-v2520
  - 📁 server
```

**Crear** los archivo `db.json` y `routes.json` en la carpeta `server` con el siguiente contenido:

#### db.json
Copiar el contenido del archivo: [daos-new-fakestore-products-db.json](/frontend/jsons/daos-new-fakestore-products-db.json)


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
- http://localhost:3000/api/v1/products
- http://localhost:3000/api/v1/carts
- http://localhost:3000/api/v1/users


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
fakestoreProviderApiBaseUrl: 'http://localhost:3000/api/v1',
fakestoreProviderProductsEndpointPath: '/products',
fakestoreProviderCartsEndpointPath: '/carts',
fakestoreProviderUsersEndpointPath: '/users'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
fakestoreProviderApiBaseUrl: 'https://fakestoreapi.com',
fakestoreProviderProductsEndpointPath: '/products',
fakestoreProviderCartsEndpointPath: '/carts',
fakestoreProviderUsersEndpointPath: '/users'
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
    - 📂 iam
      - 📁 application
      - 📁 domain
        - 📁 model
      - 📁 infrastructure
      - 📁 presentation
        - 📁 components
    - 📂 sales
      - 📁 application
      - 📁 domain
        - 📁 model
      - 📁 infrastructure
      - 📁 presentation
        - 📁 components
    - 📂 shared
      - 📁 infrastructure
      - 📁 presentation
        - 📁 components
```

## Analizando Fake Store API

**Dirijase** al url: https://fakestoreapi.com/ y **revice** la documenación del endpoint `/products`, `/carts` y `/users` ubicada en la url https://fakestoreapi.com/docs

### Analizando el endpoint products

Dirijase al endpoint local http://localhost:3000/api/v1/products y **evalue** el json de respuesta. 

```json
[
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
  },
  {
    "id": 2,
    "title": "Mens Casual Premium Slim Fit T-Shirts ",
    "price": 22.3,
    "description": "Slim-fitting style, contrast raglan long sleeve, three-button henley placket, light weight & soft fabric for breathable and comfortable wearing. And Solid stitched shirts with round neck made for durability and a great fit for casual fashion wear and diehard baseball fans. The Henley style round neckline includes a three-button placket.",
    "category": "men's clothing",
    "image": "https://fakestoreapi.com/img/71-3HjGNDUL._AC_SY879._SX._UX._SY._UY_.jpg",
    "rating": {
      "rate": 4.1,
      "count": 259
    }
  }
]
```

### Creación de la interface Product tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Product` tipo `response`:

```bash
ng generate interface sales/infrastructure/product-response
```

**Reemplazar** el contenido de la interface `ProductResponse` del archivo `product-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/sales/infrastructure`:

```ts
export interface ProductResponse {
  id: number;
  title: string;
  price: number;
  description: string;
  category: string;
  image: string;
  rating: {
    rate: number;
    count: number;
  };
}
```

### Creación del class Rating y Product tipo entity (model)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `rating`:

```bash
ng generate class sales/domain/model/rating --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Rating` del archivo `rating.entity.ts` ubicado en la carpeta `/src/app/sales/domain/model`:

```ts
rate: number;
count: number;

constructor() {
  this.rate = 0.0;
  this.count = 0;
}
```

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `product`:

```bash
ng generate class sales/domain/model/product --type=entity --skip-tests=true
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
  this.price = 0.0;
  this.description = '';
  this.category = '';
  this.image = '';
  this.rating = new Rating();
}
```

### Analizando el endpoint carts

Dirijase al endpoint local http://localhost:3000/api/v1/carts y **evalue** el json de respuesta. 

```json
[
  {
    "id": 1,
    "userId": 1,
    "date": "2020-03-02T00:00:00.000Z",
    "products": [
      {
        "productId": 1,
        "quantity": 4
      },
      {
        "productId": 2,
        "quantity": 1
      },
      {
        "productId": 3,
        "quantity": 6
      }
    ],
    "__v": 0
  },
  {
    "id": 2,
    "userId": 1,
    "date": "2020-01-02T00:00:00.000Z",
    "products": [
      {
        "productId": 2,
        "quantity": 4
      },
      {
        "productId": 1,
        "quantity": 10
      },
      {
        "productId": 5,
        "quantity": 2
      }
    ],
    "__v": 0
  }
]
```

### Creación de la interface Cart tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Cart` tipo `response`:

```bash
ng generate interface sales/infrastructure/cart-response
```

**Reemplazar** el contenido de la interface `CartResponse` del archivo `cart-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/sales/infrastructure`:

```ts
export interface CartResponse {
  id: number;
  userId: number;
  date: string;
  products: ProductResource[];
  __v: number;
}

export interface ProductResource {
  productId: number;
  quantity: number;
}
```

### Creación del class CartDetail y Cart tipo entity (model)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `cart-detail`:

```bash
ng generate class sales/domain/model/cart-detail --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `CartDetail` del archivo `cart-detail.entity.ts` ubicado en la carpeta `/src/app/sales/domain/model`:

```ts
productId: number;
quantity: number;

constructor() {
  this.productId = 0;
  this.quantity = 0;
}
```

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `cart`:

```bash
ng generate class sales/domain/model/cart --type=entity --skip-tests=true
```

**Agregar** el siguiente `import` a la clase `CartDetail` del archivo `cart-detail.entity.ts` ubicado en la carpeta `/src/app/sales/domain/model`:

```typescript
import { CartDetail } from './cart-detail.entity';
```

**Agregar** los siguentes atributos y constructor a la clase `Cart` del archivo `cart.entity.ts` ubicado en la carpeta `/src/app/sales/domain/model`:

```ts
id: number;
userId: number;
date: string;
cartDetails: CartDetail[];
__v: number;

constructor() {
  this.id = 0;
  this.userId = 0;
  this.date = '';
  this.cartDetails = new CartDetail[];
  this.__v = 0;
}
```

### Creación del class Product tipo Assembler (DTO Assembler)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Product` tipo `Assembler`:

```bash
ng generate class sales/infrastructure/product-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `product-assembler.ts`, ubicado en la carpeta `/src/app/sales/infrastructure`:
```typescript
import { ProductResponse } from './product-response';
import { Product } from '../domain/model/product.entity';
```

**Reemplazar** el contenido de la clase `ProductAssembler` con el siguiente código, ubicado en el archivo `product-assembler.ts`:

```ts
static toEntityFromResponseArray(responseArray: ProductResponse[]): Product[] {
  return responseArray.map((response) =>
    this.toEntityFromResponse(response));
}
static toEntityFromResponse(response: ProductResponse): Product {
  return {
    id: response.id,
    title: response.title,
    price: response.price,
    description: response.description,
    category: response.category,
    image: response.image,
    rating: {
      rate: response.rating.rate,
      count: response.rating.count
    }
  };
}
```

### Creación del FakestoreApi Service

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `fakestore-api`:

```bash
ng generate service sales/infrastructure/fakestore-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `fakestore-api.ts`, ubicado en la carpeta `/src/app/sales/infrastructure`:
```typescript
iimport { inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { map, Observable } from 'rxjs';
import { environment } from '../../../environments/environment';
import { Product } from '../domain/model/product.entity';
import { ProductResponse } from './product-response';
import { ProductAssembler } from './product-assembler';
```

**Reemplazar** el contenido de la clase `FakestoreApi` con el siguiente código, ubicado en el archivo `fakestore-api.ts`:

```ts
private baseUrl = environment.fakestoreProviderApiBaseUrl
private productsEndpoint = environment.fakestoreProviderProductsEndpointPath;

constructor(private http: HttpClient) { }

getProducts(): Observable<Product[]> {
  return this.http.get<ProductResponse[]>(`${this.baseUrl}${this.productsEndpoint}`)
    .pipe(
      map(responseArray =>
        ProductAssembler.toEntityFromResponseArray(responseArray))
  );
}
```

### Application layer : Creación del Service NewsStore

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `news-store`:

```bash
ng generate service sales/application/fakestore-app --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `fakestore-app.ts`, ubicado en la carpeta `/src/app/sales/application`:
```typescript
import { computed, inject, signal } from '@angular/core';
import { Source } from '../domain/model/source.entity';
import { Article } from '../domain/model/article.entity';
import { NewsApi } from '../infrastructure/news-api';
import { LogoApi } from '../../shared/infrastructure/logo-api';
```

**Reemplazar** el contenido de la clase `FakestoreApp` con el siguiente código, ubicado en el archivo `fakestore-app.ts`:

```ts
private sourcesSignal = signal<Source[]>([]);
private articlesSignal = signal<Record<string, Article[]>>({});
private newsApi = inject(NewsApi);
private logoApi = inject(LogoApi);

readonly sources = computed(() => this.sourcesSignal());
readonly articles = computed(() => this.articlesSignal());
public currentSourceArticles = computed(() => this.articlesSignal()[this.currentSource?.id] ?? []);
private _currentSource!: Source;

loadSources() {
  if (this.sourcesSignal().length === 0) {
    this.newsApi.getSources().subscribe(sources => {
      sources.forEach(source => source.urlToLogo = this.logoApi.getUrlToLogo(source));
      this.sourcesSignal.set(sources);
      this.currentSource = sources[0];
      this.loadArticlesForCurrentSource();
    });
  }
}

loadArticlesForCurrentSource() {
  console.log(this.currentSource);
  const current = this.articlesSignal() ?? {};
  const source = this._currentSource;
  if (!current[source.id]) {
    this.newsApi.getArticlesBySourceId(source.id).subscribe(articles => {
      articles.forEach(article => {
        article.source.urlToLogo = source.urlToLogo;
        article.source.url = source.url;
      });
      this.articlesSignal.set({ ...current, [source.id]: articles });
    });
  }
}

get currentSource(): Source {
  return this._currentSource;
}

set currentSource(value: Source) {
  this._currentSource = value;
  this.loadArticlesForCurrentSource();
}
```


## Creación de componentes

**Cargar** el `Terminal` del IDE y agregue un nuevo `Tab`.

**Ejecute** los siguientes comandos para la creación de los componentes: `header-content, footer-content, language-switcher, product-list` (Ejecute los comandos uno a la vez):

```bash
ng generate component shared/presentation/components/header-content --skip-tests=true
```
```bash
ng generate component shared/presentation/components/footer-content --skip-tests=true
```
```bash
ng generate component shared/presentation/components/language-switcher --skip-tests=true
```
```bash
ng generate component shared/presentation/components/layout --skip-tests=true
```
```bash
ng generate component sales/presentation/components/product-list --skip-tests=true
```

### Modificación del LanguageSwitcherComponent

**Agregar** los siguientes `import` al archivo `language-switcher.ts`, ubicado en la carpeta `/src/app/shared/presentation/components/language-switcher`:

```ts
import { TranslateService } from '@ngx-translate/core';
import { MatButtonToggle, MatButtonToggleGroup } from '@angular/material/button-toggle';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `LanguageSwitcher`:

```
MatButtonToggleGroup, MatButtonToggle
```

**Reemplazar** el contenido de la clase `LanguageSwitcher` con el siguiente código, ubicado en el archivo `language-switcher.ts`:

```ts
currentLang = 'en';
languages = ['en', 'es'];

constructor(private translate: TranslateService) {
  this.currentLang = translate.getCurrentLang();
}

useLanguage(language: string) {
  this.translate.use(language);
}
```

**Reemplazar** el contenido del archivo `language-switcher.html` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/language-switcher`:

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

**Agregar** los siguientes `imports` a la clase `HeaderContent` del archivo `header-content.ts` ubicado en la carpeta `/src/app/shared/presentation/components/header-content`
```ts
import { MatIconModule } from '@angular/material/icon';
import { MatButtonModule } from '@angular/material/button';
import { MatToolbarModule } from '@angular/material/toolbar';
import { LanguageSwitcher } from "../language-switcher/language-switcher";
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `HeaderContentComponent` ubicado en el archivo `header-content.component.ts`

```
MatToolbarModule, MatButtonModule, MatIconModule, LanguageSwitcher
```

**Reemplazar** el contenido del archivo `header-content.component.html` con el siguiente código:
```html
<mat-toolbar color="primary">
  <button mat-icon-button class="example-icon" aria-label="Example icon-button with menu icon">
    <mat-icon>menu</mat-icon>
  </button>
  <span>New Fake Store</span>
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

### Modificación del FooterContent Component

**Agregar** el siguiente `import` a la clase `Footer` del archivo `footer.ts` ubicado en la carpeta `/src/app/shared/presentation/components/footer`:

```ts
import { TranslateModule } from "@ngx-translate/core";
```

**Agregar** la siguiente clase en el array `imports` del `@Component` de la clase `Footer`:

```
TranslateModule
```

**Reemplazar** el contenido del archivo `footer.html` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/footer`:

```html
<div class="footer-content">
  <p>{{ 'footer.rights' | translate: {api: 'News API'} }}</p>
  <p>
    {{ 'footer.intro' | translate }}
    {{ 'footer.author' | translate: {team: 'DAOS'} }}
  </p>
</div>
```

**Reemplazar** el contenido del archivo `footer.css` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/footer-content`:

```css
.footer-content {
  bottom: 0;
  width: 100%;
  height: 90px;
  background-color: #3f51b5;
  color: white;
  text-align: center;
  margin: 0;
  padding: 5px;
}
```

### Modificación del Layout Component

**Agregar** los siguientes `import` al archivo `layout.ts`, ubicado en la carpeta `/src/app/shared/presentation/components/layout`:

```ts
import {inject, OnInit, Signal} from '@angular/core';
import {HeaderContent} from '../header-content/header-content';
import {FooterContent} from '../footer-content/footer-content';
import {ProductList} from '../../../../sales/presentation/components/product-list/product-list';
import {FakestoreApp} from '../../../../sales/application/fakestore-app';
import {Product} from '../../../../sales/domain/model/product.entity';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `Layout`:

```
HeaderContent, FooterContent, ProductList
```

**Agregar** la interface `OnInit` a la clase `Layout`: 

```
implements OnInit
```

**Reemplazar** el contenido de la clase `Layout` con el siguiente código:

```ts
protected fakestoreApp = inject(FakestoreApp);
protected readonly products: Signal<Product[]> = this.fakestoreApp.products;

ngOnInit(): void {
  console.log("init layout");
  this.fakestoreApp.loadProducts();
}
```

**Reemplazar** el contenido del archivo `layout.html` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/layout`:

```html
<app-header-content></app-header-content>
<app-product-list [products]="products()"></app-product-list>
<app-footer-content></app-footer-content>
```


### Modificación del ProductList Component

**Agregar** los siguientes imports a la clase `ProductList` del archivo `product-list.ts` ubicado en la carpeta `/src/app/sales/presentation/components/product-list`:
```ts
import { input, output} from '@angular/core';
import { MatCardModule } from '@angular/material/card';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { MatInputModule } from '@angular/material/input';
import { MatFormFieldModule } from '@angular/material/form-field';
import { TranslateModule } from "@ngx-translate/core";
import { Product } from '../../../domain/model/product.entity';
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `ProductListComponent` del archivo `product-list.component.ts`:

```
MatFormFieldModule, MatInputModule, MatTableModule, MatCardModule, TranslateModule
```

**Reemplazar** el contenido de la clase `ProductListComponent` ubicado en el archivo `product-list.component.ts` con el siguiente código:

```ts
products = input<Product[]>();

displayedColumns: string[] = ['id', 'title', 'price', 'description', 'category', 'image'];
//dataSource = new MatTableDataSource(this.products);
dataSource = new MatTableDataSource(this.products());

constructor() {
  console.log("constructor product-list");
  console.log(this.products());
}

applyFilter(event: Event) {
  const filterValue = (event.target as HTMLInputElement).value;
  this.dataSource.filter = filterValue.trim().toLowerCase();
}
```

**Reemplazar** el contenido del archivo `product-list.component.html` con el siguiente código:
```html
<mat-card>
  <mat-card-content>

    <mat-form-field>
      <mat-label>{{ 'filter.label' | translate }}</mat-label>
      <input matInput (keyup)="applyFilter($event)" placeholder="{{ 'filter.placeholder' | translate }}" #input>
    </mat-form-field>

    <table mat-table [dataSource]="dataSource" class="mat-elevation-z8">

      <ng-container matColumnDef="id">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.id' | translate }}</th>
        <td mat-cell *matCellDef="let element"> {{element.id}} </td>
      </ng-container>

      <ng-container matColumnDef="title">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.title' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.title}} </td>
      </ng-container>

      <ng-container matColumnDef="price">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.price' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.price}} </td>
      </ng-container>

      <ng-container matColumnDef="description">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.description' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.description}} </td>
      </ng-container>

      <ng-container matColumnDef="category">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.category' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.category}} </td>
      </ng-container>

      <ng-container matColumnDef="image">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.image' | translate }} </th>
        <td mat-cell *matCellDef="let element"><img  width="50px" height="50px" src="{{element.image}}">   </td>
      </ng-container>

      <ng-container matColumnDef="rate">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.rate' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.rating.rate}} </td>
      </ng-container>

      <ng-container matColumnDef="count">
        <th mat-header-cell *matHeaderCellDef> {{ 'product.count' | translate }} </th>
        <td mat-cell *matCellDef="let element"> {{element.rating.count}} </td>
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

**Reemplazar** el contenido del archivo `product-list.component.css` con el siguiente código:
```css
table {
  width: 100%;
}

.mat-mdc-form-field {
  font-size: 14px;
  width: 100%;
}
```


### Modificación del App

**Agregar** los siguientes `import` al archivo `app.ts`, ubicado en la carpeta `/src/app`:

```ts
import { Layout } from './shared/presentation/components/layout/layout';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `App`, ubicado en el archivo `app.ts`

```ts
Layout
```

**Reemplazar** el contenido del archivo `app.html` con el siguiente código, ubicado en la carpeta `/src/app`:

```html
<app-layout/>
<router-outlet />
```


## Actividad

**Modificar** el `ProductListComponent` para incluir los atributos `rate` y `count` del modelo **Rating**.

# Catch Up Project

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
ng new daos-catch-up-v2520
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
cd daos-catch-up-v2520
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

### Cambiar el propietario del proyecto creado con sudo (Solo MAC)

> [!CAUTION]
> **Solo ejecutar si estas en un equipo MAC:**


**Ejecutar** los siguientes commands:
```
cd ..
```

```
sudo chown -R alumnos ./daos-catch-up-v2520
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
  "app": {
    "title": "CatchUp News"
  },
  "sources": {
    "title": "News Sources"
  },
  "articles": {
    "title": "Top Headlines"
  },
  "article": {
    "read-more": "Read more"
  },
  "footer": {
    "rights": "Copyright © 2025 {{ api }}, All rights reserved",
    "intro": "Powered by NewsAPI.org and Clearbit Logo API ",
    "author": "and {{ team }} Team"
  }
}
```

#### es.json
```json
{
  "app": {
    "title": "CatchUp News"
  },
  "sources": {
    "title": "Fuentes de Noticias"
  },
  "articles": {
    "title": "Titulares Principales"
  },
  "article": {
    "read-more": "Leer más"
  },
  "footer": {
    "rights": "Derechos de autor © 2025 {{ api }}, Todos los derechos reservados",
    "intro": "Desarrollado por NewsAPI.org y Clearbit Logo API y ",
    "author": "por el Equipo {{ team }}"
  }
}
```

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
newsProviderApiBaseUrl: 'https://newsapi.org/v2',
newsProviderNewsEndpointPath: '/top-headlines',
newsProviderSourcesEndpointPath: '/sources',
newsProviderApiKey: '8a86875d83d94cc5acb05de9cfd89940jjj',
logoProviderApiBaseUrl: 'https://logo.clearbit.com/'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
newsProviderApiBaseUrl: 'https://newsapi.org/v2',
newsProviderNewsEndpointPath: '/top-headlines',
newsProviderSourcesEndpointPath: '/sources',
newsProviderApiKey: '8a86875d83d94cc5acb05de9cfd89940jjj',
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
import { provideHttpClient } from '@angular/common/http';
import { provideTranslateService } from '@ngx-translate/core';
import { provideTranslateHttpLoader } from '@ngx-translate/http-loader';
```

**Agregar** los siguientes métodos al array `providers` de la constante `appConfig` del archivo `app.config.ts` :
```ts
provideHttpClient(),
provideTranslateService({
  loader: provideTranslateHttpLoader({prefix: './assets/i18n/', suffix: '.json'}),
  fallbackLang: 'en'
})
```

### Creación de la estructura del proyecto

**Crear** la siguiente estrutura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📂 app
    - 📂 news
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


### Analizando News API

**Dirijase** al url: https://newsapi.org/ y **registrese** para obtener un `API key` ingresando al siguiente url: https://newsapi.org/register

Llene sus datos y registrese. 

Después del registro, obtendrá un `API key` el cual debe reemplazarlo en la variable `newsProviderApiKey` de los archivos: `environment.development.ts` y `environment.ts` ubicado en la carpeta `/src/environments`.


### Analizando el endpoint de sources

**Reemplazar** el valor de `xxxyyzzz` en la url por el apiKey obtenido previamente.

Dirijase al endpoint: https://newsapi.org/v2/sources?apiKey=xxxyyzzz y **evaluar** el json de respuesta:

```json
{
  "status": "ok",
  "sources": [
    {
      "id": "abc-news",
      "name": "ABC News",
      "description": "Your trusted source for breaking news, analysis, exclusive interviews, headlines, and videos at ABCNews.com.",
      "url": "https://abcnews.go.com",
      "category": "general",
      "language": "en",
      "country": "us"
    },
    {
      "id": "abc-news-au",
      "name": "ABC News (AU)",
      "description": "Australia's most trusted source of local, national and world news. Comprehensive, independent, in-depth analysis, the latest business, sport, weather and more.",
      "url": "https://www.abc.net.au/news",
      "category": "general",
      "language": "en",
      "country": "au"
    }
  ]
}
```

### Creación de la interface Source tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `Source`:

```bash
ng generate interface news/infrastructure/sources-response
```

**Reemplazar** el contenido de la interface `Source` del archivo `sources-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/news/infrastructure`:

```ts
export interface SourcesResponse {
  status: string;
  sources: SourceResource[];
}

export interface SourceResource {
  id: string;
  name: string;
  url: string;
  urlToLogo: string;
}
```

### Creación del model Source tipo entity

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `source`:

```bash
ng generate class news/domain/model/source --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Source` del archivo `source.entity.ts`, ubicado en la carpeta `/src/app/news/domain/model`:

```ts
id: string;
name: string;
url: string;
urlToLogo: string;

constructor() {
  this.id = '';
  this.name = '';
  this.url = '';
  this.urlToLogo = '';
}
```

### Analizando el endpoint de top-headlines

**Reemplazar** el valor de `xxxyyzzz` en la url por el apiKey obtenido previamente.

Dirijase al endpoint: https://newsapi.org/v2/top-headlines?sources=abc-news&apiKey=xxxyyzzz y **evaluar** el json de respuesta:

```json
{
  "status": "ok",
  "totalResults": 10,
  "articles": [
    {
      "source": {
        "id": "abc-news",
        "name": "ABC News"
      },
      "author": "The Associated Press",
      "title": "India's northeast is hit by a 5.8 magnitude earthquake but no damage is reported",
      "description": "An earthquake has struck India’s northeastern Assam state, sending tremors throughout the region",
      "url": "https://abcnews.go.com/International/wireStory/indias-northeast-hit-58-magnitude-earthquake-damage-reported-125553033",
      "urlToImage": "https://s.abcnews.com/images/US/abc_news_default_2000x2000_update_16x9_992.jpg",
      "publishedAt": "2025-09-14T12:47:45Z",
      "content": "NEW DELHI -- A 5.8 magnitude earthquake struck Indias northeastern Assam state on Sunday, sending tremors throughout the region, officials said. There were no immediate reports of any damage.\r\nThe qu… [+424 chars]"
    },
    {
      "source": {
        "id": "abc-news",
        "name": "ABC News"
      },
      "author": "The Associated Press",
      "title": "Ricky Hatton, former world boxing champion, dies at 46",
      "description": "Former world boxing champion Ricky Hatton, who rose to become one of the most popular fighters in the sport, has died",
      "url": "https://abcnews.go.com/Sports/wireStory/ricky-hatton-former-world-boxing-champion-dies-46-125553122",
      "urlToImage": "https://i.abcnewsfe.com/a/ad0dbae0-433d-4bed-97a4-04aab968e402/wirestory_e306374b99c884a1005c6aef50cecf32_16x9.jpg?w=1600",
      "publishedAt": "2025-09-14T12:15:14Z",
      "content": "MANCHESTER, England -- Former world boxing champion Ricky Hatton, who rose to become one of the most popular fighters in the sport, has died. He was 46.\r\nHatton was found dead at his home in Greater … [+1985 chars]"
    }
  ]
}
```

### Creación de la interface TopHeadlines tipo Response

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la interface `TopHeadlines`:

```bash
ng generate interface news/infrastructure/top-headlines-response
```

**Reemplazar** el contenido de la interface `TopHeadlines` del archivo `top-headlines-response.ts` con el siguiente código, ubicado en la carpeta `/src/app/news/infrastructure`:

```ts
export interface TopHeadlinesResponse {
  status: string;
  totalResults: number;
  articles: ArticleResource[];
}

export interface ArticleResource {
  source: { 
    id: string | null; 
    name: string 
  };
  title: string;
  description: string | null;
  url: string;
  urlToImage: string | null;
  publishedAt: string;
}
```

### Creación del model Article tipo entity

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el modelo `source`:

```bash
ng generate class news/domain/model/article --type=entity --skip-tests=true
```

**Agregar** el siguiente `import` al archivo `article.entity.ts`, ubicado en la carpeta `/src/app/news/model`:
```ts
import {Source} from './source.entity';
```

**Agregar** los siguentes atributos y constructor a la clase `Article` del archivo `article.entity.ts`, ubicado en la carpeta `/src/app/news/model`:

```ts
title: string;
description: string;
url: string;
urlToImage: string;
publishedAt: string;
source: Source;
constructor() {
  this.title = '';
  this.description = '';
  this.url = '';
  this.urlToImage = '';
  this.publishedAt = '';
  this.source = new Source();
}
```

### Creación del Service LogoApi

Genera la url que permite obtener el logotipo de una web, ejemplo: https://logo.clearbit.com/www.google.com 


**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `logo-api`:

```bash
ng generate service shared/infrastructure/logo-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `logo-api.ts`, ubicado en la carpeta `/src/app/shared/infrastructure`:
```typescript
import { environment } from '../../../environments/environment';
import { Source } from '../../news/domain/model/source.entity';
```

**Reemplazar** el contenido de la clase `LogoApi` con el siguiente código, ubicado en el archivo `logo-api.ts`:

```ts
baseUrl = environment.logoProviderApiBaseUrl;

constructor() { }

getUrlToLogo(source: Source) {
  console.log('getUrlToLogo', source);
  return `${this.baseUrl}${new URL(source.url).hostname}`;
}
```

### Creación de la clase SourceAssembler

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Source` tipo `Assembler`:

```bash
ng generate class news/infrastructure/source-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `source-assembler.ts`, ubicado en la carpeta `/src/app/news/infrastructure`:
```typescript
import { LogoApi } from '../../shared/infrastructure/logo-api';
import { SourceResource, SourcesResponse } from './sources-response';
import { Source } from '../domain/model/source.entity';
```

**Reemplazar** el contenido de la clase `SourceAssembler` con el siguiente código, ubicado en el archivo `source-assembler.ts`:

```ts
static logoApi: LogoApi;
static withLogoApi(logoApi: LogoApi) {
  this.logoApi = logoApi;
  return this;
}
static toEntityFromResource(resource: SourceResource): Source {
  return {
    id: resource.id,
    name: resource.name,
    url: resource.url || '',
    urlToLogo: this.logoApi.getUrlToLogo(resource)
  };
}

static toEntitiesFromResponse(response: SourcesResponse): Source[] {
  return response.sources.map(source => this.toEntityFromResource(source));
}
```

### Creación de la clase Article tipo Assembler

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Article` tipo `Assembler`:

```bash
ng generate class news/infrastructure/article-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `article-assembler.ts`, ubicado en la carpeta `/src/app/news/infrastructure`:
```typescript
import { LogoApi } from '../../shared/infrastructure/logo-api';
import { ArticleResource, TopHeadlinesResponse } from './top-headlines-response';
import { Article } from '../domain/model/article.entity';
```

**Reemplazar** el contenido de la clase `ArticleAssembler` con el siguiente código, ubicado en el archivo `article-assembler.ts`:

```ts
static logoApi: LogoApi;
static withLogoApi(logoApi: LogoApi) {
  this.logoApi = logoApi;
  return this;
}
static toEntityFromResource(resource: ArticleResource): Article {

  return {
    source: {
      id: resource.source.id || '',
      name: resource.source.name,
      url: '',
      urlToLogo: ''
    },
    title: resource.title,
    description: resource.description || '',
    url: resource.url,
    urlToImage: resource.urlToImage || '',
    publishedAt: resource.publishedAt
  };
}

static toEntitiesFromResponse(response: TopHeadlinesResponse): Article[] {
  return response.articles.map(article => this.toEntityFromResource(article));
}
```

### Creación del Service NewsApi

Consume las APIs de https://newsapi.org


**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `news-api`:

```bash
ng generate service news/infrastructure/news-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `news-api.ts`, ubicado en la carpeta `/src/app/news/infrastructure`:
```typescript
import { inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { LogoApi } from '../../shared/infrastructure/logo-api';
import { map, Observable } from 'rxjs';
import { Source } from '../domain/model/source.entity';
import { SourcesResponse } from './sources-response';
import { environment } from '../../../environments/environment';
import { SourceAssembler } from './source-assembler';
import { Article } from '../domain/model/article.entity';
import { TopHeadlinesResponse } from './top-headlines-response';
import { ArticleAssembler } from './article-assembler';
```

**Reemplazar** el contenido de la clase `NewsApi` con el siguiente código, ubicado en el archivo `news-api.ts`:

```ts
private baseUrl = environment.newsProviderApiBaseUrl;
private newsEndpoint = environment.newsProviderNewsEndpointPath;
private sourcesEndpoint = environment.newsProviderSourcesEndpointPath;
private apiKey = environment.newsProviderApiKey;
private http = inject(HttpClient);
private logoApi = inject(LogoApi);

getSources(): Observable<Source[]> {
  return this.http.get<SourcesResponse>(`${this.baseUrl}${this.sourcesEndpoint}`, {
    params: { apiKey: this.apiKey }
  }).pipe(
    map(response => SourceAssembler.withLogoApi(this.logoApi)
        .toEntitiesFromResponse(response))
  );
}

getArticlesBySourceId(sourceId: string): Observable<Article[]> {
  return this.http.get<TopHeadlinesResponse>(`${this.baseUrl}${this.newsEndpoint}`, {
    params: { apiKey: this.apiKey, sources: sourceId }
  }).pipe(
    map(response => ArticleAssembler.withLogoApi(this.logoApi)
        .toEntitiesFromResponse(response))
  );
}
```

### Application layer : Creación del Service NewsStore

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `news-store`:

```bash
ng generate service news/application/news-store --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `logo-api.ts`, ubicado en la carpeta `/src/app/shared/infrastructure`:
```typescript
import { computed, inject, signal } from '@angular/core';
import { Source } from '../domain/model/source.entity';
import { Article } from '../domain/model/article.entity';
import { NewsApi } from '../infrastructure/news-api';
import { LogoApi } from '../../shared/infrastructure/logo-api';
```

**Reemplazar** el contenido de la clase `NewsStore` con el siguiente código, ubicado en el archivo `news-store.ts`:

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


### Creación de componentes

**Cargar** el `Terminal` del IDE y **agregar** un nuevo `Tab`.

**Ejecutar** los siguientes CLI commands para la creación de los componentes (Ejecute los comandos uno a la vez):
```bash
ng generate component news/components/article-item --skip-tests=true
```
```bash
ng generate component news/components/article-list --skip-tests=true
```
```bash
ng generate component news/components/source-item --skip-tests=true
```
```bash
ng generate component news/components/source-list --skip-tests=true
```
```bash
ng generate component public/components/footer-content --skip-tests=true
```
```bash
ng generate component public/components/language-switcher --skip-tests=true
```
```bash
ng generate component public/components/side-navigation-bar --skip-tests=true
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
import { TranslateService } from '@ngx-translate/core';
import { SideNavigationBarComponent } from './public/components/side-navigation-bar/side-navigation-bar.component';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `AppComponent`, ubicado en el archivo `app.component.ts`

```ts
SideNavigationBarComponent
```

**Reemplazar** el contenido de la clase `AppComponent` con el siguiente código, ubicado en el archivo `app.component.ts`

```ts
title = 'catch-up';
constructor(private translate: TranslateService) {
  this.translate.addLangs(['en', 'es']);
  this.translate.setDefaultLang('en');
  this.translate.use('en');
}
```

**Reemplazar** el contenido del archivo `app.component.html` con el siguiente código, ubicado en la carpeta `/src/app`:

```html
<app-side-navigation-bar></app-side-navigation-bar>
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

**Reemplazar** el contenido del archivo `footer-content.component.html` con el siguiente código, ubicado en la carpeta `/src/app/public/components/footer-content`:

```html
<div class="footer-content">
  <p>{{ 'footer.rights' | translate: {api: 'News API'} }}</p>
  <p>
    {{ 'footer.intro' | translate }}
    {{ 'footer.author' | translate: {team: 'DAOS'} }}
  </p>
</div>
```

**Reemplazar** el contenido del archivo `footer-content.component.css` con el siguiente código, ubicado en la carpeta `/src/app/public/components/footer-content`:

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

### Modificación del SourceItemComponent

**Agregar** los siguientes `import` al archivo `source-item.component.ts`, ubicado en la carpeta `/src/app/news/components/source-item`:

```ts
import { EventEmitter, Input, Output } from '@angular/core';
import { Source } from '../../model/source.entity';
import { MatListItem } from '@angular/material/list';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `SourceItemComponent`, ubicado en el archivo `source-item.component.ts`:

```ts
MatListItem
```

**Reemplazar** el contenido de la clase `SourceItemComponent` con el siguiente código, ubicado en el archivo `source-item.component.ts`:

```ts
@Input() source!: Source;
@Output() sourceSelected = new EventEmitter<Source>();

onClick() {
  this.sourceSelected.emit(this.source);
}
```

**Reemplazar** el contenido del archivo `source-item.component.html` con el siguiente código, ubicado en la carpeta `/src/app/news/components/source-item`:

```html
<mat-list-item (click)="onClick()" style="margin: 10px; padding: 10px; cursor: pointer; align-items: center;">
  <img matListItemAvatar class="avatar" [alt]="source.name" [src]="source.urlToLogo"/>>
  <p matListItemLine><span>{{ source.name }}</span></p>
</mat-list-item>
```

**Reemplazar** el contenido del archivo `source-item.component.css` con el siguiente código, ubicado en la carpeta `/src/app/news/components/source-item`:

```css
.avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  margin-right: 10px;
}
```

### Modificación del SourceListComponent

**Agregar** los siguientes `import` al archivo `source-list.component.ts`, ubicado en la carpeta `/src/app/news/components/source-list`:

```ts
import { EventEmitter, Input, Output } from '@angular/core';
import { Source } from '../../model/source.entity';
import { MatNavList } from '@angular/material/list';
import { SourceItemComponent } from '../source-item/source-item.component';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `SourceListComponent`, ubicado en el archivo `source-list.component.ts`:

```ts
MatNavList, SourceItemComponent
```

**Reemplazar** el contenido de la clase `SourceListComponent` con el siguiente código, ubicado en el archivo `source-list.component.ts`:

```ts
@Input() sources!: Array<Source>;
@Output() sourceSelected = new EventEmitter<Source>();

onSourceSelected(source: Source) {
  this.sourceSelected.emit(source);
}
```

**Reemplazar** el contenido del archivo `source-list.component.html` con el siguiente código, ubicado en la carpeta `/src/app/news/components/source-list`:

```html
<mat-nav-list>
  @for (source of sources; track source.name) {
    <app-source-item (sourceSelected)="onSourceSelected($event);"
                     [source]="source"/>
  }
</mat-nav-list>
```


### Modificación del ArticleItemComponent

**Agregar** los siguientes `import` al archivo `article-item.component.ts`, ubicado en la carpeta `/src/app/news/components/article-item`:

```ts
import { Input } from '@angular/core';
import { Article } from '../../model/article.entity';
import { MatCardModule } from '@angular/material/card';
import { MatAnchor, MatIconButton } from '@angular/material/button';
import { TranslatePipe } from '@ngx-translate/core';
import { MatIcon } from '@angular/material/icon';
import { MatSnackBar, MatSnackBarModule } from '@angular/material/snack-bar';
import { DatePipe } from '@angular/common';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `ArticleItemComponent`, ubicado en el archivo `article-item.component.ts`:

```ts
MatCardModule, MatAnchor, MatIconButton, MatIcon, 
TranslatePipe, MatSnackBarModule, DatePipe
```

**Reemplazar** el contenido de la clase `ArticleItemComponent` con el siguiente código, ubicado en el archivo `article-item.component.ts`:

```ts
@Input() article!: Article;

constructor(private snackBar: MatSnackBar) {}

async shareArticle() {
  const articleShareInfo = {
    title: this.article.title,
    url: this.article.url
  };

  if (navigator.share) {
    try {
      await navigator.share(articleShareInfo);
      this.snackBar.open('Article shared successfully!', 'Close', { duration: 3000 });
    } catch (error) {
      this.snackBar.open('Sharing failed.', 'Close', { duration: 3000 });
    }
  } else {
    try {
      await navigator.clipboard.writeText(articleShareInfo.url);
      this.snackBar.open('Article URL copied to clipboard!', 'Close', { duration: 3000 });
    } catch (error) {
      this.snackBar.open('Failed to copy URL.', 'Close', { duration: 3000 });
    }
  }
}
```

**Reemplazar** el contenido del archivo `article-item.component.html` con el siguiente código, ubicado en la carpeta `/src/app/news/components/article-item`:

```html
<mat-card class="article-card">
  <img mat-card-image [alt]="article.title" [src]="article.urlToImage"/>
  <mat-card-header>
    <mat-card-title-group>
      <mat-card-title>{{ article.title }}</mat-card-title>
      <mat-card-subtitle> {{ article.publishedAt | date:'medium'  }} </mat-card-subtitle>
    </mat-card-title-group>
  </mat-card-header>
  <mat-card-content>
    <p> {{ article.description }}</p>
  </mat-card-content>
  <mat-card-actions class="actions-section">
    <a [href]="article.url" class="action-button" color="primary"
       mat-button
       target="_blank">{{ 'article.read-more' | translate }}</a>
    <span class="default-spacer"></span>
    <a [href]="article.source.url" class="action-button"
       mat-button
       target="_blank">{{ article.source.name }}</a>
    <button aria-label="Comments" color="white" mat-icon-button>
      <mat-icon>comment</mat-icon>
    </button>
    <button (click)="shareArticle()" aria-label="Share Article" color="white" mat-icon-button>
      <mat-icon>share</mat-icon>
    </button>
  </mat-card-actions>
</mat-card>
```

**Reemplazar** el contenido del archivo `article-item.component.css` con el siguiente código, ubicado en la carpeta `/src/app/news/components/article-item`:

```css
.article-card {
  margin: 4px;
}

.actions-section {
  padding-top: 10px;
  display: flex;
}

.action-button {
  font-size: large;
}

.default-spacer {
  flex: 1 1 auto;
}
```

### Modificación del ArticleListComponent

**Agregar** los siguientes `import` al archivo `article-list.component.ts`, ubicado en la carpeta `/src/app/news/components/article-list`:

```ts
import { Input } from '@angular/core';
import { Article } from '../../model/article.entity';
import { ArticleItemComponent } from '../article-item/article-item.component';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `ArticleListComponent`, ubicado en el archivo `article-list.component.ts`:

```ts
ArticleItemComponent
```

**Reemplazar** el contenido de la clase `ArticleListComponent` con el siguiente código, ubicado en el archivo `article-list.component.ts`:

```ts
@Input() articles: Array<Article> = [];
```

**Reemplazar** el contenido del archivo `article-list.component.html` con el siguiente código, ubicado en la carpeta `/src/app/news/components/article-list`:

```html
@for (article of articles; track article.title) {
  <app-article-item [article]="article"/>
}
```

### Modificación del SideNavigationBarComponent

**Agregar** los siguientes `import` al archivo `side-navigation-bar.component.ts`, ubicado en la carpeta `/src/app/news/components/side-navigation-bar`:

```ts
import { inject, OnInit } from '@angular/core';
import { Source } from '../../../news/model/source.entity';
import { Article } from '../../../news/model/article.entity';
import { NewsApiService } from '../../../news/services/news-api.service';
import { LogoApiService } from '../../../shared/services/logo-api.service';
import { MatSidenav, MatSidenavModule } from '@angular/material/sidenav';
import { MatToolbar } from '@angular/material/toolbar';
import { ArticleListComponent } from '../../../news/components/article-list/article-list.component';
import { FooterContentComponent } from '../footer-content/footer-content.component';
import { SourceListComponent } from '../../../news/components/source-list/source-list.component';
import { LanguageSwitcherComponent } from '../language-switcher/language-switcher.component';
import { MatIconButton } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `SideNavigationBarComponent`, ubicado en el archivo `side-navigation-bar.component.ts`:

```ts
MatSidenavModule, MatToolbar, ArticleListComponent, FooterContentComponent,
SourceListComponent, LanguageSwitcherComponent, MatIconButton,
MatSidenav, MatIconModule
```

**Reemplazar** el contenido de la clase `SideNavigationBarComponent` con el siguiente código, ubicado en el archivo `side-navigation-bar.component.ts`:

```ts
sources: Array<Source> = [];
articles: Array<Article> = [];

private newsApi = inject(NewsApiService);
private logoApi = inject(LogoApiService);
ngOnInit() {
  this.newsApi.getSources().subscribe(sources => {
    console.log(sources);
    this.sources = sources;
    this.sources.forEach(source => source.urlToLogo = this.logoApi.getUrlToLogo(source));
    console.log(this.sources);
    this.searchArticlesForSource(this.sources[0]);
  });
}

searchArticlesForSource(source: Source) {
  console.log(`selected source is: ${source.id}`);
  this.newsApi.getArticlesBySourceId(source.id)
    .subscribe( articles => {
      this.articles = articles;
      this.articles.forEach((article: { source: { urlToLogo: any; url: any; }; }) => {
        article.source.urlToLogo = source.urlToLogo;
        article.source.url = source.url;
      });
      console.log(this.articles);
    });
}

onSourceSelected(source: Source) {
  console.log(source.name);
  this.searchArticlesForSource(source);
}
```

**Reemplazar** el contenido del archivo `side-navigation-bar.component.html` con el siguiente código, ubicado en la carpeta `/src/app/news/components/side-navigation-bar`:

```html
<mat-sidenav-container class="sidenav-container">
  <mat-sidenav #drawer class="sidenav" fixedInViewport>
    <mat-toolbar>Sources</mat-toolbar>
    <app-source-list (sourceSelected)="onSourceSelected($event);drawer.toggle();" [sources]="sources"/>
  </mat-sidenav>
  <mat-sidenav-content>
    <mat-toolbar color="primary">
      <button (click)="drawer.toggle()"
              aria-label="Toggle sidenav"
              mat-icon-button
              type="button">
        <mat-icon aria-label="Sidenav toggle icon">menu</mat-icon>
      </button>
      <span>CatchUp</span>
      <span class="spacer"></span>
      <span>
        <app-language-switcher/>
      </span>
    </mat-toolbar>
    <app-article-list [articles]="articles"/>
    <app-footer-content/>
  </mat-sidenav-content>
</mat-sidenav-container>
```

**Reemplazar** el contenido del archivo `side-navigation-bar.component.css` con el siguiente código, ubicado en la carpeta `/src/app/news/components/side-navigation-bar`:

```css
.sidenav-container {
  height: 100%;
}

.sidenav {
  width: 300px;
}

.sidenav .mat-toolbar {
  background: inherit;
}

.mat-toolbar.mat-primary {
  position: sticky;
  top: 0;
  z-index: 1;
}

.spacer {
  flex: 1 1 auto;
}
```

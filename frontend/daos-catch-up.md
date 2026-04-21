# Catch Up Project v2610

## Creación del proyecto

> [!CAUTION]
> **En el caso de estar en un equipo MAC:**
> - Debe anteceder el comando `sudo` al ejecutar las instrucciones: `ng` y `chown`, y luego ingresar la contraseña del Administrador (`d3v3l0p3rUPC`).
> - Debe ubicarse en la carpeta `/Users/alumnos/IdeaProjects/1asi0729/2026-10` o en otra de su preferencia.

> [!CAUTION]
> **En el caso de estar en un equipo Windows:**
> - Debe ubicarse en la carpeta `IdeaProjects/` o en otra de su preferencia.

A continuación se detalla las intrucciones para crear un nuevo `workspace` e `initial starter app` de Angular. Más información en: https://angular.dev/tools/cli/setup-local

### Creación de un workspace y un initial application

**Cargar** el `Terminal` del sistema Operativo, ubicarse en la carpeta de su preferencia de acuerdo al Sistema Operativo y **ejecutar** el siguiente CLI command:

```bash
ng new daos-catch-up-v2610
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
cd daos-catch-up-v2610
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
sudo chown -R alumnos ./daos-catch-up-v2610
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
    "rights": "Copyright © 2026 {{ api }}, All rights reserved",
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
    "rights": "Derechos de autor © 2026 {{ api }}, Todos los derechos reservados",
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
newsProviderApiKey: 'fecf4feeffa64e4da682e7d268612ce5',
logoProviderApiBaseUrl: 'https://img.logo.dev/',
logoProviderPublishableKey: 'pk_H1j_uA9GRFizL2ikMw4qwQ'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
newsProviderApiBaseUrl: 'https://newsapi.org/v2',
newsProviderNewsEndpointPath: '/top-headlines',
newsProviderSourcesEndpointPath: '/sources',
newsProviderApiKey: 'fecf4feeffa64e4da682e7d268612ce5',
logoProviderApiBaseUrl: 'https://img.logo.dev/',
logoProviderPublishableKey: 'pk_H1j_uA9GRFizL2ikMw4qwQ'
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
ng generate service shared/infrastructure/logo-dev-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `logo-api.ts`, ubicado en la carpeta `/src/app/shared/infrastructure`:
```typescript
import { environment } from '../../../environments/environment';
import { Source } from '../../news/domain/model/source.entity';
```

**Reemplazar** el contenido de la clase `LogoApi` con el siguiente código, ubicado en el archivo `logo-api.ts`:

```ts
baseUrl = environment.logoProviderApiBaseUrl;
apiKey = environment.logoProviderPublishableKey;

constructor() { }

getUrlToLogo(source: Source) {
  //console.log('getUrlToLogo', source);
  return `${this.baseUrl}${new URL(source.url).hostname}?token=${this.apiKey}`;
}
```

### Creación de la clase SourceAssembler

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Source` tipo `Assembler`:

```bash
ng generate class news/infrastructure/source-assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `source-assembler.ts`, ubicado en la carpeta `/src/app/news/infrastructure`:
```typescript
import {SourceResource, SourcesResponse} from './sources-response';
import {LogoDevApi} from '../../shared/infrastructure/logo-dev-api';
import {Source} from '../domain/model/source.entity';
```

**Reemplazar** el contenido de la clase `SourceAssembler` con el siguiente código, ubicado en el archivo `source-assembler.ts`:

```ts
static logoApi: LogoDevApi;

static withLogoApi(logoApi: LogoDevApi) {
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
import {ArticleResource, TopHeadlinesResponse} from './top-headlines-response';
import {Article} from '../domain/model/article.entity';
import {LogoDevApi} from '../../shared/infrastructure/logo-dev-api';
```

**Reemplazar** el contenido de la clase `ArticleAssembler` con el siguiente código, ubicado en el archivo `article-assembler.ts`:

```ts
static logoApi: LogoDevApi;

static withLogoApi(logoApi: LogoDevApi) {
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
import {LogoDevApi} from '../../shared/infrastructure/logo-dev-api';
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
private logoApi = inject(LogoDevApi);

getSources(): Observable<Source[]> {
  return this.http.get<SourcesResponse>(`${this.baseUrl}${this.sourcesEndpoint}`, {
    params: { apiKey: this.apiKey }
  }).pipe(
    map(response => SourceAssembler.withLogoApi(this.logoApi).toEntitiesFromResponse(response))
  );
}

getArticlesBySourceId(sourceId: string): Observable<Article[]> {
  return this.http.get<TopHeadlinesResponse>(`${this.baseUrl}${this.newsEndpoint}`, {
    params: { apiKey: this.apiKey, sources: sourceId }
  }).pipe(
    map(response => ArticleAssembler.withLogoApi(this.logoApi).toEntitiesFromResponse(response))
  );
}
```

### Application layer : Creación del Service NewsStore

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `news-store`:

```bash
ng generate service news/application/news-store --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `news-store.ts`, ubicado en la carpeta `/src/app/news/application`:
```typescript
import { computed, inject, signal } from '@angular/core';
import { Source } from '../domain/model/source.entity';
import { Article } from '../domain/model/article.entity';
import { NewsApi } from '../infrastructure/news-api';
import {LogoDevApi} from '../../shared/infrastructure/logo-dev-api';
```

**Reemplazar** el contenido de la clase `NewsStore` con el siguiente código, ubicado en el archivo `news-store.ts`:

```ts
/** Internal signal containing all available sources. */
private sourcesSignal = signal<Source[]>([]);
/** Internal signal indexed by source id with preloaded article lists. */
private articlesSignal = signal<Record<string, Article[]>>({});
private newsApi = inject(NewsApi);
private logoApi = inject(LogoDevApi);

/** Read-only projection of available news sources. */
readonly sources = computed(() => this.sourcesSignal());
/** Read-only projection of the article cache keyed by source id. */
readonly articles = computed(() => this.articlesSignal());
/** Reactive list of articles for the currently selected source. */
public currentSourceArticles = computed(() => this.articlesSignal()[this.currentSource?.id] ?? []);
/** Currently selected source used as aggregate navigation focus. */
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
ng generate component shared/presentation/components/footer --skip-tests=true
```
```bash
ng generate component shared/presentation/components/language-switcher --skip-tests=true
```
```bash
ng generate component shared/presentation/components/layout --skip-tests=true
```

```bash
ng generate component news/presentation/components/article-item --skip-tests=true
```
```bash
ng generate component news/presentation/components/article-list --skip-tests=true
```
```bash
ng generate component news/presentation/components/source-item --skip-tests=true
```
```bash
ng generate component news/presentation/components/source-list --skip-tests=true
```


### Modificación del LanguageSwitcher Component

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

### Modificación del Footer Component

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
import {NewsStore} from '../../../../news/application/news-store';
import {Source} from '../../../../news/domain/model/source.entity';
import {MatSidenav, MatSidenavContainer, MatSidenavContent} from '@angular/material/sidenav';
import {MatToolbar} from '@angular/material/toolbar';
import {SourceList} from '../../../../news/presentation/components/source-list/source-list';
import {MatIcon} from '@angular/material/icon';
import {LanguageSwitcher} from '../language-switcher/language-switcher';
import {ArticleList} from '../../../../news/presentation/components/article-list/article-list';
import {Footer} from '../footer/footer';
import {MatIconButton} from '@angular/material/button';
import {Article} from '../../../../news/domain/model/article.entity';
```

**Agregar** la siguiente clase en el array `imports` del decorator `@Component` de la clase `Layout`:

```
MatSidenavContainer, MatToolbar, MatSidenav, SourceList, MatSidenavContent, MatIcon,
LanguageSwitcher, ArticleList, Footer, MatIconButton
```

**Agregar** la interface `OnInit` a la clase `Layout`: 

```
implements OnInit
```

**Reemplazar** el contenido de la clase `Layout` con el siguiente código:

```ts
protected store = inject(NewsStore);
protected readonly sources: Signal<Source[]> = this.store.sources;
protected readonly articles: Signal<Article[]> = this.store.currentSourceArticles;

ngOnInit(): void {
  this.store.loadSources();
  // Validar que funcione, en caso contrario retirar
  this.store.loadArticlesForCurrentSource();
}

updateArticlesBySource(source: Source): void {
  this.store.currentSource = source;
  this.store.loadArticlesForCurrentSource();
}
```

**Reemplazar** el contenido del archivo `layout.html` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/layout`:

```html
<mat-sidenav-container class="sidenav-container">
  <mat-sidenav #drawer class="sidenav" fixedInViewport>
    <mat-toolbar>Sources</mat-toolbar>
    <app-source-list (sourceSelected)="updateArticlesBySource($event);drawer.toggle();" [sources]="sources()"/>
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

    <app-article-list [articles]="articles()"/>
    <app-footer/>
  </mat-sidenav-content>
</mat-sidenav-container>
```

**Reemplazar** el contenido del archivo `layout.css` con el siguiente código, ubicado en la carpeta `/src/app/shared/presentation/components/layout`:

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

### Modificación del SourceItem Component

**Agregar** los siguientes `import` al archivo `source-item.ts`, ubicado en la carpeta `/src/app/news/presentation/components/source-item`:

```ts
import {input, InputSignal, output, OutputEmitterRef} from '@angular/core';
import {Source} from '../../../domain/model/source.entity';
import {MatListItem} from '@angular/material/list';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `SourceItem`:

```ts
MatListItem
```

**Reemplazar** el contenido de la clase `SourceItem` con el siguiente código:

```ts
source: InputSignal<Source>  = input.required<Source>();
sourceSelected: OutputEmitterRef<Source> = output<Source>();

emitSourceSelectedEvent() {
  this.sourceSelected.emit(this.source());
}
```

**Reemplazar** el contenido del archivo `source-item.html` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/source-item`:

```html
<mat-list-item (click)="emitSourceSelectedEvent()" style="margin: 10px; padding: 10px; cursor: pointer; align-items: center;">
  <img matListItemAvatar class="avatar" [alt]="source().name" [src]="source().urlToLogo"/>>
  <p matListItemLine><span>{{ source().name }}</span></p>
</mat-list-item>
```

**Reemplazar** el contenido del archivo `source-item.component.css` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/source-item`:

```css
.avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  margin-right: 10px;
}
```

### Modificación del SourceList Component

**Agregar** los siguientes `import` al archivo `source-list.ts`, ubicado en la carpeta `/src/app/news/presentation/components/source-list`:

```ts
import {input, output} from '@angular/core';
import {Source} from '../../../domain/model/source.entity';
import {MatNavList} from '@angular/material/list';
import {SourceItem} from '../source-item/source-item';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `SourceList`, ubicado en el archivo `source-list.ts`:

```ts
MatNavList, SourceItem
```

**Reemplazar** el contenido de la clase `SourceList` con el siguiente código, ubicado en el archivo `source-list.ts`:

```ts
sources = input<Source[]>();
sourceSelected = output<Source>();

emitSourceSelectedEvent(source: Source) {
  this.sourceSelected.emit(source);
}
```

**Reemplazar** el contenido del archivo `source-list.html` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/source-list`:

```html
<mat-nav-list>
  @for (source of sources(); track source.name) {
    <app-source-item (sourceSelected)="emitSourceSelectedEvent($event);"
                     [source]="source"/>
  }
</mat-nav-list>
```


### Modificación del ArticleItem Component

**Agregar** los siguientes `import` al archivo `article-item.ts`, ubicado en la carpeta `/src/app/news/presentation/components/article-item`:

```ts
import {inject, input, InputSignal} from '@angular/core';
import {Article} from '../../../domain/model/article.entity';
import {MatSnackBar} from '@angular/material/snack-bar';
import {MatCard, MatCardActions, MatCardContent, MatCardHeader } from '@angular/material/card';
import {MatCardImage, MatCardSubtitle, MatCardTitle, MatCardTitleGroup } from '@angular/material/card';
import {DatePipe} from '@angular/common';
import {MatButton, MatIconButton} from '@angular/material/button';
import {TranslatePipe} from '@ngx-translate/core';
import {MatIcon} from '@angular/material/icon';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `ArticleItem`:

```ts
MatCard, MatCardHeader, MatCardTitleGroup, MatCardTitle, MatCardSubtitle, 
DatePipe, MatCardContent, MatCardActions, MatButton, MatCardImage, 
TranslatePipe, MatIconButton, MatIcon
```

**Reemplazar** el contenido de la clase `ArticleItem` con el siguiente código:

```ts
private snackBar = inject(MatSnackBar);
article: InputSignal<Article> = input.required<Article>();

async shareArticle() {
  const articleShareInfo = {
    title: this.article()?.title,
    url: this.article()?.url
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
      if (articleShareInfo.url) {
        await navigator.clipboard.writeText(articleShareInfo.url);
        this.snackBar.open('Article URL copied to clipboard!', 'Close', { duration: 3000 });
      }
    } catch (error) {
      this.snackBar.open('Failed to copy URL.', 'Close', { duration: 3000 });
    }
  }
}
```

**Reemplazar** el contenido del archivo `article-item.html` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/article-item`:

```html
<mat-card class="article-card">
  <img mat-card-image [alt]="article().title" [src]="article().urlToImage"/>
  <mat-card-header>
    <mat-card-title-group>
      <mat-card-title>{{ article().title }}</mat-card-title>
      <mat-card-subtitle> {{ article().publishedAt | date:'medium'  }} </mat-card-subtitle>
    </mat-card-title-group>
  </mat-card-header>
  <mat-card-content>
    <p> {{ article().description }}</p>
  </mat-card-content>
  <mat-card-actions class="actions-section">
    <a [href]="article().url" class="action-button" color="primary"
       mat-button
       target="_blank">{{ 'article.read-more' | translate }}</a>
    <span class="default-spacer"></span>
    <a [href]="article().source.url" class="action-button"
       mat-button
       target="_blank">{{ article().source.name }}</a>
    <button aria-label="Comments" color="white" mat-icon-button>
      <mat-icon>comment</mat-icon>
    </button>
    <button (click)="shareArticle()" aria-label="Share Article" color="white" mat-icon-button>
      <mat-icon>share</mat-icon>
    </button>
  </mat-card-actions>
</mat-card>
```

**Reemplazar** el contenido del archivo `article-item.css` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/article-item`:

```css
.article-card {
  margin: 50px;
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

### Modificación del ArticleList Component

**Agregar** los siguientes `import` al archivo `article-list.ts`, ubicado en la carpeta `/src/app/news/presentation/components/article-list`:

```ts
import {input, InputSignal} from '@angular/core';
import {Article} from '../../../domain/model/article.entity';
import {ArticleItem} from '../article-item/article-item';
```

**Agregar** las siguientes clases en el array `imports` del decorator `@Component` de la clase `ArticleList`, ubicado en el archivo `article-list.ts`:

```ts
ArticleItem
```

**Reemplazar** el contenido de la clase `ArticleList` con el siguiente código, ubicado en el archivo `article-list.component.ts`:

```ts
articles: InputSignal<Article[]> = input.required<Array<Article>>();
```

**Reemplazar** el contenido del archivo `article-list.html` con el siguiente código, ubicado en la carpeta `/src/app/news/presentation/components/article-list`:

```html
@for (article of articles(); track article.title) {
  <app-article-item [article]="article"/>
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


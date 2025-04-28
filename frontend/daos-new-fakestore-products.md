# Proyecto Fake-store
 
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
ng new daos-new-fakestore-products
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
cd daos-new-fakestore-products
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
sudo chown -R alumnos ./daos-new-fakestore-products
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
- 📂 daos-new-fakestore-products
  - 📁 server
```

**Crear** los archivo `db.json` y `routes.json` en la carpeta `server` con el siguiente contenido:

#### db.json
```json
{
  "products": [
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
    },
    {
      "id": 3,
      "title": "Mens Cotton Jacket",
      "price": 55.99,
      "description": "great outerwear jackets for Spring/Autumn/Winter, suitable for many occasions, such as working, hiking, camping, mountain/rock climbing, cycling, traveling or other outdoors. Good gift choice for you or your family member. A warm hearted love to Father, husband or son in this thanksgiving or Christmas Day.",
      "category": "men's clothing",
      "image": "https://fakestoreapi.com/img/71li-ujtlUL._AC_UX679_.jpg",
      "rating": {
        "rate": 4.7,
        "count": 500
      }
    },
    {
      "id": 4,
      "title": "Mens Casual Slim Fit",
      "price": 15.99,
      "description": "The color could be slightly different between on the screen and in practice. / Please note that body builds vary by person, therefore, detailed size information should be reviewed below on the product description.",
      "category": "men's clothing",
      "image": "https://fakestoreapi.com/img/71YXzeOuslL._AC_UY879_.jpg",
      "rating": {
        "rate": 2.1,
        "count": 430
      }
    },
    {
      "id": 5,
      "title": "John Hardy Women's Legends Naga Gold & Silver Dragon Station Chain Bracelet",
      "price": 695,
      "description": "From our Legends Collection, the Naga was inspired by the mythical water dragon that protects the ocean's pearl. Wear facing inward to be bestowed with love and abundance, or outward for protection.",
      "category": "jewelery",
      "image": "https://fakestoreapi.com/img/71pWzhdJNwL._AC_UL640_QL65_ML3_.jpg",
      "rating": {
        "rate": 4.6,
        "count": 400
      }
    },
    {
      "id": 6,
      "title": "Solid Gold Petite Micropave ",
      "price": 168,
      "description": "Satisfaction Guaranteed. Return or exchange any order within 30 days.Designed and sold by Hafeez Center in the United States. Satisfaction Guaranteed. Return or exchange any order within 30 days.",
      "category": "jewelery",
      "image": "https://fakestoreapi.com/img/61sbMiUnoGL._AC_UL640_QL65_ML3_.jpg",
      "rating": {
        "rate": 3.9,
        "count": 70
      }
    },
    {
      "id": 7,
      "title": "White Gold Plated Princess",
      "price": 9.99,
      "description": "Classic Created Wedding Engagement Solitaire Diamond Promise Ring for Her. Gifts to spoil your love more for Engagement, Wedding, Anniversary, Valentine's Day...",
      "category": "jewelery",
      "image": "https://fakestoreapi.com/img/71YAIFU48IL._AC_UL640_QL65_ML3_.jpg",
      "rating": {
        "rate": 3,
        "count": 400
      }
    },
    {
      "id": 8,
      "title": "Pierced Owl Rose Gold Plated Stainless Steel Double",
      "price": 10.99,
      "description": "Rose Gold Plated Double Flared Tunnel Plug Earrings. Made of 316L Stainless Steel",
      "category": "jewelery",
      "image": "https://fakestoreapi.com/img/51UDEzMJVpL._AC_UL640_QL65_ML3_.jpg",
      "rating": {
        "rate": 1.9,
        "count": 100
      }
    },
    {
      "id": 9,
      "title": "WD 2TB Elements Portable External Hard Drive - USB 3.0 ",
      "price": 64,
      "description": "USB 3.0 and USB 2.0 Compatibility Fast data transfers Improve PC Performance High Capacity; Compatibility Formatted NTFS for Windows 10, Windows 8.1, Windows 7; Reformatting may be required for other operating systems; Compatibility may vary depending on user’s hardware configuration and operating system",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/61IBBVJvSDL._AC_SY879_.jpg",
      "rating": {
        "rate": 3.3,
        "count": 203
      }
    },
    {
      "id": 10,
      "title": "SanDisk SSD PLUS 1TB Internal SSD - SATA III 6 Gb/s",
      "price": 109,
      "description": "Easy upgrade for faster boot up, shutdown, application load and response (As compared to 5400 RPM SATA 2.5” hard drive; Based on published specifications and internal benchmarking tests using PCMark vantage scores) Boosts burst write performance, making it ideal for typical PC workloads The perfect balance of performance and reliability Read/write speeds of up to 535MB/s/450MB/s (Based on internal testing; Performance may vary depending upon drive capacity, host device, OS and application.)",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/61U7T1koQqL._AC_SX679_.jpg",
      "rating": {
        "rate": 2.9,
        "count": 470
      }
    },
    {
      "id": 11,
      "title": "Silicon Power 256GB SSD 3D NAND A55 SLC Cache Performance Boost SATA III 2.5",
      "price": 109,
      "description": "3D NAND flash are applied to deliver high transfer speeds Remarkable transfer speeds that enable faster bootup and improved overall system performance. The advanced SLC Cache Technology allows performance boost and longer lifespan 7mm slim design suitable for Ultrabooks and Ultra-slim notebooks. Supports TRIM command, Garbage Collection technology, RAID, and ECC (Error Checking & Correction) to provide the optimized performance and enhanced reliability.",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/71kWymZ+c+L._AC_SX679_.jpg",
      "rating": {
        "rate": 4.8,
        "count": 319
      }
    },
    {
      "id": 12,
      "title": "WD 4TB Gaming Drive Works with Playstation 4 Portable External Hard Drive",
      "price": 114,
      "description": "Expand your PS4 gaming experience, Play anywhere Fast and easy, setup Sleek design with high capacity, 3-year manufacturer's limited warranty",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/61mtL65D4cL._AC_SX679_.jpg",
      "rating": {
        "rate": 4.8,
        "count": 400
      }
    },
    {
      "id": 13,
      "title": "Acer SB220Q bi 21.5 inches Full HD (1920 x 1080) IPS Ultra-Thin",
      "price": 599,
      "description": "21. 5 inches Full HD (1920 x 1080) widescreen IPS display And Radeon free Sync technology. No compatibility for VESA Mount Refresh Rate: 75Hz - Using HDMI port Zero-frame design | ultra-thin | 4ms response time | IPS panel Aspect ratio - 16: 9. Color Supported - 16. 7 million colors. Brightness - 250 nit Tilt angle -5 degree to 15 degree. Horizontal viewing angle-178 degree. Vertical viewing angle-178 degree 75 hertz",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/81QpkIctqPL._AC_SX679_.jpg",
      "rating": {
        "rate": 2.9,
        "count": 250
      }
    },
    {
      "id": 14,
      "title": "Samsung 49-Inch CHG90 144Hz Curved Gaming Monitor (LC49HG90DMNXZA) – Super Ultrawide Screen QLED ",
      "price": 999.99,
      "description": "49 INCH SUPER ULTRAWIDE 32:9 CURVED GAMING MONITOR with dual 27 inch screen side by side QUANTUM DOT (QLED) TECHNOLOGY, HDR support and factory calibration provides stunningly realistic and accurate color and contrast 144HZ HIGH REFRESH RATE and 1ms ultra fast response time work to eliminate motion blur, ghosting, and reduce input lag",
      "category": "electronics",
      "image": "https://fakestoreapi.com/img/81Zt42ioCgL._AC_SX679_.jpg",
      "rating": {
        "rate": 2.2,
        "count": 140
      }
    },
    {
      "id": 15,
      "title": "BIYLACLESEN Women's 3-in-1 Snowboard Jacket Winter Coats",
      "price": 56.99,
      "description": "Note:The Jackets is US standard size, Please choose size as your usual wear Material: 100% Polyester; Detachable Liner Fabric: Warm Fleece. Detachable Functional Liner: Skin Friendly, Lightweigt and Warm.Stand Collar Liner jacket, keep you warm in cold weather. Zippered Pockets: 2 Zippered Hand Pockets, 2 Zippered Pockets on Chest (enough to keep cards or keys)and 1 Hidden Pocket Inside.Zippered Hand Pockets and Hidden Pocket keep your things secure. Humanized Design: Adjustable and Detachable Hood and Adjustable cuff to prevent the wind and water,for a comfortable fit. 3 in 1 Detachable Design provide more convenience, you can separate the coat and inner as needed, or wear it together. It is suitable for different season and help you adapt to different climates",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/51Y5NI-I5jL._AC_UX679_.jpg",
      "rating": {
        "rate": 2.6,
        "count": 235
      }
    },
    {
      "id": 16,
      "title": "Lock and Love Women's Removable Hooded Faux Leather Moto Biker Jacket",
      "price": 29.95,
      "description": "100% POLYURETHANE(shell) 100% POLYESTER(lining) 75% POLYESTER 25% COTTON (SWEATER), Faux leather material for style and comfort / 2 pockets of front, 2-For-One Hooded denim style faux leather jacket, Button detail on waist / Detail stitching at sides, HAND WASH ONLY / DO NOT BLEACH / LINE DRY / DO NOT IRON",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/81XH0e8fefL._AC_UY879_.jpg",
      "rating": {
        "rate": 2.9,
        "count": 340
      }
    },
    {
      "id": 17,
      "title": "Rain Jacket Women Windbreaker Striped Climbing Raincoats",
      "price": 39.99,
      "description": "Lightweight perfet for trip or casual wear---Long sleeve with hooded, adjustable drawstring waist design. Button and zipper front closure raincoat, fully stripes Lined and The Raincoat has 2 side pockets are a good size to hold all kinds of things, it covers the hips, and the hood is generous but doesn't overdo it.Attached Cotton Lined Hood with Adjustable Drawstrings give it a real styled look.",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/71HblAHs5xL._AC_UY879_-2.jpg",
      "rating": {
        "rate": 3.8,
        "count": 679
      }
    },
    {
      "id": 18,
      "title": "MBJ Women's Solid Short Sleeve Boat Neck V ",
      "price": 9.85,
      "description": "95% RAYON 5% SPANDEX, Made in USA or Imported, Do Not Bleach, Lightweight fabric with great stretch for comfort, Ribbed on sleeves and neckline / Double stitching on bottom hem",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/71z3kpMAYsL._AC_UY879_.jpg",
      "rating": {
        "rate": 4.7,
        "count": 130
      }
    },
    {
      "id": 19,
      "title": "Opna Women's Short Sleeve Moisture",
      "price": 7.95,
      "description": "100% Polyester, Machine wash, 100% cationic polyester interlock, Machine Wash & Pre Shrunk for a Great Fit, Lightweight, roomy and highly breathable with moisture wicking fabric which helps to keep moisture away, Soft Lightweight Fabric with comfortable V-neck collar and a slimmer fit, delivers a sleek, more feminine silhouette and Added Comfort",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/51eg55uWmdL._AC_UX679_.jpg",
      "rating": {
        "rate": 4.5,
        "count": 146
      }
    },
    {
      "id": 20,
      "title": "DANVOUY Womens T Shirt Casual Cotton Short",
      "price": 12.99,
      "description": "95%Cotton,5%Spandex, Features: Casual, Short Sleeve, Letter Print,V-Neck,Fashion Tees, The fabric is soft and has some stretch., Occasion: Casual/Office/Beach/School/Home/Street. Season: Spring,Summer,Autumn,Winter.",
      "category": "women's clothing",
      "image": "https://fakestoreapi.com/img/61pHAEJ4NML._AC_UX679_.jpg",
      "rating": {
        "rate": 3.6,
        "count": 145
      }
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

**Cargar** el navegador e **ingrese** la siguiente Url: http://localhost:3000/api/v1/products

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
fakestoreProviderApiBaseUrl: 'http://localhost:3000/api/v1',
fakestoreProviderProductsEndpointPath: '/products'
```

**Agregar** los siguientes valores a la constante `environment` del archivo `environment.ts` ubicado en la carpeta `environments`:

```ts
production: true,
fakestoreProviderApiBaseUrl: 'https://fakestoreapi.com',
fakestoreProviderProductsEndpointPath: '/products'
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

**Crear** la siguiente estructura de carpetas en la carpeta :file_folder: `/src/app`:

```markdown
- 📂 src
  - 📁 app
    - 📁 products
      - 📁 components
      - 📁 model
      - 📁 services
    - 📁 public
      - 📁 components
    - 📂 shared
      - 📁 services
```

### Analizando Fake Store API

**Dirijase** al url: https://fakestoreapi.com/ y **revice** la documenación del endpoint `/products` ubicada en la url https://fakestoreapi.com/docs#tag/Products

### Analizando el endpoint products

Dirijase al endpoint: https://fakestoreapi.com/products y **evalue** el json de respuesta.

Ahora dirjase al endpoint local http://localhost:3000/api/v1/products y **evalue** el json de respuesta. Podrá verificar que dan el mismo son de respuesta.

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
ng generate interface products/services/product response
```

**Reemplazar** el contenido de la interface `Product` del archivo `product.response.ts` con el siguiente código, ubicado en la carpeta `/src/app/products/services`:

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
ng generate class products/model/rating --type=entity --skip-tests=true
```

**Agregar** los siguentes atributos y constructor a la clase `Rating` del archivo `rating.entity.ts` ubicado en la carpeta `/src/app/products/model`:

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
ng generate class products/model/product --type=entity --skip-tests=true
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

### Creación del class Product tipo Assembler (DTO Assembler)

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear la clase `Product` tipo `Assembler`:

```bash
ng generate class products/services/product --type=assembler --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `source.assembler.ts`, ubicado en la carpeta `/src/app/news/services`:
```typescript
import { ProductResponse } from './product.response';
import { Product } from '../model/product.entity';
```

**Reemplazar** el nombre de la clase `Source` del archivo `source.assembler.ts` por:

```ts
ProductAssembler
```

**Reemplazar** el contenido de la clase `SourceAssembler` con el siguiente código, ubicado en el archivo `source.assembler.ts`:

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

### Creación del Service FakestoreApiService

**Cargar** el `Terminal` del IDE y **ejecutar** el siguiente CLI command para crear el service `fakestore-api`:

```bash
ng generate service products/services/fakestore-api --skip-tests=true
```

**Agregar** los siguentes `import` al archivo `fakestore-api.service.ts`, ubicado en la carpeta `/src/app/products/services`:
```typescript
import { HttpClient } from '@angular/common/http';
import { map, Observable } from 'rxjs';
import { environment } from '../../../environments/environment';
import { Product } from '../model/product.entity';
import { ProductResponse } from './product.response';
import { ProductAssembler } from './product.assembler';
```

**Reemplazar** el contenido de la clase `FakestoreApiService` con el siguiente código, ubicado en el archivo `fakestore-api.service.ts`:

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


## Creación de componentes

**Cargar** el `Terminal` del IDE y agregue un nuevo `Tab`.

**Ejecute** los siguientes comandos para la creación de los componentes: `header-content, footer-content, language-switcher, product-list` (Ejecute los comandos uno a la vez):

```bash
ng generate component public/components/header-content --skip-tests=true
```
```bash
ng generate component public/components/footer-content --skip-tests=true
```
```bash
ng generate component public/components/language-switcher --skip-tests=true
```
```bash
ng generate component products/components/product-list --skip-tests=true
```

### Modificación del LanguageSwitcherComponent

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
  <p>{{ 'footer.rights' | translate: {api: 'Fake Store API'} }}</p>
  <p>
    {{ 'footer.intro' | translate }}
    {{ 'footer.author' | translate: {team: 'DAOS'} }}
  </p>
</footer>
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
constructor(private translate: TranslateService) {
  this.translate.addLangs(['en', 'es']);
  this.translate.setDefaultLang('en');
  this.translate.use('en');
}
```

**Reemplazar** el contenido del archivo `app.component.html` con:
```html
<app-header-content></app-header-content>
<app-product-list></app-product-list>
<app-footer-content></app-footer-content>
```


### Modificación del ProductListComponent

**Agregar** los siguientes imports a la clase `ProductListComponent` del archivo `product-list.component.ts` ubicado en la carpeta `/src/app/products/components/product-list`:
```ts
import { MatCardModule } from '@angular/material/card';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { MatInputModule } from '@angular/material/input';
import { MatFormFieldModule } from '@angular/material/form-field';
import { Product } from "../../model/product.entity";
import { FakestoreApiService } from '../../services/fakestore-api.service';
import { TranslateModule } from "@ngx-translate/core";
```

**Agregar** las siguientes clases en el array `imports` del `@Component` de la clase `ProductListComponent` del archivo `product-list.component.ts`:

```
MatFormFieldModule, MatInputModule, MatTableModule, MatCardModule, TranslateModule
```

**Reemplazar** el contenido de la clase `ProductListComponent` ubicado en el archivo `product-list.component.ts` con el siguiente código:

```ts
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

## Actividad

**Modificar** el `ProductListComponent` para incluir los atributos `rate` y `count` del modelo **Rating**.

# Guía Paso a Paso: Migración de `json-server` a MockAPI.io + Despliegue en Angular

**MockAPI.io** es una herramienta en la nube que permite simular APIs REST fácilmente sin necesidad de mantener un servidor en Render o Railway. Soporta persistencia de datos (operaciones `GET`, `POST`, `PUT`, `DELETE`) de forma gratuita.

---

## Parte 1: Crear la API REST en MockAPI.io

### Paso 1.1: Registro y Creación de Proyecto
1. Ingresa a [https://mockapi.io/](https://mockapi.io/) y crea una cuenta (puedes ingresar directamente con tu cuenta de GitHub).
2. En el panel principal, haz clic en el botón **`+ New Project`**.
3. Configura el proyecto:
   * **Project name:** `mis-cursos-api` (o el nombre de tu preferencia)
   * **API Prefix:** Puedes dejarlo por defecto (ej. `/api/v1`)
4. Haz clic en **Create**.

---

### Paso 1.2: Crear el Recurso `categories`

1. Entra al proyecto creado y haz clic en **`+ New Resource`**.
2. **Resource name:** `categories`
3. Define el esquema de campos en el editor visual o JSON:
   * `id`: (Generado automáticamente por MockAPI como string/number)
   * `name`: tipo `String` (puedes colocar Faker.js o dejarlo como cadena)
4. Haz clic en **Create**.
5. **Cargar los datos iniciales:**
   * En la fila del recurso `categories`, haz clic en el botón **Data**.
   * Reemplaza el contenido con tu array de categorías:
     ```json
     [
       {
         "id": "1",
         "name": "Java"
       },
       {
         "id": "2",
         "name": "Spring Boot"
       }
     ]
     ```
   * Haz clic en **Update**.

---

### Paso 1.3: Crear el Recurso `courses`

1. Haz clic de nuevo en **`+ New Resource`**.
2. **Resource name:** `courses`
3. Define los campos:
   * `id`: (automático)
   * `title`: tipo `String`
   * `description`: tipo `String`
   * `categoryId`: tipo `String` o `Number`
4. Haz clic en **Create**.
5. **Cargar los datos iniciales:**
   * Haz clic en **Data** dentro de `courses`.
   * Pega tus datos iniciales:
     ```json
     [
       {
         "id": "1",
         "title": "Java for Beginners",
         "description": "An introductory course to Java programming language.",
         "categoryId": "1"
       },
       {
         "id": "2",
         "title": "Advanced Java Concepts",
         "description": "A deep dive into advanced Java programming topics.",
         "categoryId": "1"
       }
     ]
     ```
   * Haz clic en **Update**.

> **URL Base generada:** Copia la URL que aparece en la parte superior del panel de MockAPI. Tendrá un formato similar a:  
> `https://65ab1234567890.mockapi.io/api/v1`

---

## Parte 2: Conectar Angular a MockAPI.io

### Paso 2.1: Actualizar Entornos (`environment.ts` / `environment.prod.ts`)

En tu proyecto de Angular, edita el archivo de entorno de producción:

**`src/environments/environment.prod.ts`**:
```typescript
export const environment = {
  production: true,
  apiUrl: 'https://65ab1234567890.mockapi.io/api/v1' // Tu URL real de MockAPI
};
```

---

### Paso 2.2: Actualizar los Servicios HTTP en Angular

Asegúrate de ajustar los tipos de identificadores si MockAPI los devuelve como `string`:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { environment } from '../environments/environment';

export interface Category {
  id?: string;
  name: string;
}

export interface Course {
  id?: string;
  title: string;
  description: string;
  categoryId: string;
}

@Injectable({
  providedIn: 'root'
})
export class CourseService {
  private apiUrl = environment.apiUrl;

  constructor(private http: HttpClient) {}

  getCourses(): Observable<Course[]> {
    return this.http.get<Course[]>(`${this.apiUrl}/courses`);
  }

  getCategories(): Observable<Category[]> {
    return this.http.get<Category[]>(`${this.apiUrl}/categories`);
  }

  createCourse(course: Course): Observable<Course> {
    return this.http.post<Course>(`${this.apiUrl}/courses`, course);
  }
}
```

---

## Parte 3: Desplegar el Frontend (Angular)

### Paso 3.1: Redirección para Angular Router (Evitar Error 404 al recargar)

* **Si usas Vercel:** Crea `vercel.json` en la raíz de tu proyecto Angular:
  ```json
  {
    "rewrites": [
      { "source": "/(.*)", "destination": "/index.html" }
    ]
  }
  ```

* **Si usas Netlify:** Crea `_redirects` en la carpeta `src/` de Angular:
  ```text
  /* /index.html 200
  ```
  E inclúyelo en el archivo `angular.json` en la propiedad `assets`:
  ```json
  "assets": [
    "src/favicon.ico",
    "src/assets",
    "src/_redirects"
  ]
  ```

---

### Paso 3.2: Despliegue en Vercel

1. Subes tu código fuente de Angular a un repositorio en **GitHub**.
2. Entra en [Vercel](https://vercel.com/) e inicia sesión.
3. Haz clic en **Add New > Project** e importa tu repositorio de Angular.
4. Framework Preset: **Angular**.
5. Presiona **Deploy**.

---

## ⚡ Diferencias principales entre `json-server` y MockAPI.io

| Característica | `json-server` (Local/Render) | MockAPI.io |
| :--- | :--- | :--- |
| **Relaciones/Filtros** | Soporta `courses?categoryId=1` y `courses?_embed=categories` | Admite filtrado simple por query params (`courses?categoryId=1`) |
| **Persistencia** | En Render se pierden datos al reiniciar el servidor | **Persistente** (los `POST`, `PUT`, `DELETE` se guardan en la nube) |
| **Velocidad / Cold Start** | En capa gratuita de Render tarda en despertar | **Respuesta inmediata** (sin inicio en frío) |
| **Tipo de ID** | Soporta números o strings | Devuelve `id` como `string` por defecto |
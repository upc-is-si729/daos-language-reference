# Migración de `json-server` a MockAPI.io

**MockAPI.io** es una herramienta en la nube que permite simular APIs REST fácilmente sin necesidad de mantener un servidor en Render o Railway. Soporta persistencia de datos (operaciones `GET`, `POST`, `PUT`, `DELETE`) de forma gratuita.

---

## Parte 1: Crear la API REST en MockAPI.io

### Paso 1.1: Registro y Creación de Proyecto

1. Ingresa a [https://mockapi.io/](https://mockapi.io/) y crea una cuenta conun correo electrónico (puedes ingresar directamente con tu cuenta de GitHub).
2. En el panel principal, haz clic en el botón **`+ New Project`**.
3. Configura el proyecto:
    * **Project name:** `learning` (o el nombre de tu preferencia)
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
          "description": "Build microservices architecture using Spring Boot.",
          "categoryId": 2
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


# Deployment Proyecto Learning Center


## Build del Proyecto

Cargar el `Terminal` del IDE y ejecutar la siguiente instrucción: 
```
ng build
```

## Creación de Hosting en Firebase

Ingresar a la página de Firebase, coloque la siguiente URL en el navegador:

```
https://firebase.google.com/
```

Haga click en `sign in` ubicada en la parte superior derecha de la pantalla. luego ingrese sus credenciales de Google para ingresar a Firebase.

Siga los siguientes pasos para crear un nuevo `Firebase projects`:
- Haga click en `Go to console` ubicada en la parte superior derecha de la pantalla.
- Haga click en `Create a new Firebase project` ubicada en la parte media de la pantalla.
- Ingrese el nombre del proyecto: `daos-codigo-learning-center` y haga click en `Continue`.
- Desmarque el check `Enable Gemini in Firebase` y haga click en `Continue`.
- Desmarque el check `Enable Google Analytics for this project` y haga click en `Create project`.
- Espere mientras se crea el proyecto.
- Le mostrará el mensaje `Your Firebase project is ready` y finalmente haga click `Continue`.

Siga los siguientes pasos para crear el Hosting:
- En el menu ubicado en la parte izquierda central de la pantalla, haga click en `Build` y luego haga click en `Hosting`.
- Haga click en `Get started` ubicada en la parte central de la pantalla.
- Ahi encontraras los siguientes pasos: Install Firebase CLI, Initialize your project y Deploy to Firebase Hosting.
- Siga los pasos en el orden que indica.

## Instalación y configuración de Firebase

Cargar el `Terminal` del IDE y ejecutar la siguiente instrucción: 
```
npm install -g firebase-tools
```

En el `Terminal` del IDE ejecute la siguiente instrucción: 
```
firebase login
```

A la pregunta `Allow Firebase to collect CLI and Emulator Suite usage and error reporting information? (Y/n)`, responder: `n`.

Cargará el Navegador Web con las siguientes indicaciones: 
- `Choose an account to continue to Firebase CLI`, seleccione la cuenta donde creo el proyecto.
- `Sign in to Firebase CLI`, haga click en `Continue`.
- `Firebase CLI wants to access your Google Account`, haga click en `Allow`.
- Le aparecerá el mensaje: `Firebase CLI Login Successful`.
- En el Terminal del IDE debe de aparecerle el siguiente mensaje: `Success! Logged in as ....`

En el `Terminal` del IDE ejecute la siguiente instrucción: 
```
firebase init
```

Caragará el Firebase y responda a las siguientes preguntas:
- `Are you ready to proceed? (Y/n)`, responder: `Y`
- `Which Firebase features do you want to set up for this directory? Press Space to select features, then Enter to confirm your choices.`, Seleccione: `Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys` con el `<space>` y presione `<enter>`.
- `Please select an option:`, seleccione `Use an existing project`.
- `Select a default Firebase project for this directory`: seleccione `daos-codigo-learning-center (daos-codigo-learning-center)`.
- `What do you want to use as your public directory? (public)`, ingrese la siguiente dirección: `dist/daos-ws53-learning-center/browser`, ingrese el valor correcto. 
- `Configure as a single-page app (rewrite all urls to /index.html)? (y/N)`, responda: `Y`.
- `Set up automatic builds and deploys with GitHub? (y/N)`, responda: `N`.
- `File dist/daos-ws53-learning-center/browser/index.html already exists. Overwrite? (y/N)`, responda: `n`.

## Deployment en Firebase

En el `Terminal` del IDE ejecute la siguiente instrucción: 
```
firebase deploy
```

La aplicación se desplego en:
```
https://daos-ws51-learning-center.web.app/
```

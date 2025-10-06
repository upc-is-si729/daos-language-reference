# Deployment Proyecto Learning Center

## JSON-Server

### My JSON Server

Fake Online REST server for teams:

```
https://my-json-server.typicode.com
```

### Ubicación del db.json
 
Colocar el archivo `db.json` en la raiz de tu proyecto y subir el repositorio del proyecto.

Para probar que el My-json-server funcione pruebe colocando la siguiente url:
```typescript
https://my-json-server.typicode.com/<your-username>/<your-repo>
```

### Ejemplo:

A continuación se tiene un ejemplo:

```
https://my-json-server.typicode.com/upc-is-si729/db-server
```

### Modificando el environments

Cargar el IntelliJ IDEA y abra el proyecto ubicado en la carpeta de su preferencia.

Modificar el valor de `serverBasePath` en la constante `environment` ubicado en el archivo `environment.development.ts` de la carpeta `environments`:
```typescript
  serverBasePath: ' https://my-json-server.typicode.com/<your-username>/<your-repo>'
```

Modificar el valor de `serverBasePath` en la constante `environment` ubicado en el archivo `environment.ts` de la carpeta `environments`:
```typescript
  serverBasePath: ' https://my-json-server.typicode.com/<your-username>/<your-repo>'
```



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

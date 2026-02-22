# TERM — Internal Messenger

Chat interno en tiempo real. Firebase backend. Sin npm. Sin build step.

```
┌──────────────────────────────────┐
│  TERM  // INTERNAL MESSENGER     │
│  ● EN LÍNEA · CIFRADO · FIREBASE │
└──────────────────────────────────┘
```

## Stack

- **Frontend:** HTML + CSS + Vanilla JS (IBM Plex Mono)
- **Backend:** Firebase (Auth + Firestore + Storage)
- **Deploy:** GitHub Pages o Firebase Hosting
- **Dependencias:** 0 (Firebase vía CDN)

## Características

**Auth real**

- Registro con email + contraseña
- Login persistente (sesión guardada)
- Nombre de usuario y avatar ASCII

**Chat en tiempo real**

- Mensajes instantáneos vía Firestore
- Múltiples canales: `#general` `#archivos` `#ops`
- Crear canales personalizados
- Ver quién está en línea (presence system)

**Archivos**

- Imágenes → preview en lightbox
- Cualquier tipo de archivo → descarga directa
- Almacenados en Firebase Storage
- Drag & drop · pegar desde portapapeles

**Comandos**

- `/help` `/nick` `/me` `/clear` `/shrug` `/flip` `/whoami`

## Estructura

```
term-messenger/
├── index.html   ← toda la app + Firebase CDN
├── style.css    ← estilos (monochrome)
├── favicon.svg  ← icono animado
└── README.md
```

## Deploy en GitHub Pages

1. Sube los archivos a un repo en GitHub
1. Settings → Pages → Source: `main` branch → `/root`
1. Tu app estará en `https://tuusuario.github.io/term-messenger`

## Reglas de Firestore (importante)

En Firebase Console → Firestore → Rules, pega esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /channels/{channel}/messages/{msg} {
      allow read, write: if request.auth != null;
    }
    match /users/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == uid;
    }
    match /presence/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == uid;
    }
  }
}
```

## Reglas de Storage

En Firebase Console → Storage → Rules:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /channels/{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## Límites plan gratuito (Spark)

|Recurso                 |Límite gratuito|
|------------------------|---------------|
|Auth usuarios           |ilimitados     |
|Firestore lecturas/día  |50,000         |
|Firestore escrituras/día|20,000         |
|Storage                 |5 GB           |
|Hosting                 |10 GB/mes      |

Para uso interno con pocos usuarios, el plan gratuito es más que suficiente.

## License

MITo
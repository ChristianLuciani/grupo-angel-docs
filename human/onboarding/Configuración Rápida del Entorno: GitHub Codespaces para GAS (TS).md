# 🛠️ Guía de Configuración Rápida del Entorno: GitHub Codespaces para GAS (TS)

Esta guía te ayudará a configurar tu entorno de desarrollo en la nube utilizando **GitHub Codespaces**, asegurando que cumples con las directrices del proyecto (TypeScript, `clasp`, Jest).

## 🎯 Objetivo
Configurar un entorno de desarrollo pre-configurado que contenga Node.js, TypeScript, y la herramienta `clasp`, y autenticarlo con tu cuenta de Google.

---

## I. Inicio del Codespace (Entorno en la Nube)

Un Codespace es un entorno de desarrollo completo que se ejecuta en la nube, basado en la configuración que definimos en el archivo `.devcontainer/devcontainer.json` del repositorio.

### Paso 1: Crear un Nuevo Codespace

1.  Ve al repositorio del proyecto en GitHub.
2.  Haz clic en el botón verde **`< > Code`**.
3.  Selecciona la pestaña **"Codespaces"**.
4.  Haz clic en **"Create codespace on `<tu-rama>`"** (usualmente `main` o una rama de desarrollo designada).

> ⏳ **Espera:** GitHub tardará unos minutos en construir tu Codespace. El archivo `.devcontainer/devcontainer.json` garantiza que herramientas como **Node.js, `clasp` y las extensiones de VS Code** se instalen automáticamente.

### Paso 2: Verificar el Entorno

Una vez que el Codespace se inicie (se abrirá VS Code directamente en tu navegador), abre una nueva terminal (Terminal > New Terminal) y verifica las instalaciones:

| Comando | Resultado Esperado |
| :--- | :--- |
| `node -v` | Muestra la versión LTS de Node.js (ej: `v20.x.x`) |
| `clasp --version` | Muestra la versión de la CLI de Apps Script (ej: `@google/clasp/2.x.x`) |
| `npm test` | Debería ejecutar las pruebas Jest y fallar/pasar según los tests del proyecto. |

---

## II. Autenticación de `clasp` (Acceso a Google)

Para que tu Codespace pueda subir (`push`) o bajar (`pull`) código del proyecto de Google Apps Script asociado, debes autenticar la herramienta `clasp` con tu cuenta de Google.

> ⚠️ **Importante:** Este paso **debe repetirse** si el Codespace es reconstruido o si la sesión de autenticación expira.

### Paso 3: Iniciar Sesión con `clasp`

1.  En la terminal de tu Codespace, ejecuta el comando de inicio de sesión:
    ```bash
    clasp login
    ```
2.  La terminal mostrará un mensaje pidiéndote que visites una URL de Google y te autentiques. Copia la URL que aparece:
    ```
    Please log in to Google. Visit this URL:
    [Copia esta URL]
    ```
3.  **Abre un navegador** (fuera del Codespace) y pega la URL.
4.  Selecciona tu cuenta de Google de la empresa y **otorga los permisos necesarios** a la CLI de Apps Script.
5.  Google te proporcionará un **código de verificación alfanumérico largo**. **Cópialo.**

### Paso 4: Finalizar la Autenticación

1.  Vuelve a la terminal del Codespace.
2.  Pega el código de verificación que acabas de copiar de Google y presiona **Enter**.

> ✅ **Éxito:** Recibirás un mensaje como: `Logged in to Google. Your credentials are saved to ~/.clasp-credentials.json`
> ¡Tu entorno está listo para interactuar con los servicios de Google!

---

## III. Flujo de Trabajo (Cumpliendo con las Directrices)

Una vez configurado, sigue siempre el **GitHub Flow** y las directrices de codificación:

| Tarea | Comando / Regla |
| :--- | :--- |
| **1. Iniciar Desarrollo** | Crea una rama de `feature` o `bugfix` **siempre** (`git checkout -b feature/nombre-tarea`). |
| **2. Codificar** | Desarrolla en `src/` siguiendo la **Separación de Responsabilidades** (`logic`, `api`, `server.ts`) y el **Modo Estricto de TS**. |
| **3. Probar** | Ejecuta las pruebas unitarias para tu lógica antes de enviar (`npm test`). |
| **4. Subir a GAS** | **Solo** sube el código a Apps Script una vez que las pruebas hayan pasado y estés listo para una revisión local (`clasp push`). |
| **5. Colaboración** | Haz `commit`, sube tu rama a GitHub, y abre un **Pull Request (PR)** para que el equipo revise tu código antes de que se haga merge a `main`. |

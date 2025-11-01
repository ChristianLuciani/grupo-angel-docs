# 📖 Guía de Configuración del Entorno: Wiki de Documentación (Hugo)

Esta guía establece los pasos para configurar tu entorno de desarrollo en la nube (Codespace) y comenzar a escribir o editar la documentación de nuestro proyecto, utilizando **Hugo** para la previsualización en tiempo real.

## 🎯 Configuración Inicial del Codespace

Nuestro Codespace está configurado para la documentación. Esto significa que ya tendrás **Hugo, las herramientas de Markdown y las extensiones de VS Code** instaladas automáticamente.

### Paso 1: Crear e Iniciar el Codespace

1.  Ve al repositorio de la documentación en GitHub.
2.  Haz clic en el botón verde **`< > Code`**.
3.  Selecciona la pestaña **"Codespaces"**.
4.  Haz clic en **"Create codespace on `<tu-rama>`"**.

> ⏳ **Espera:** GitHub construirá el entorno, usando el archivo `.devcontainer/devcontainer.json` para instalar Hugo y las extensiones necesarias.

### Paso 2: Verificar el Entorno

Una vez que VS Code se abra en tu navegador, abre una nueva terminal (**Terminal > New Terminal**) y verifica la instalación de Hugo:

| Comando | Función | Resultado Esperado |
| :--- | :--- | :--- |
| `hugo version` | Verifica la instalación de Hugo. | Muestra la versión de Hugo (ej: `hugo v0.120.4+...`). |

---

## 🏗️ Paso 3: Inicializar la Estructura de Hugo (Si es Necesario)

Si eres el primer desarrollador en configurar esto y el repositorio aún no tiene la estructura de Hugo, sigue estos pasos para crearla.

> **¡Importante!** Si ya ves carpetas como `content/`, `layouts/` y `themes/`, puedes omitir este paso.

1.  **Crea el esqueleto del sitio Hugo:**
    ```bash
    hugo new site . --force
    ```
2.  **Agrega el tema de documentación:** Por convención, usaremos un tema de documentación simple (ejemplo: Ananke):
    ```bash
    git submodule add [https://github.com/theNewDynamic/gohugo-theme-ananke.git](https://github.com/theNewDynamic/gohugo-theme-ananke.git) themes/ananke
    ```
3.  **Configura el sitio para usar el tema (Edita el archivo de configuración):**
    Abre el archivo **`config.toml`** en la raíz del proyecto y asegúrate de que tiene la siguiente línea, o una similar si usas otro formato (`.yaml` o `.json`):
    ```toml
    baseURL = "[http://example.org/](http://example.org/)"
    languageCode = "es-es"
    title = "Wiki de Documentación del Proyecto"
    theme = "ananke" # Asegúrate de que este nombre coincida con la carpeta del tema
    ```
4.  **Haz un commit inicial de la estructura de Hugo:**
    ```bash
    git add .
    git commit -m "feat: Add initial Hugo structure and Ananke theme"
    git push
    ```

---

## 💻 Paso 4: Escribir y Previsualizar la Documentación

### A. Creación de Contenido

Toda la documentación debe residir en la carpeta **`content/`**.

* Para crear una nueva página (ej. `guia-flujo.md`):
    ```bash
    hugo new content guia-flujo.md
    ```
* Edita los archivos Markdown dentro de la carpeta `content/`.

### B. Iniciar el Servidor de Previsualización

Para ver tu documentación tal como aparecerá publicada, inicia el servidor de desarrollo de Hugo:

1.  En la terminal del Codespace, ejecuta:
    ```bash
    hugo server -D 
    ```
    *La bandera `-D` incluye páginas marcadas como `draft` (borrador).*
2.  **Abre el Previsualizador:** Codespaces detectará automáticamente que el puerto `1313` está activo.
    * Verás un mensaje emergente que dice **"Port 1313 is available"**.
    * Haz clic en **"Open in Browser"** o **"Open in VS Code"** para ver tu wiki en vivo.

> 📝 **Flujo de Edición:** Mientras el servidor esté activo, cualquier cambio que guardes en tus archivos `.md` se actualizará **instantáneamente** en la ventana del navegador.

---

## 🚀 Flujo de Colaboración Final

Cuando hayas terminado de escribir o editar tu documentación:

1.  Detén el servidor de Hugo (presionando `Ctrl + C` en la terminal).
2.  Asegúrate de que tus archivos de documentación estén listos para revisión.
3.  Sigue el flujo estándar de Git:
    ```bash
    git add .
    git commit -m "docs: Finaliza la guía sobre el flujo de trabajo"
    git push
    ```
4.  Abre un **Pull Request (PR)** en GitHub para que tu cambio sea revisado antes de hacer *merge* a la rama principal.

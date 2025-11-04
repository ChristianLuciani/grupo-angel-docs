# Configurar CLASP en GitHub Codespaces

## Problema
`clasp login` no funciona directamente en Codespaces porque intenta redirigir a `localhost` después de la autenticación OAuth, lo cual falla en entornos remotos.

## Solución: Transferir Credenciales Manualmente

### Paso 1: Autenticar en tu Máquina Local

En tu computadora local (no en Codespaces):
```bash
clasp login
```

Completa el proceso de login en el navegador que se abre automáticamente.

### Paso 2: Copiar las Credenciales

En tu máquina local, obtén el contenido del archivo de credenciales:
```bash
cat ~/.clasprc.json
```

Copia todo el contenido JSON que aparece (debería verse algo como esto):
```json
{
  "token": {
    "access_token": "...",
    "refresh_token": "...",
    "scope": "...",
    "token_type": "Bearer",
    "expiry_date": ...
  },
  "oauth2ClientSettings": {
    "clientId": "...",
    "clientSecret": "...",
    "redirectUri": "..."
  },
  "isLocalCreds": false
}
```

### Paso 3: Crear el Archivo en Codespaces

En tu terminal de Codespaces:
```bash
# Crear el archivo de credenciales
nano ~/.clasprc.json
```

- Pega el contenido que copiaste
- Guarda con `Ctrl+O` y presiona `Enter`
- Sal con `Ctrl+X`

### Paso 4: Configurar Permisos
```bash
chmod 600 ~/.clasprc.json
```

### Paso 5: Verificar la Autenticación
```bash
clasp login --status
```

Deberías ver tu email autenticado correctamente.

## Automatizar con GitHub Secrets (Opcional pero Recomendado)

Para no repetir este proceso cada vez que se reconstruya Codespaces:

### 1. Crear un Secret en GitHub

1. Ve a tu repositorio → **Settings** → **Secrets and variables** → **Codespaces**
2. Click en **New repository secret**
3. Nombre: `CLASP_CREDENTIALS`
4. Valor: Pega el contenido completo de tu `~/.clasprc.json`
5. Click en **Add secret**

### 2. Configurar en devcontainer.json

Crea o edita `.devcontainer/devcontainer.json`:
```json
{
  "name": "Apps Script Development",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:18",
  "postCreateCommand": "npm install -g @google/clasp",
  "postStartCommand": "bash .devcontainer/setup-clasp.sh",
  "secrets": {
    "CLASP_CREDENTIALS": {
      "description": "CLASP authentication credentials"
    }
  }
}
```

### 3. Crear Script de Setup

Crea `.devcontainer/setup-clasp.sh`:
```bash
#!/bin/bash

if [ -n "$CLASP_CREDENTIALS" ]; then
    echo "$CLASP_CREDENTIALS" > ~/.clasprc.json
    chmod 600 ~/.clasprc.json
    echo "✅ CLASP credentials configured"
else
    echo "⚠️  CLASP_CREDENTIALS secret not found"
fi
```

Dale permisos de ejecución:
```bash
chmod +x .devcontainer/setup-clasp.sh
```

### 4. Rebuild Codespace

Presiona `Cmd/Ctrl + Shift + P` y busca: **Codespaces: Rebuild Container**

Ahora tus credenciales se configurarán automáticamente cada vez que inicies Codespaces.

## Trabajar con Múltiples Proyectos Apps Script

### Estructura Actual (Monorepo)
```
mi-repo/
├── proyecto-a/
│   ├── .clasp.json
│   └── appsscript.json
├── proyecto-b/
│   ├── .clasp.json
│   └── appsscript.json
└── proyecto-c/
    ├── .clasp.json
    └── appsscript.json
```

### Comandos para Cada Proyecto

Cuando trabajes con múltiples proyectos, debes cambiar al directorio correspondiente:
```bash
# Trabajar en proyecto-a
cd proyecto-a
clasp push
clasp deploy

# Trabajar en proyecto-b
cd ../proyecto-b
clasp push
clasp deploy
```

### Recomendación: Un Repositorio por App

Para Apps Script, generalmente es mejor **un repositorio por aplicación** porque:

✅ **Ventajas:**
- Cada app tiene su propio ciclo de desarrollo y versiones
- No hay riesgo de conflictos entre proyectos
- Más fácil configurar CI/CD específico para cada app
- Mejor control de permisos y colaboradores por proyecto
- Historial de commits más limpio y relevante

❌ **Monorepo solo si:**
- Las apps están fuertemente relacionadas (componentes de un mismo sistema)
- Compartes mucho código común entre ellas
- Necesitas coordinar deploys entre múltiples apps

### Migrar a Repositorios Separados

Si decides separar tus proyectos:
```bash
# Para cada proyecto
cd proyecto-a
git init
git add .
git commit -m "Initial commit"
gh repo create mi-proyecto-a --private --source=. --push
```

## Comandos Útiles de CLASP
```bash
# Ver estado de autenticación
clasp login --status

# Listar proyectos
clasp list

# Crear nuevo proyecto
clasp create --title "Mi Proyecto" --type standalone

# Push de cambios
clasp push

# Pull de cambios desde Apps Script
clasp pull

# Abrir proyecto en el navegador
clasp open

# Ver versiones
clasp versions

# Crear deployment
clasp deploy -d "v1.0.0"
```

## Troubleshooting

### Error: "User has not enabled the Apps Script API"
1. Ve a https://script.google.com/home/usersettings
2. Activa "Google Apps Script API"

### Error: "Could not find .clasp.json"
Asegúrate de estar en el directorio correcto del proyecto o crea uno nuevo con `clasp create`.

### Las credenciales expiraron
Repite el proceso desde el Paso 1 para obtener nuevas credenciales.

---

**Nota:** Nunca compartas tu archivo `.clasprc.json` públicamente ni lo subas al repositorio. Añádelo a `.gitignore`:
```bash
echo "~/.clasprc.json" >> .gitignore
```
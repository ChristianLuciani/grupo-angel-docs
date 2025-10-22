# ⚙️ Local Setup Guide

> Configuración completa del ambiente de desarrollo local

---

## 🎯 Objetivo

Al terminar esta guía tendrás:

✅ Repos clonados localmente  
✅ clasp configurado y autenticado  
✅ GitHub Desktop funcional  
✅ Workflow híbrido (Cloud & Local) listo para usar  
✅ Primera prueba exitosa  

**Tiempo estimado**: 30-45 minutos

---

## 📋 Pre-requisitos

Antes de empezar, verifica que tienes:

- [x] macOS con Terminal (NOTA PARA GUADIAN DE LA DOCUMENTACION CREAR ARCHIVO PARA LA INSTALACION DE LOS PREREQUISITOS PARA LOS DISTINDOS SYSTEMAS OPERATIVO, Y SECCIONES PARA LOS DISTINTOS SISTEMAS OPERATIVOS DE ESTA GUIA)
- [x] Cuenta Google Workspace (Grupo Angel)
- [x] Acceso a repos GitHub (grupo-angel-apps, grupo-angel-docs)
- [x] clasp instalado 
- [x] GitHub Desktop instalado 
- [x] VS Code instalado (recomendado)

---

## 🚀 Setup Paso a Paso

### **PASO 1: Verificar Instalaciones** (5 min)

#### 1.1 Verificar clasp
```bash
clasp --version
Resultado esperado: 2.4.x o superior
Si falla:
#bash
npm install -g @google/clasp
1.2 Verificar Git
#bash
git --version
Resultado esperado: 2.x.x o superior
1.3 Verificar Node
#bash
node --version
Resultado esperado: v18.x.x o superior

PASO 2: Autenticar clasp (5 min)
2.1 Login
#bash
clasp login
Qué pasa:

Se abre navegador
Elige cuenta de Grupo Angel
Acepta permisos
Mensaje de éxito en terminal

2.2 Verificar autenticación
#bash
clasp list
Resultado esperado: Lista de scripts de tu cuenta
💡 Por qué: clasp necesita OAuth para interactuar con Apps Script API
⚠️ Importante: Usa la cuenta de Grupo Angel, NO cuenta personal

PASO 3: Clonar Repositorios (10 min)
3.1 Navegar a carpeta de trabajo
#bash
cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel"
💡 Nota: Esta carpeta ya existe en tu setup
3.2 Verificar repos existentes
#bash
ls -la
Deberías ver:
grupo-angel-apps/
grupo-angel-docs/
✅ Ya tienes los repos clonados
3.3 Si necesitas clonarlos de nuevo:
Repo de código:
bashgit clone https://github.com/ChristianLuciani/grupo-angel-apps.git
Repo de docs:
bashgit clone https://github.com/ChristianLuciani/grupo-angel-docs.git

PASO 4: Configurar Git (5 min)
4.1 Configurar identidad global (si no está configurado)
#bash
git config --global user.name "Christian Luciani"
git config --global user.email "cluciani@gmail.com"
4.2 Verificar configuración
#bash
git config --list
Busca estas líneas:
user.name=Christian Luciani
user.email=cluciani@gmail.com
💡 Por qué: Git necesita saber quién hace los commits

PASO 5: Explorar Estructura (5 min)
5.1 Ver estructura de apps
#bash
cd grupo-angel-apps
tree -L 2
Resultado:
.
├── README.md
├── apps/
│   ├── CasinoETL/
│   ├── FileConverter/
│   ├── TicketProcessor/
│   └── xls2gsheet/
├── backups/
├── libraries/
│   ├── AngelStyle/
│   └── CLib/
└── scripts/
5.2 Ver estructura de docs
#bash
cd ../grupo-angel-docs
tree -L 2
Resultado:
.
├── README.md
├── ai-context/
│   ├── claude/
│   ├── gemini/
│   └── universal/
├── assets/
│   └── images/
├── human/
│   ├── architecture/
│   ├── guides/
│   ├── onboarding/
│   ├── reference/
│   └── workflows/
└── templates/

PASO 6: Conectar clasp con Scripts (10 min)
6.1 Listar tus scripts
#bash
clasp list
Busca estos IDs:

CLib: 1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij
AngelStyle: 19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S
TicketProcessor: 1KU1SHCh1_4MvaNbeInWsj6-RVknSKJmxXtfjIEhuTtfRbBPe3M4FB8tP
xls2gsheet: 1Y2Ad5IKgaG2_XeV1iqzet71zJrLJqk8GL74b1ZSH6YrQKPz83o1DCUj0

6.2 Clonar un script (ejemplo: CLib)
bash
cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps/libraries/CLib"

clasp clone 1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij
Qué pasa:

Descarga Code.gs, CasinoETL.gs, etc.
Crea .clasp.json (config local)
Ahora puedes editar local y subir cambios

💡 Por qué: Permite desarrollo local con VS Code
⚠️ Nota: Solo clona si la carpeta está vacía, si ya tiene archivos .gs no es necesario

PASO 7: Configurar GitHub Desktop (5 min)
7.1 Abrir GitHub Desktop
#bash
open -a "GitHub Desktop"
7.2 Agregar repos existentes

File → Add Local Repository
Navega a: /Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps
Click Add Repository
Repite para grupo-angel-docs

7.3 Verificar branches

grupo-angel-apps: Debería estar en main
grupo-angel-docs: Debería estar en main

💡 Por qué: GitHub Desktop simplifica Git para ADHD (visual, no comandos)
___

PASO 8: Workflow Híbrido (5 min)
Cuándo Usar Cada Herramienta
TareaHerramientaPor QuéPrototipar nueva funciónApps Script onlineTesting rápido, Logger.log inmediatoEditar código existenteVS Code localAutocompletado, búsqueda, Git integradoAgregar librería a appApps Script onlineUI más clara para libreríasDeploy a producciónApps Script onlineVersionado nativo, menos erroresEditar HTML/CSSVS Code localMejor syntax highlightingVer logs de ejecuciónApps Script onlineExecutions tab
Flujo Recomendado
mermaidgraph LR
    A[Prototipo online] --> B[Copiar a local]
    B --> C[Refinar en VS Code]
    C --> D[Push a GitHub]
    D --> E[Pull online]
    E --> F[Deploy]
Paso a paso:

Prototipa en Apps Script online
Cuando funciona: clasp pull (descarga a local)
Refina en VS Code
Commit con GitHub Desktop
Push a GitHub
Deploy desde Apps Script online

💡 Por qué híbrido: Apps Script online es mejor para testing, local es mejor para desarrollo

PASO 9: Primera Prueba (5 min)
9.1 Crear script de prueba
bashcd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps/scripts"

# Crear carpeta para test
mkdir test-setup
cd test-setup

# Crear nuevo script
clasp create --type standalone --title "Test Setup" --rootDir .
9.2 Editar Code.gs local
bashcat > Code.gs << 'SCRIPT'
function testSetup() {
  Logger.log('✅ Setup completo!');
  Logger.log('📅 Fecha: ' + new Date());
  Logger.log('👤 Usuario: ' + Session.getActiveUser().getEmail());
  
  return {
    success: true,
    message: 'Ambiente configurado correctamente'
  };
}
SCRIPT
9.3 Subir cambios
bashclasp push
9.4 Ejecutar online

Abre: https://script.google.com
Busca script "Test Setup"
Ejecuta función testSetup()
Revisa Logs (View → Logs)

Resultado esperado:
✅ Setup completo!
📅 Fecha: [fecha actual]
👤 Usuario: cluciani@gmail.com
✅ Si ves esto: SETUP COMPLETO

PASO 10: Cleanup del Test (2 min)
bash# Volver a carpeta principal

cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps/scripts"

# Eliminar carpeta de test
rm -rf test-setup
💡 Por qué: Era solo para verificar que todo funciona

✅ Checklist Final de Verificación
Marca cada uno al completarlo:
Instalaciones

 clasp --version funciona
 git --version funciona
 node --version funciona

Autenticación

 clasp login exitoso
 clasp list muestra tus scripts

Repositorios

 grupo-angel-apps clonado
 grupo-angel-docs clonado
 Estructura visible con tree

Git Config

 user.name configurado
 user.email configurado

GitHub Desktop

 Ambos repos agregados
 Branches visibles (main)

clasp Connection

 Probaste clasp pull en alguna librería
 .clasp.json creado en carpeta

Workflow Test

 Script de prueba creado
 clasp push funcionó
 Ejecución online exitosa
 Logs visibles


🎯 Próximos Pasos
Ahora que tienes setup completo:

Explora el código

bash   cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"
   code .

Lee documentación existente

libraries/CLib/CURRENT_STATE.md
libraries/AngelStyle/CURRENT_STATE.md
apps/TicketProcessor/CURRENT_STATE.md


Revisa estructura

📖 Project Structure Reference


Planea primera contribución

Mejora un README
Agrega comentarios a código
Documenta algo no documentado




🆘 Troubleshooting
Problema: clasp: command not found
Solución:
bashnpm install -g @google/clasp

Problema: clasp login no abre navegador
Solución:
bash# Login manual
clasp login --no-localhost
Luego copia URL que aparece y pégala en navegador

Problema: Permission denied al clonar repo
Solución:
bash# Verifica SSH key
ls -la ~/.ssh/

# Si no existe, crea una
ssh-keygen -t ed25519 -C "cluciani@gmail.com"

# Agrega a GitHub
cat ~/.ssh/id_ed25519.pub
Copia output y agrégalo en GitHub: Settings → SSH Keys

Problema: clasp push falla con error de permisos
Solución:

Ve a https://script.google.com/home/usersettings
Activa: "Google Apps Script API"
Intenta de nuevo


Problema: Git pide username/password constantemente
Solución:
bash# Cambiar a SSH en lugar de HTTPS
cd grupo-angel-apps
git remote set-url origin git@github.com:ChristianLuciani/grupo-angel-apps.git

cd ../grupo-angel-docs
git remote set-url origin git@github.com:ChristianLuciani/grupo-angel-docs.git

Problema: tree command not found
Solución:
bashbrew install tree
Si no tienes Homebrew:
bash/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

📚 Comandos Útiles de Referencia
clasp
bashclasp login              # Autenticar
clasp logout             # Cerrar sesión
clasp list               # Listar scripts
clasp create             # Crear nuevo script
clasp clone <scriptId>   # Clonar script existente
clasp pull               # Descargar cambios desde online
clasp push               # Subir cambios locales
clasp open               # Abrir script en navegador
clasp logs               # Ver logs de última ejecución
clasp version            # Crear nueva versión
Git (terminal)
bashgit status               # Ver cambios pendientes
git add .                # Preparar todos los cambios
git commit -m "mensaje"  # Crear commit
git push                 # Subir a GitHub
git pull                 # Descargar de GitHub
git branch               # Ver branches
git checkout -b nueva    # Crear branch nueva
Navegación
bashcd ruta/a/carpeta        # Cambiar directorio
pwd                      # Ver directorio actual
ls -la                   # Listar archivos (incluye ocultos)
tree -L 2                # Ver estructura de carpetas (2 niveles)
code .                   # Abrir VS Code en carpeta actual
open .                   # Abrir Finder en carpeta actual

💡 Tips para ADHD/Dislexia
✅ Buenas Prácticas

Usa GitHub Desktop en lugar de comandos Git

Visual, menos errores
Ves cambios antes de commit


Crea aliases para comandos frecuentes

bash   # Agregar a ~/.zshrc
   alias ga='cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel"'
   alias gaa='cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"'
   alias gad='cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-docs"'

Usa VS Code Tasks para comandos complejos

.vscode/tasks.json con tareas pre-configuradas
Ejecuta con Cmd+Shift+B


Mantén Terminal organizada

bash   clear   # Limpiar pantalla cuando se satura

Workflow visual

Usa GitHub Desktop para Git
Usa Apps Script online para deploy
Usa VS Code solo para editar




🔗 Links Relacionados

📖 Onboarding README
📖 Project Structure
📖 Creating New App Guide (próximamente)
🔗 clasp Documentation
🔗 Apps Script API


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

# 📂 Estructura del Proyecto

> Guía completa de organización de carpetas y archivos

---

## 🌳 Árbol Completo

### **Repo 1: grupo-angel-apps** (Código)
grupo-angel-apps/
├── README.md                          # Overview del proyecto
├── apps/                              # Aplicaciones desplegadas
│   ├── CasinoETL/                    # ETL de archivos casino
│   │   └── [código sin documentar aún]
│   ├── FileConverter/                # (legacy/experimental)
│   ├── TicketProcessor/              # OCR tickets con Gemini Vision
│   │   └── CURRENT_STATE.md          # Estado actual completo
│   └── xls2gsheet/                   # Conversor XLS→Sheets
│       └── CURRENT_STATE.md          # Estado actual
├── backups/                          # Respaldos manuales
├── libraries/                        # Librerías compartidas
│   ├── AngelStyle/                   # UI/UX (CSS + JS)
│   │   ├── CURRENT_STATE.md
│   │   └── README.md
│   └── CLib/                         # Lógica core (Auth, Services, ETL)
│       ├── CURRENT_STATE.md
│       └── README.md
├── scripts/                          # Scripts auxiliares
└── 🏗️ ARQUITECTURA Y METODOLOGÍA...  # Documento largo (referencia)

### **Repo 2: grupo-angel-docs** (Documentación)
grupo-angel-docs/
├── README.md                         # Landing page de docs
├── ai-context/                       # Contextos para AI
│   ├── claude/                       # Para Claude Sonnet (backup)
│   │   ├── troubleshooting-context.md (próximamente)
│   │   └── refactoring-context.md (próximamente)
│   ├── gemini/                       # Para Gemini Pro (primary)
│   │   ├── universal-context.md (próximamente)
│   │   ├── clib-development.md (próximamente)
│   │   ├── angelstyle-development.md (próximamente)
│   │   └── new-app-creation.md (próximamente)
│   └── universal/                    # Contexto compartido
├── assets/                           # Recursos
│   └── images/                       # Diagramas, screenshots
├── human/                            # Documentación para desarrolladores
│   ├── architecture/                 # Decisiones de arquitectura
│   ├── guides/                       # Guías paso a paso
│   │   └── local-setup.md           ✅ ESTE ARCHIVO
│   ├── onboarding/                   # Onboarding de nuevos devs
│   │   └── README.md                ✅ ESTE ARCHIVO
│   ├── reference/                    # Referencias técnicas
│   │   └── project-structure.md     ✅ ESTE ARCHIVO
│   └── workflows/                    # Procesos de trabajo
└── templates/                        # Plantillas reutilizables

---

## 📋 Convenciones de Nombres

### **Apps (carpetas en `apps/`)**

| Patrón | Ejemplo | Cuándo Usar |
|--------|---------|-------------|
| `PascalCase` | `TicketProcessor` | Siempre para apps |
| Sin espacios | ❌ `Ticket Processor` | Nunca |
| Descriptivo | ✅ `CasinoETL` | Refleja funcionalidad |

### **Librerías (carpetas en `libraries/`)**

| Patrón | Ejemplo | Cuándo Usar |
|--------|---------|-------------|
| `PascalCase` | `CLib` | Core libraries |
| Sin prefijo "Lib" | ✅ `AngelStyle` | Nombre directo |

### **Archivos de Código (.gs)**

| Tipo | Patrón | Ejemplo |
|------|--------|---------|
| Main | `Code.gs` | Siempre el archivo principal |
| Config | `Config.gs` | Configuración y constantes |
| Módulo | `ModuleName.gs` | `CasinoETL.gs`, `Auth.gs` |
| HTML | `PageName.html` | `Operator.html`, `Supervisor.html` |
| Estilos | `main.css.html` | Estilos embebidos |
| Scripts | `main.js.html` | JavaScript embebido |

### **Archivos de Documentación (.md)**

| Tipo | Nombre | Ubicación |
|------|--------|-----------|
| Estado actual | `CURRENT_STATE.md` | En cada app/library |
| Guía general | `README.md` | Raíz de carpeta |
| Changelog | `CHANGELOG.md` | Raíz de app/library |

---

## 🗂️ Dónde Va Cada Cosa

### ✅ **Código de Producción**
📂 `grupo-angel-apps/apps/NombreApp/`

**Estructura típica**:
apps/TicketProcessor/
├── Code.gs              # doGet(), rutas, lógica principal
├── Config.gs            # PROJECT_ID, DATASET_ID, constantes
├── Operator.html        # Interfaz operador móvil
├── Supervisor.html      # Interfaz supervisor desktop
├── AdminDashboard.html  # Panel admin
├── AccessDenied.html    # Página de error
├── CURRENT_STATE.md     # Documentación técnica
└── appsscript.json      # Manifest (OAuth, timeZone)

### ✅ **Librerías Compartidas**
📂 `grupo-angel-apps/libraries/NombreLib/`

**CLib** (Lógica):
libraries/CLib/
├── Code.gs              # Auth, Services core
├── CasinoETL.gs         # Módulo ETL
├── CURRENT_STATE.md     # Inventario de funciones
└── README.md            # Guía de uso

**AngelStyle** (UI/UX):
libraries/AngelStyle/
├── Code.gs              # Función main getStyles()
├── main.css.html        # Estilos globales
├── main.js.html         # Helpers JS (logout, impersonation)
├── casino.css.html      # Estilos específicos casino
├── CURRENT_STATE.md     # Componentes actuales
└── README.md            # Manual de estilo

### ✅ **Documentación para Humanos**
📂 `grupo-angel-docs/human/`

| Carpeta | Contenido | Ejemplo |
|---------|-----------|---------|
| `onboarding/` | Guías para nuevos devs | `README.md`, `getting-started.md` |
| `guides/` | Tutoriales paso a paso | `local-setup.md`, `creating-new-app.md` |
| `reference/` | Referencias técnicas | `project-structure.md`, `clib-api.md` |
| `architecture/` | Decisiones de diseño | `system-overview.md`, `data-flow.md` |
| `workflows/` | Procesos de trabajo | `git-workflow.md`, `deployment.md` |

### ✅ **Contextos para AI**
📂 `grupo-angel-docs/ai-context/`

**Gemini** (desarrollo diario):
ai-context/gemini/
├── universal-context.md          # Stack, IDs, convenciones
├── clib-development.md           # Cómo extender CLib
├── angelstyle-development.md     # Cómo crear componentes UI
└── new-app-creation.md           # Template + checklist app nueva

**Claude** (troubleshooting):
ai-context/claude/
├── troubleshooting-context.md    # Debugging avanzado
└── refactoring-context.md        # Migración segura

---

## 🎯 Casos de Uso

### **Caso 1: Crear Nueva App**

1. **Crear carpeta**: `apps/MiNuevaApp/`
2. **Archivos mínimos**:
   - `Code.gs` (doGet, lógica)
   - `Config.gs` (constantes)
   - `Index.html` (UI)
   - `appsscript.json` (manifest)
   - `CURRENT_STATE.md` (docs)
3. **Agregar librerías**: CLib + AngelStyle en manifest
4. **Documentar**: Crear `README.md` con overview

### **Caso 2: Extender CLib**

1. **Abrir**: `libraries/CLib/Code.gs`
2. **Agregar función** con JSDoc completo
3. **Testing**: Probar en Apps Script online
4. **Actualizar**: `CURRENT_STATE.md` con nueva función
5. **Deploy**: Nueva versión de librería
6. **Notificar**: Actualizar apps que la usan

### **Caso 3: Crear Componente UI (AngelStyle)**

1. **Abrir**: `libraries/AngelStyle/main.css.html`
2. **Agregar clase**: `.angel-nuevo-componente { ... }`
3. **Testing visual**: En todas las apps
4. **Documentar**: `CURRENT_STATE.md` con ejemplo
5. **Deploy**: Nueva versión de AngelStyle

### **Caso 4: Documentar Feature Nueva**

1. **Si es guía**: `human/guides/nombre-guia.md`
2. **Si es referencia**: `human/reference/nombre-ref.md`
3. **Formato**: Listas numeradas, emojis, ejemplos
4. **Links**: Conectar con docs relacionadas
5. **Commit**: `docs(guides): agregar guía de X`

---

## 📊 Matriz de Decisión: ¿Dónde Poner Código?

| Pregunta | Respuesta SÍ → | Respuesta NO → |
|----------|----------------|----------------|
| ¿Es lógica compartida? | `libraries/CLib/` | Siguiente pregunta |
| ¿Es estilo/UI compartido? | `libraries/AngelStyle/` | Siguiente pregunta |
| ¿Es app específica? | `apps/NombreApp/` | Siguiente pregunta |
| ¿Es experimento? | `scripts/` o branch `experiment/` | Revisar arquitectura |

---

## 🔍 Archivos Importantes por App

### **TicketProcessor** (Producción ✅)
- `Code.gs`: 600+ líneas, lógica OCR + aprobación
- `Operator.html`: SPA móvil con upload de fotos
- `Supervisor.html`: Dashboard de aprobación
- `CURRENT_STATE.md`: Estado v13.0.0 documentado

**Usa**: CLib completo + AngelStyle completo

### **xls2gsheet** (Legacy ⚠️)
- **NO usa librerías** (standalone)
- Tiene función `extraerDatosDeHTML()` duplicada
- **Candidato #1 para migración** a arquitectura modular

**Estado**: Funcional pero necesita refactor

### **CasinoETL** (En desarrollo 🚧)
- Módulo dentro de CLib (`CasinoETL.gs`)
- Funciones: `transformAxesGamingVoucher()`, `transformSielconTITO()`
- **Sin app propia** (solo librería por ahora)

---

## 🚫 Qué NO Hacer

❌ **NO crear apps sin `CURRENT_STATE.md`**  
❌ **NO duplicar funciones** (usar CLib)  
❌ **NO estilos inline** (usar AngelStyle)  
❌ **NO hardcodear credenciales** (usar PropertiesService)  
❌ **NO commits sin descripción** (usar conventional commits)  
❌ **NO deploy sin testing** (checklist obligatorio)  

---

## 🔗 Links Relacionados

- 📖 [Onboarding README](../onboarding/README.md)
- 📖 [Local Setup Guide](../guides/local-setup.md)
- 📖 CLib API Reference (próximamente)
- 📖 AngelStyle Components (próximamente)

---

**Última actualización**: Octubre 2025  
**Mantenido por**: Christian Luciani (@ChristianLuciani)

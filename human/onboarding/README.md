# 🏠 Bienvenido a Grupo Angel Apps

> Onboarding para desarrolladores del ecosistema de aplicaciones internas

---

## 🎯 ¿Qué es este proyecto?

- **Automatización y datos** para Grupo Angel (casinos, restaurantes, legal, real estate)
- **BigQuery como fuente única de verdad** → BI → Machine Learning
- **Apps modulares** con librerías compartidas (CLib + AngelStyle)

---

## ⚡ Quick Start (3 Pasos)

### 1️⃣ **Setup Local** (20 minutos)
📖 Sigue: [`guides/local-setup.md`](../guides/local-setup.md)

### 2️⃣ **Entiende la Estructura** (10 minutos)
📖 Lee: [`reference/project-structure.md`](../reference/project-structure.md)

### 3️⃣ **Explora una App Real** (15 minutos)
📂 Abre: `grupo-angel-apps/apps/TicketProcessor/CURRENT_STATE.md`

---

## 📚 Documentación Disponible

### 🟢 Onboarding (Estás aquí)
- **README.md** ← Empiezas aquí
- `getting-started.md` (próximamente)
- `first-contribution.md` (próximamente)

### 🔵 Guías Prácticas
- **[`guides/local-setup.md`](../guides/local-setup.md)** - Setup completo
- `guides/creating-new-app.md` (próximamente)
- `guides/testing-checklist.md` (próximamente)

### 🟡 Referencia Técnica
- **[`reference/project-structure.md`](../reference/project-structure.md)** - Estructura de carpetas
- `reference/clib-api.md` (próximamente)
- `reference/angelstyle-components.md` (próximamente)

### 🟣 Arquitectura
- `architecture/system-overview.md` (próximamente)
- `architecture/data-flow.md` (próximamente)

### 🔴 Workflows
- `workflows/git-workflow.md` (próximamente)
- `workflows/deployment.md` (próximamente)

---

## 🏗️ Stack Tecnológico
┌─────────────────────────────────────┐
│         Apps (TicketProcessor,      │
│         xls2gsheet, CasinoETL)      │
├─────────────────────────────────────┤
│   CLib v6.0.0    │ AngelStyle v4.0  │
│   (Lógica Core)  │ (UI/UX)          │
├─────────────────────────────────────┤
│  BigQuery │ Gemini AI │ Drive API   │
└─────────────────────────────────────┘

**Tecnologías**:
- **Platform**: Google Apps Script (JavaScript ES6+)
- **AI**: Gemini Pro (primary), Claude Sonnet (backup)
- **Data**: BigQuery, Google Sheets API v4
- **Dev Tools**: clasp, GitHub Desktop, VS Code

---

## 🎓 Tu Perfil de Onboarding

### ✅ **Pre-requisitos Instalados**
- [x] clasp (CLI de Apps Script)
- [x] GitHub Desktop
- [x] VS Code
- [x] Acceso a repos (grupo-angel-apps, grupo-angel-docs)

### 📋 **Checklist de Onboarding**

#### Día 1: Setup (HOY)
- [ ] Completar [`guides/local-setup.md`](../guides/local-setup.md)
- [ ] Clonar ambos repos localmente
- [ ] Probar `clasp login`
- [ ] Revisar [`reference/project-structure.md`](../reference/project-structure.md)

#### Día 2: Exploración
- [ ] Leer `libraries/CLib/CURRENT_STATE.md`
- [ ] Leer `libraries/AngelStyle/CURRENT_STATE.md`
- [ ] Abrir TicketProcessor en Apps Script online
- [ ] Ejecutar función de prueba en CLib

#### Día 3: Primera Contribución
- [ ] Crear branch: `docs/mi-primera-contribucion`
- [ ] Agregar algo al README principal
- [ ] Hacer commit con formato: `docs: mi primer commit`
- [ ] Push y crear PR

#### Semana 1: Profundización
- [ ] Estudiar código de xls2gsheet (app más simple)
- [ ] Entender flujo de autenticación (CLib)
- [ ] Revisar componentes de AngelStyle
- [ ] Hacer deploy de prueba en TEST

---

## 🤖 Trabajando con AI

### Gemini Pro (Primary)
**Cuándo usar**:
- Desarrollo nuevo
- Generación de código
- Refactoring
- Documentación

**Contextos disponibles**:
- `ai-context/gemini/universal-context.md` (próximamente)
- `ai-context/gemini/clib-development.md` (próximamente)

### Claude Sonnet (Backup)
**Cuándo usar**:
- Troubleshooting complejo
- Debugging difícil
- Revisión de arquitectura
- Cuando Gemini no entiende el contexto

**Contextos disponibles**:
- `ai-context/claude/troubleshooting-context.md` (próximamente)
- `ai-context/claude/refactoring-context.md` (próximamente)

---

## 🆘 ¿Necesitas Ayuda?

### 📖 Primero revisa:
1. Esta documentación (carpeta `human/`)
2. Archivos `CURRENT_STATE.md` en cada componente
3. README.md del repo principal

### 🐛 Si encuentras un bug:
**Repo código**: [grupo-angel-apps/issues](https://github.com/ChristianLuciani/grupo-angel-apps/issues)

### 📝 Si la documentación no es clara:
**Repo docs**: [grupo-angel-docs/issues](https://github.com/ChristianLuciani/grupo-angel-docs/issues)

### 💬 Comunicación:
- **Primary**: Google Chat (Grupo Angel workspace)
- **Async**: Issues en GitHub
- **AI**: Gemini Pro con contextos compartidos

---

## 🚀 Próximos Pasos

### Ahora mismo:
👉 Ve a [`guides/local-setup.md`](../guides/local-setup.md)

### Después:
👉 Revisa [`reference/project-structure.md`](../reference/project-structure.md)

### Luego:
👉 Explora `apps/TicketProcessor/CURRENT_STATE.md`

---

## 🎯 Objetivo de Esta Semana

Al final de tu primera semana deberías poder:

✅ Clonar y modificar repos localmente  
✅ Entender la arquitectura de 3 capas  
✅ Usar CLib y AngelStyle en una app  
✅ Hacer commits con formato correcto  
✅ Crear un PR simple de documentación  

---

**¡Bienvenido al equipo! 🎉**

---

**Última actualización**: Octubre 2025  
**Mantenido por**: Christian Luciani (@ChristianLuciani)

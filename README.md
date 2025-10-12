# 📦 Grupo Angel Apps

> Ecosistema de aplicaciones internas para automatización y gestión de datos

---

## 🧭 Índice de Contenido (TOC)

1.  🎯 [Objetivo](#objetivo)
2.  🚀 [Quick Start](#quick-start)
3.  📚 [Documentación](#documentación)
4.  🏗️ [Estructura del Proyecto](#estructura-del-proyecto)
5.  🛠️ [Stack Tecnológico](#stack-tecnológico)
6.  📦 [Apps en Producción](#apps-en-producción)
7.  🔧 [Librerías](#librerías)
8.  🚀 [Roadmap](#roadmap)
9.  🤝 [Contribuir](#contribuir)
10. 🛡️ [Auto Mantenimiento y Documentación Estratégica](#auto-mantenimiento-y-documentación-estratégica)
11. 📞 [Soporte](#soporte)
12. 📄 [Licencia](#licencia)

---

## 🎯 Objetivo

Crear un **Departamento de Datos, Automatización y AI (DAAI)** con BigQuery como fuente única de verdad, BI centralizado y preparación para Machine Learning.

---

## 🚀 Quick Start

### Nuevo Developer
👉 Comienza aquí: [Onboarding Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/onboarding/README.md)

### Ya configurado
👉 Crear nueva app: [Creating New App Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/creating-new-app.md)

---

## 📚 Documentación

Toda la documentación está en el repo **[grupo-angel-docs](https://github.com/ChristianLuciani/grupo-angel-docs)**.

### Para Humanos (Documentación de Proceso)
*   🎓 **Onboarding:** [Guía de Inicio](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/onboarding/README.md)
*   📖 **Guías:** [Tutoriales y How-To's](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/README.md)
*   📘 **Referencias:** [APIs y Estándares](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/README.md)
*   🏗️ **Arquitectura:** [Visión General del Sistema](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/architecture/README.md)
*   🔄 **Workflows:** [Procesos de Desarrollo](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/workflows/README.md)

### Para AI (Contextos de Agentes)
*   🤖 **Contextos Gemini** (próximamente)
*   🤖 **Contextos Claude** (próximamente)

---

## 🏗️ Estructura del Proyecto
grupo-angel-apps/
├── apps/ # Aplicaciones desplegadas
│ ├── TicketProcessor/ # OCR de tickets (PROD)
│ ├── xls2gsheet/ # Conversor XLS→Sheets (PROD)
│ └── CasinoETL/ # ETL archivos casino
├── libraries/ # Librerías compartidas
│ ├── CLib/ # Core logic (Auth, Services, ETL)
│ └── AngelStyle/ # UI/UX components
│ └── XUtil/ # Utilidades extendidas
├── scripts/ # Scripts auxiliares
└── backups/ # Respaldos manuales
code
---

## 🛠️ Stack Tecnológico

*   **Platform**: Google Apps Script (JavaScript ES6+)
*   **AI**: Gemini Pro (primary), Claude Sonnet (backup)
*   **Data**: BigQuery, Google Sheets API
*   **Dev Tools**: clasp, GitHub Desktop, VS Code
*   **Version Control**: Git + GitHub

---

## 📦 Apps en Producción

| App | Versión | Status | Usuarios |
| :--- | :--- | :--- | :--- |
| [TicketProcessor](./apps/TicketProcessor/) | v13.0.0 | ✅ PROD | Operadores + Supervisores |
| [xls2gsheet](./apps/xls2gsheet/) | v1.0.0 | ✅ PROD | Contabilidad |
| CasinoETL | v1.0.0 | 🚧 Beta | Interno |

---

## 🔧 Librerías

### CLib v6.0.0
**ID**: `1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij`

Core business logic compartida:
*   🔐 Authentication & Authorization
*   🛠️ Services (Logging, Drive, Gemini, BigQuery)
*   🔄 ETL Transforms (AXES, SIELCON)

[Ver documentación completa](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/clib-api.md)

### AngelStyle v4.0.0
**ID**: `19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S`

UI/UX components con AI-Modern Dark Theme:
*   🎨 Paleta corporativa Grupo Angel
*   🧩 Componentes reutilizables
*   📱 Responsive design
*   ✨ Glassmorphism effects

[Ver documentación completa](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/angelstyle-api.md)

### XUtil v0.1.0
**ID**: `1v456789012345678901234567890123456789012345678901234567890` (Placeholder)

Utilidades extendidas y funciones auxiliares:
*   ⚙️ Funciones de validación de datos
*   🔗 Utilidades de URL y JSON
*   ⏱️ Funciones de tiempo y fecha

[Ver documentación completa](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/xutil-api.md)

---

## 🚀 Roadmap

### Q4 2024 ✅
*   CLib v6.0.0 estable
*   AngelStyle v4.0.0 base
*   TicketProcessor en producción
*   Documentación completa

### Q1 2025 🚧
*   Migración xls2gsheet a arquitectura modular
*   Refactor AngelStyle → AI-Modern Dark Theme completo
*   InventoryManager (nueva app)
*   Contextos AI para Gemini/Claude

### Q2 2025 🔮
*   CasinoETL v2.0 con validaciones avanzadas
*   BigQuery ML models
*   Dashboard ejecutivo BI
*   API Gateway para integraciones

---

## 🤝 Contribuir

### Workflow
1.  Fork del repo (o crear branch)
2.  Seguir [Git Workflow](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/workflows/git-workflow.md)
3.  Crear Pull Request
4.  Code review
5.  Merge a main

### Estándares
*   📝 [Conventional Commits](https://www.conventionalcommits.org/)
*   🔢 [Semantic Versioning](https://semver.org/)
*   📖 JSDoc en funciones públicas
*   ✅ Testing antes de PR

---

## 🛡️ Auto Mantenimiento y Documentación Estratégica

La documentación de Grupo Angel Apps es gestionada por el **Guardián Estratégico (AI)** para asegurar su consistencia, Neuro Accesibilidad y escalabilidad.

### 🔄 Proceso de Actualización

Para mantener la documentación sincronizada con el código y el formato de calidad (Neuro Accesibilidad), sigue estos pasos:

1.  **Identifica el Cambio:** Determina qué archivo de documentación (`.md`) y qué snippet de código (`.gs`) se vieron afectados.
2.  **Ejecuta el Comando:** Usa el comando `/actualizar` o `/limpiar` en el chat.
3.  **Doble Verificación:** Pega el contenido COMPLETO del archivo `.md` y el snippet de código afectado (Ver Regla V.2).

El Guardián Estratégico generará el `Reporte de Cambios` y el `Contenido Final Completo` con formato optimizado.

---

## 📞 Soporte

### Problemas
*   🐛 [Reportar Bug](https://github.com/ChristianLuciani/grupo-angel-apps/issues/new)
*   📖 [Troubleshooting Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/troubleshooting.md)

### Contacto
*   **Developer**: Christian Luciani ([@ChristianLuciani](https://github.com/ChristianLuciani))
*   **Docs**: [grupo-angel-docs](https://github.com/ChristianLuciani/grupo-angel-docs)

---

## 📄 Licencia

Privado - Grupo Angel  
© 2024-2025

---

**Última actualización**: Octubre 2025
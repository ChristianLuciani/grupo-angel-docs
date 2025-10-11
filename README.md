# 📦 Grupo Angel Apps

> Ecosistema de aplicaciones internas para automatización y gestión de datos

---

## 🎯 Objetivo

Crear un **Departamento de Datos y Automatización** con BigQuery como fuente única de verdad, BI centralizado y preparación para Machine Learning.

---

## 🚀 Quick Start

### Nuevo Developer
👉 Comienza aquí: [Onboarding Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/onboarding/README.md)

### Ya configurado
👉 Crear nueva app: [Creating New App Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/creating-new-app.md)

---

## 📚 Documentación

Toda la documentación está en el repo **[grupo-angel-docs](https://github.com/ChristianLuciani/grupo-angel-docs)**

### Para Humanos
- 🎓 [Onboarding](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/human/onboarding)
- 📖 [Guías](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/human/guides)
- 📘 [Referencias](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/human/reference)
- 🏗️ [Arquitectura](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/human/architecture)
- 🔄 [Workflows](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/human/workflows)

### Para AI
- 🤖 [Contextos Gemini](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/ai-context/gemini) (próximamente)
- 🤖 [Contextos Claude](https://github.com/ChristianLuciani/grupo-angel-docs/tree/main/ai-context/claude) (próximamente)

---

## 🏗️ Estructura del Proyecto
grupo-angel-apps/
├── apps/                    # Aplicaciones desplegadas
│   ├── TicketProcessor/    # OCR de tickets (PROD)
│   ├── xls2gsheet/         # Conversor XLS→Sheets (PROD)
│   └── CasinoETL/          # ETL archivos casino
├── libraries/              # Librerías compartidas
│   ├── CLib/              # Core logic (Auth, Services, ETL)
│   └── AngelStyle/        # UI/UX components
├── scripts/               # Scripts auxiliares
└── backups/              # Respaldos manuales

---

## 🛠️ Stack Tecnológico

- **Platform**: Google Apps Script (JavaScript ES6+)
- **AI**: Gemini Pro (primary), Claude Sonnet (backup)
- **Data**: BigQuery, Google Sheets API
- **Dev Tools**: clasp, GitHub Desktop, VS Code
- **Version Control**: Git + GitHub

---

## 📦 Apps en Producción

| App | Versión | Status | Usuarios |
|-----|---------|--------|----------|
| [TicketProcessor](./apps/TicketProcessor/) | v13.0.0 | ✅ PROD | Operadores + Supervisores |
| [xls2gsheet](./apps/xls2gsheet/) | v1.0.0 | ✅ PROD | Contabilidad |
| CasinoETL | v1.0.0 | 🚧 Beta | Interno |

---

## 🔧 Librerías

### CLib v6.0.0
**ID**: `1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij`

Core business logic compartida:
- 🔐 Authentication & Authorization
- 🛠️ Services (Logging, Drive, Gemini, BigQuery)
- 🔄 ETL Transforms (AXES, SIELCON)

[Ver documentación completa](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/clib-api.md)

### AngelStyle v4.0.0
**ID**: `19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S`

UI/UX components con AI-Modern Dark Theme:
- 🎨 Paleta corporativa Grupo Angel
- 🧩 Componentes reutilizables
- 📱 Responsive design
- ✨ Glassmorphism effects

[Ver documentación completa](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/angelstyle-api.md)

---

## 🚀 Roadmap

### Q4 2024 ✅
- CLib v6.0.0 estable
- AngelStyle v4.0.0 base
- TicketProcessor en producción
- Documentación completa

### Q1 2025 🚧
- Migración xls2gsheet a arquitectura modular
- Refactor AngelStyle → AI-Modern Dark Theme completo
- InventoryManager (nueva app)
- Contextos AI para Gemini/Claude

### Q2 2025 🔮
- CasinoETL v2.0 con validaciones avanzadas
- BigQuery ML models
- Dashboard ejecutivo BI
- API Gateway para integraciones

---

## 🤝 Contribuir

### Workflow
1. Fork del repo (o crear branch)
2. Seguir [Git Workflow](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/workflows/git-workflow.md)
3. Crear Pull Request
4. Code review
5. Merge a main

### Estándares
- 📝 [Conventional Commits](https://www.conventionalcommits.org/)
- 🔢 [Semantic Versioning](https://semver.org/)
- 📖 JSDoc en funciones públicas
- ✅ Testing antes de PR

---

## 📞 Soporte

### Problemas
- 🐛 [Reportar Bug](https://github.com/ChristianLuciani/grupo-angel-apps/issues/new)
- 📖 [Troubleshooting Guide](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/troubleshooting.md)

### Contacto
- **Developer**: Christian Luciani ([@ChristianLuciani](https://github.com/ChristianLuciani))
- **Docs**: [grupo-angel-docs](https://github.com/ChristianLuciani/grupo-angel-docs)

---

## 📄 Licencia

Privado - Grupo Angel  
© 2024-2025

---

**Última actualización**: Octubre 2025

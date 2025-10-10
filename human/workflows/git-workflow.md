# 🔀 Git Workflow

> Proceso completo de control de versiones para Grupo Angel Apps

---

## 🎯 Objetivo

Mantener código organizado, versionado correctamente y facilitar colaboración mediante un workflow Git estandarizado.

**Beneficios**:
- ✅ Historial claro de cambios
- ✅ Rollback fácil si algo falla
- ✅ Branches organizados
- ✅ Commits descriptivos
- ✅ Colaboración sin conflictos

---

## 📋 Tabla de Contenidos

1. [Branch Strategy](#branch-strategy)
2. [Conventional Commits](#conventional-commits)
3. [Workflow Diario](#workflow-diario)
4. [Pull Requests](#pull-requests)
5. [Tags y Versiones](#tags-y-versiones)
6. [Resolución de Conflictos](#resolución-de-conflictos)
7. [GitHub Desktop](#github-desktop)

---

## 🌳 Branch Strategy

### Estructura de Branches
main (producción)
↓
├── feature/nombre-feature (nuevas funcionalidades)
├── fix/nombre-bug (bug fixes)
├── refactor/nombre-refactor (refactoring)
├── docs/nombre-doc (documentación)
└── hotfix/nombre-urgente (fixes urgentes en prod)

---

### Branch Principal: `main`

**Propósito**: Código en producción

**Reglas**:
- ✅ Siempre debe estar estable
- ✅ Solo se mergea código testeado
- ✅ Cada merge debe tener PR aprobado
- ❌ NO hacer commits directos a main
- ❌ NO pushear código sin testear

**Protección** (configurar en GitHub):
Settings → Branches → Branch protection rules
☑ Require pull request before merging
☑ Require status checks to pass

---

### Feature Branches

**Naming**: `feature/descripcion-corta`

**Ejemplos**:
```bash
feature/inventory-manager
feature/barcode-scanner
feature/export-pdf
feature/gemini-integration
Cuándo usar:

Nueva app completa
Nueva funcionalidad grande
Nueva integración

Lifetime: 1-7 días (mergear rápido)

Fix Branches
Naming: fix/descripcion-del-bug
Ejemplos:
bashfix/login-redirect-loop
fix/bigquery-timeout
fix/mobile-layout-broken
fix/ticket-validation
Cuándo usar:

Bug reportado por usuario
Error en producción (no crítico)
Corrección de comportamiento incorrecto

Lifetime: <24 horas (mergear rápido)

Refactor Branches
Naming: refactor/que-se-refactoriza
Ejemplos:
bashrefactor/xls2gsheet-to-modular
refactor/angelstyle-v5
refactor/clib-etl-module
Cuándo usar:

Mejorar código existente sin cambiar funcionalidad
Migrar a nueva arquitectura
Optimización de performance

Lifetime: 2-7 días

Docs Branches
Naming: docs/que-se-documenta
Ejemplos:
bashdocs/api-reference
docs/onboarding
docs/troubleshooting
Cuándo usar:

Agregar/actualizar documentación
Crear nuevas guías
Actualizar README

Lifetime: <2 días

Hotfix Branches
Naming: hotfix/descripcion-critica
Ejemplos:
bashhotfix/critical-auth-failure
hotfix/data-loss-bigquery
hotfix/app-crash-on-load
Cuándo usar:

⚠️ Bug crítico en producción
⚠️ Sistema caído
⚠️ Pérdida de datos
⚠️ Seguridad comprometida

Lifetime: <4 horas (URGENTE)
Proceso especial:
bash# Crear desde main
git checkout main
git pull
git checkout -b hotfix/critical-bug

# Fix + test RÁPIDO
# ... código

# Commit
git add .
git commit -m "hotfix: descripción urgente"

# Push y crear PR INMEDIATO
git push origin hotfix/critical-bug

# Mergear AHORA (skip normal review si es crítico)
# Deploy inmediato

# Notificar a equipo

📝 Conventional Commits
Formato
<type>(<scope>): <description>

[optional body]

[optional footer]

Types
TypeUsoEmojiEjemplofeatNueva funcionalidad✨feat(inventory): agregar búsqueda de itemsfixBug fix🐛fix(auth): corregir redirect loopdocsDocumentación📝docs(readme): actualizar deployment stepsstyleFormato, CSS💄style(angelstyle): ajustar padding buttonsrefactorRefactoring♻️refactor(clib): extraer función comúnperfPerformance⚡perf(bigquery): optimizar query itemstestTests✅test(inventory): agregar unit testschoreMantenimiento🔧chore(deps): actualizar CLib a v6.1revertRevertir commit⏪revert: feat(inventory): agregar X

Scope
Scope = Qué parte del proyecto afecta
Ejemplos:
bashfeat(inventory): ...      # App InventoryManager
fix(clib): ...            # Librería CLib
docs(guides): ...         # Documentación guides
style(angelstyle): ...    # Librería AngelStyle
refactor(xls2gsheet): ... # App xls2gsheet
Si afecta múltiples partes:
bashfeat(inventory,clib): agregar función compartida
Si afecta todo el proyecto:
bashchore(project): actualizar estructura carpetas

Description
Reglas:

✅ Empezar con verbo en infinitivo
✅ Lowercase (minúsculas)
✅ Sin punto final
✅ Máximo 50 caracteres
✅ Describir QUÉ cambió, no CÓMO

Buenos ejemplos:
bashfeat(inventory): agregar exportación a PDF
fix(auth): corregir validación de email
docs(api): documentar función searchItems
refactor(clib): simplificar lógica de logging
Malos ejemplos:
bash❌ feat(inventory): Add PDF export  # Inglés (usamos español)
❌ fix(auth): Fixed the bug.        # No descriptivo
❌ Updated stuff                     # Sin type/scope
❌ feat: cambios varios              # Muy vago

Body (Opcional)
Explicación detallada del cambio.
Cuándo incluir:

Cambio complejo que necesita contexto
Breaking changes
Referencia a issue

Formato:
bashgit commit -m "feat(inventory): agregar búsqueda avanzada" -m "
Implementa búsqueda por:
- Código de item
- Nombre
- Categoría
- Rango de precios

Incluye paginación (20 items por página).
Optimizado para BigQuery.

Closes #42
"

Footer (Opcional)
Breaking Changes:
bashfeat(clib): cambiar firma de getUserRole

BREAKING CHANGE: getUserRole() ahora requiere parámetro email obligatorio.
Migración: getUserRole() → getUserRole(email)
Referencias a Issues:
bashfix(inventory): corregir bug de stock negativo

Fixes #123
Closes #124
Related to #125

Ejemplos Completos
Ejemplo 1: Feature Simple
bashgit commit -m "feat(inventory): agregar botón de refresh en dashboard"
Ejemplo 2: Fix con Detalles
bashgit commit -m "fix(auth): corregir logout en mobile" -m "
El botón de logout no funcionaba en dispositivos móviles
debido a evento onclick no capturado correctamente.

Cambios:
- Reemplazar onclick por addEventListener
- Agregar preventDefault
- Testear en iPhone y Android

Fixes #89
"
Ejemplo 3: Refactor Grande
bashgit commit -m "refactor(xls2gsheet): migrar a arquitectura modular" -m "
Refactoriza xls2gsheet para usar CLib y AngelStyle.

Antes:
- Código standalone con duplicación
- Estilos inline
- Sin uso de librerías

Después:
- Usa CLib.detectFileType()
- Usa CLib.transformSielconTITO()
- Usa AngelStyle componentes
- Elimina 200 líneas de código duplicado

BREAKING CHANGE: Cambia estructura de archivos.
Requiere actualizar deployment.

Related to #67
"

🔄 Workflow Diario
Escenario 1: Empezar Nueva Feature
bash# 1. Asegurarte estar en main actualizado
cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"
git checkout main
git pull origin main

# 2. Crear branch de feature
git checkout -b feature/barcode-scanner

# 3. Trabajar en código
# ... editar archivos

# 4. Ver cambios
git status
git diff

# 5. Agregar archivos
git add apps/InventoryManager/BarcodeScannerView.html
git add apps/InventoryManager/Code.gs

# 6. Commit con mensaje descriptivo
git commit -m "feat(inventory): agregar scanner de códigos de barras"

# 7. Push a GitHub
git push origin feature/barcode-scanner

# 8. Continuar trabajando (repetir 3-7 según necesites)

# 9. Cuando feature esté completa → crear PR (ver sección PR)

Escenario 2: Fix Rápido
bash# 1. Desde main, crear branch fix
git checkout main
git pull
git checkout -b fix/mobile-button-size

# 2. Hacer el fix
# ... editar código

# 3. Commit rápido
git add .
git commit -m "fix(angelstyle): aumentar tamaño mínimo botones mobile

Botones eran muy pequeños (<44px) en mobile.
Ahora respetan guidelines de accesibilidad.

Fixes #91"

# 4. Push
git push origin fix/mobile-button-size

# 5. Crear PR inmediatamente
# 6. Mergear rápido después de review

Escenario 3: Múltiples Commits en Feature
bash# Working on feature/inventory-reports

# Commit 1: Setup inicial
git add apps/InventoryManager/ReportsView.html
git commit -m "feat(inventory): crear estructura ReportsView"

# Commit 2: Agregar lógica backend
git add apps/InventoryManager/Code.gs
git commit -m "feat(inventory): implementar generación de reportes"

# Commit 3: Integrar con BigQuery
git add apps/InventoryManager/Code.gs
git commit -m "feat(inventory): conectar reportes con BigQuery"

# Commit 4: UI completa
git add apps/InventoryManager/ReportsView.html
git commit -m "feat(inventory): completar UI de filtros de reportes"

# Push todos los commits
git push origin feature/inventory-reports

# Ver historial
git log --oneline
# Output:
# abc1234 feat(inventory): completar UI de filtros de reportes
# def5678 feat(inventory): conectar reportes con BigQuery
# ghi9012 feat(inventory): implementar generación de reportes
# jkl3456 feat(inventory): crear estructura ReportsView

Escenario 4: Actualizar Branch con Cambios de Main
Mientras trabajas en tu feature, otros mergearon código a main.
bash# Estás en feature/my-feature
git status  # Verifica que no tengas cambios sin commit

# Opción A: Merge (recomendado para simplicidad)
git checkout main
git pull origin main
git checkout feature/my-feature
git merge main

# Resolver conflictos si hay (ver sección de conflictos)
# Luego:
git add .
git commit -m "chore: merge main into feature/my-feature"
git push origin feature/my-feature

# Opción B: Rebase (para historial más limpio, avanzado)
git checkout feature/my-feature
git rebase main
# Resolver conflictos si hay
git push --force-with-lease origin feature/my-feature
⚠️ Para ADHD/Dislexia: Usa Opción A (merge) siempre. Es más seguro y visual.

🔍 Pull Requests
Cuándo Crear PR

✅ Feature completa y testeada
✅ Fix funcionando
✅ Código listo para producción
✅ Documentación actualizada
❌ NO crear PR con código "work in progress" (WIP)


Template de PR
markdown## 🎯 Descripción

[Descripción breve de 2-3 líneas]

## 🔗 Issue Relacionado

Closes #[número]

## ✨ Cambios

### Added
- [Nueva funcionalidad 1]
- [Nueva funcionalidad 2]

### Changed
- [Cambio en funcionalidad existente]

### Fixed
- [Bug fix 1]
- [Bug fix 2]

## 📸 Screenshots (si aplica UI)

### Antes
[imagen]

### Después
[imagen]

## ✅ Testing Checklist

- [ ] Unit tests pasando
- [ ] Manual testing completo
- [ ] Responsive testing (mobile/tablet/desktop)
- [ ] Performance aceptable
- [ ] Documentación actualizada

## 🚀 Deployment Notes

[Instrucciones especiales para deploy, si las hay]

## 📝 Notas Adicionales

[Cualquier contexto adicional]

Crear PR en GitHub
Método 1: GitHub Web
bash# 1. Push tu branch
git push origin feature/my-feature

# 2. Ve a GitHub
open https://github.com/ChristianLuciani/grupo-angel-apps

# 3. Verás banner: "Compare & pull request" → Click

# 4. Llenar template de PR

# 5. Reviewers: Asignar a ti mismo o a reviewer

# 6. Labels: Agregar (enhancement, bug, documentation, etc.)

# 7. Create pull request
Método 2: GitHub Desktop
bash# 1. En GitHub Desktop, asegúrate estar en tu branch

# 2. Menu: Branch → Create Pull Request

# 3. Se abre navegador con formulario pre-llenado

# 4. Completar y crear

Review Process
Como Autor (quien creó el PR)
Checklist antes de pedir review:
markdown- [ ] Todos los tests pasan
- [ ] No hay conflictos con main
- [ ] Código documentado con comments
- [ ] README actualizado si es necesario
- [ ] CHANGELOG.md actualizado
- [ ] Screenshots agregados (si cambió UI)
- [ ] Deployment notes claros
Durante review:

✅ Responde a comentarios rápido
✅ Haz cambios solicitados en nuevos commits
✅ Marca conversaciones como "resolved" cuando fixes
❌ NO hagas force push (perderás historial de review)


Como Reviewer
Qué revisar:
markdown### Código
- [ ] Lógica correcta y clara
- [ ] No hay código duplicado
- [ ] Error handling implementado
- [ ] Naming convenciones seguidas
- [ ] No hay console.log() olvidados

### Testing
- [ ] Tests escritos (si corresponde)
- [ ] Edge cases considerados
- [ ] Performance aceptable

### Documentación
- [ ] README actualizado
- [ ] JSDoc en funciones públicas
- [ ] Comments en lógica compleja

### Seguridad
- [ ] No hay API keys expuestas
- [ ] Input validation presente
- [ ] Permisos correctos

### UI (si aplica)
- [ ] Responsive en mobile
- [ ] Componentes AngelStyle usados
- [ ] Accesibilidad (contraste, tamaño táctil)
Cómo dar feedback:
markdown✅ Bueno: "En línea 45, considera usar CLib.logDebugMessage() en lugar de Logger.log() para consistencia"

❌ Malo: "Esto está mal"

✅ Bueno: "Esta función es compleja. ¿Qué te parece extraer la validación a una función separada?"

❌ Malo: "Refactoriza esto"

Mergear PR
Opción 1: Squash and Merge (Recomendado)
Cuándo usar: Feature con múltiples commits WIP
Resultado: Todos los commits se combinan en uno solo
bash# En GitHub:
# 1. Click "Squash and merge"
# 2. Editar mensaje de commit final
# 3. Confirmar

# Localmente después:
git checkout main
git pull origin main
git branch -d feature/my-feature  # Eliminar branch local

Opción 2: Merge Commit
Cuándo usar: Commits bien organizados que quieres preservar
Resultado: Todos los commits individuales se preservan
bash# En GitHub:
# 1. Click "Merge pull request"
# 2. Confirmar

Opción 3: Rebase and Merge (Avanzado)
Cuándo usar: Historial lineal limpio
Resultado: Commits se aplican uno por uno sobre main
⚠️ No recomendado para principiantes

Después del Merge
bash# 1. Actualizar main local
git checkout main
git pull origin main

# 2. Eliminar branch (ya no se necesita)
git branch -d feature/my-feature

# 3. Eliminar branch remoto (opcional, GitHub lo hace auto)
git push origin --delete feature/my-feature

# 4. Verificar que todo está OK
git log --oneline -5

🏷️ Tags y Versiones
Semantic Versioning
MAJOR.MINOR.PATCH
  │     │     │
  │     │     └─ Bug fixes
  │     └─────── New features (backwards compatible)
  └───────────── Breaking changes
Ejemplos:

1.0.0 → Primera versión estable
1.1.0 → Nueva feature (compatible)
1.1.1 → Bug fix
2.0.0 → Breaking change


Cuándo Crear Tag

✅ Después de mergear a main
✅ Antes de deploy a producción
✅ Cuando hay cambios significativos


Crear Tag
bash# 1. Asegúrate estar en main actualizado
git checkout main
git pull origin main

# 2. Crear tag anotado
git tag -a v1.2.0 -m "Release v1.2.0

Features:
- Agregar búsqueda avanzada
- Export de reportes a PDF

Fixes:
- Corregir bug de stock negativo
- Mejorar performance de dashboard
"

# 3. Push tag a GitHub
git push origin v1.2.0

# 4. Verificar en GitHub
open https://github.com/ChristianLuciani/grupo-angel-apps/tags

Ver Tags
bash# Listar todos los tags
git tag

# Output:
# v1.0.0
# v1.1.0
# v1.2.0

# Ver detalles de un tag
git show v1.2.0

# Checkout a un tag específico (solo lectura)
git checkout v1.1.0

GitHub Release
Después de crear tag, crear Release en GitHub:
bash# 1. Ve a Releases
open https://github.com/ChristianLuciani/grupo-angel-apps/releases

# 2. Click "Draft a new release"

# 3. Elegir tag: v1.2.0

# 4. Release title: "Version 1.2.0 - Advanced Search"

# 5. Description (copy from CHANGELOG):
```markdown
## ✨ Features
- Búsqueda avanzada con filtros múltiples
- Export de reportes a PDF

## 🐛 Fixes
- Corregido bug de stock negativo
- Mejorada performance de dashboard

## 📦 Assets
- [InventoryManager-v1.2.0.zip]

## 🚀 Deployment
Script ID: [ID]
URL: [URL]
6. Publish release

---

## ⚔️ Resolución de Conflictos

### Qué es un Conflicto

Sucede cuando dos branches modificaron las mismas líneas de código.

**Ejemplo**:
main:    línea 10: const VERSION = '1.0.0';
feature: línea 10: const VERSION = '1.1.0';

Git no sabe cuál versión mantener.

---

### Identificar Conflicto
```bash
git merge main
# Output:
# Auto-merging apps/InventoryManager/Config.gs
# CONFLICT (content): Merge conflict in apps/InventoryManager/Config.gs
# Automatic merge failed; fix conflicts and then commit the result.

# Ver archivos con conflictos
git status
# Output:
# Unmerged paths:
#   both modified:   apps/InventoryManager/Config.gs

Resolver con GitHub Desktop (Recomendado)

GitHub Desktop detecta conflicto automáticamente
Click en archivo con conflicto
Click "Open in [Editor]" (VS Code)
VS Code muestra opciones visuales:

   <<<<<<< HEAD (Current Change)
   const VERSION = '1.0.0';
   =======
   const VERSION = '1.1.0';
   >>>>>>> feature/my-feature (Incoming Change)
   
   [Accept Current Change]
   [Accept Incoming Change]
   [Accept Both Changes]
   [Compare Changes]

Click en opción apropiada
Guardar archivo
En GitHub Desktop: Mark as resolved
Commit merge


Resolver Manualmente (Terminal)
bash# 1. Abrir archivo con conflicto
code apps/InventoryManager/Config.gs

# 2. Buscar marcadores de conflicto
# <<<<<<< HEAD
# ... código de main
# =======
# ... código de tu branch
# >>>>>>> feature/my-feature

# 3. Editar manualmente, decidir qué mantener
# Eliminar marcadores (<<<<, ====, >>>>)
# Dejar solo el código correcto

# 4. Guardar archivo

# 5. Marcar como resuelto
git add apps/InventoryManager/Config.gs

# 6. Verificar que no hay más conflictos
git status

# 7. Commit del merge
git commit -m "chore: resolver conflictos merge main into feature"

# 8. Push
git push origin feature/my-feature

Ejemplo Completo de Resolución
Antes (archivo con conflicto):
javascript<<<<<<< HEAD
const PROJECT_VERSION = '1.0.0';
const FEATURE_FLAG_REPORTS = false;
=======
const PROJECT_VERSION = '1.1.0';
const FEATURE_FLAG_REPORTS = true;
const NEW_SETTING = 'value';
>>>>>>> feature/reports
Decisión: Mantener versión nueva + nuevo setting + feature flag true
Después (resuelto):
javascriptconst PROJECT_VERSION = '1.1.0';
const FEATURE_FLAG_REPORTS = true;
const NEW_SETTING = 'value';

Prevenir Conflictos

Pull main frecuentemente

bash   # Cada mañana
   git checkout main
   git pull
   git checkout feature/my-feature
   git merge main

Branches cortos (1-7 días)
Comunicación con equipo sobre qué archivos están editando
Dividir trabajo en archivos diferentes cuando sea posible


💻 GitHub Desktop
Ventajas para ADHD/Dislexia

✅ Visual (ver cambios con colores)
✅ No necesitas recordar comandos
✅ Previene errores comunes
✅ Resolución de conflictos más simple


Workflow en GitHub Desktop
1. Ver Cambios
Changes tab → Lista de archivos modificados
Click en archivo → Ver diff visual
Verde = Líneas agregadas
Rojo = Líneas eliminadas
Amarillo = Líneas modificadas

2. Hacer Commit
1. Check archivos que quieres incluir
2. Escribir mensaje en "Summary" (título)
3. (Opcional) Descripción en "Description"
4. Click "Commit to [branch-name]"

3. Push
Click "Push origin" (arriba derecha)

4. Pull
Click "Fetch origin" → Si hay cambios → "Pull origin"

5. Crear Branch
Branch menu → New Branch
Nombre: feature/my-feature
Base: main
Create Branch

6. Cambiar Branch
Branch menu → Click en branch deseado

7. Ver Historial
History tab → Ver todos los commits con diffs

🔧 Mantenimiento de Esta Documentación
Cuándo Actualizar
✅ Cuando cambies el workflow
Si decides cambiar algún proceso:

Actualiza secciones relevantes
Agrega nota:

markdown   > **Actualizado 2025-10-15**: Ahora usamos squash merge por default

Notifica al equipo


✅ Cuando encuentres errores
Si un comando no funciona:

Corrige el comando
Agrega troubleshooting:

markdown   ### Error: "fatal: not a git repository"
   
   **Causa**: No estás en carpeta de repo
   
   **Solución**:
   \```bash
   cd "/path/to/grupo-angel-apps"
   \```

✅ Cuando agregues nuevos tipos de branches
Ejemplo: decides agregar experiment/ branches

Agrega a sección "Branch Strategy"
Actualiza diagrama
Agrega ejemplos
Actualiza conventional commits si es necesario


Template para Casos de Uso Nuevos
markdown### Escenario X: [Título]

**Situación**: [Descripción del caso]

**Pasos**:
\```bash
# 1. [Paso 1 con comando]
git ...

# 2. [Paso 2]
git ...

# 3. [Resultado esperado]
\```

**Notas**:
- ✅ [Tip útil]
- ⚠️ [Advertencia importante]

📚 Cheat Sheet Rápido
Comandos Más Usados
bash# === DAILY WORKFLOW ===
git status                              # Ver qué cambió
git diff                                # Ver cambios detallados
git add [archivo]                       # Preparar archivo
git add .                               # Preparar todos
git commit -m "type(scope): mensaje"   # Commit
git push origin [branch]                # Subir a GitHub
git pull origin main                    # Bajar cambios de main

# === BRANCHES ===
git branch                              # Listar branches
git checkout -b feature/nombre          # Crear y cambiar a branch
git checkout main                       # Cambiar a main
git branch -d feature/nombre            # Eliminar branch local

# === SYNC ===
git fetch origin                        # Ver cambios remotos
git pull origin main                    # Bajar main
git merge main                          # Mergear main en branch actual

# === TAGS ===
git tag                                 # Listar tags
git tag -a v1.0.0 -m "Release 1.0"     # Crear tag
git push origin v1.0.0                  # Subir tag

# === INFO ===
git log --oneline                       # Ver historial resumido
git log --oneline -5                    # Últimos 5 commits
git show [commit-hash]                  # Ver detalles de commit

# === UNDO ===
git restore [archivo]                   # Descartar cambios no staged
git restore --staged [archivo]          # Unstage archivo
git reset HEAD~1                        # Deshacer último commit (mantener cambios)
git reset --hard HEAD~1                 # Deshacer último commit (ELIMINAR cambios)

🔗 Links Relacionados

📖 Local Setup
📖 Creating New App
📖 Deployment Guide (próximamente)
📖 Troubleshooting (próximamente)
🔗 Conventional Commits
🔗 Semantic Versioning


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

# 🚀 Deployment Guide

> Proceso completo de deployment para Apps Script desde desarrollo hasta producción

---

## 🎯 Objetivo

Desplegar aplicaciones de manera segura, predecible y con capacidad de rollback.

**Principios**:
- ✅ Deploy solo código testeado
- ✅ Deployment incremental (TEST → PROD)
- ✅ Smoke tests obligatorios
- ✅ Rollback plan siempre listo
- ✅ Documentar cada deployment

---

## 📋 Tabla de Contenidos

1. [Entornos](#entornos)
2. [Pre-Deployment Checklist](#pre-deployment-checklist)
3. [Proceso de Deployment](#proceso-de-deployment)
4. [Post-Deployment](#post-deployment)
5. [Rollback](#rollback)
6. [Hotfix Deployment](#hotfix-deployment)
7. [Deployment de Librerías](#deployment-de-librerías)

---

## 🌍 Entornos

### Desarrollo (DEV)

**Dónde**: Local + Apps Script Editor Online

**Propósito**: 
- Experimentación
- Desarrollo activo
- Testing inicial

**Acceso**: Solo tú

**Datos**: Mock data o test data

---

### Testing (TEST)

**Dónde**: Apps Script Test Deployment

**Propósito**:
- Testing completo pre-producción
- QA manual
- Validación con datos reales (safe)

**Acceso**: Equipo interno

**URL**: `https://script.google.com/macros/s/[TEST-DEPLOYMENT-ID]/exec`

---

### Producción (PROD)

**Dónde**: Apps Script Production Deployment

**Propósito**:
- Aplicación en uso por usuarios finales
- Datos reales
- Alta disponibilidad

**Acceso**: Usuarios finales

**URL**: `https://script.google.com/macros/s/[PROD-DEPLOYMENT-ID]/exec`

---

## ✅ Pre-Deployment Checklist

### Checklist Completo
```markdown
## Pre-Deployment: [App Name] v[Version]

### 📝 Código
- [ ] Todos los commits mergeados a main
- [ ] Tag creado: `v[X.Y.Z]`
- [ ] No hay `console.log()` olvidados
- [ ] No hay `TODO:` críticos sin resolver
- [ ] Código formateado consistentemente

### 🧪 Testing
- [ ] Unit tests pasando (si existen)
- [ ] Manual testing completo
- [ ] Testing en múltiples navegadores
  - [ ] Chrome
  - [ ] Firefox
  - [ ] Safari
- [ ] Testing responsive
  - [ ] Desktop
  - [ ] Tablet
  - [ ] Mobile (iOS y Android)
- [ ] Performance aceptable (< 5s load)

### 📚 Documentación
- [ ] README.md actualizado
- [ ] CHANGELOG.md con nueva versión
- [ ] CURRENT_STATE.md actualizado
- [ ] API docs actualizadas (si aplica)
- [ ] Deployment notes escritas

### 🔐 Seguridad
- [ ] No hay API keys expuestas
- [ ] OAuth scopes verificados
- [ ] Validación de inputs implementada
- [ ] Role-based access funcionando

### 🔧 Configuración
- [ ] Script Properties configuradas
- [ ] Environment variables verificadas
- [ ] BigQuery tables creadas (si nuevas)
- [ ] Drive folders creados (si nuevos)
- [ ] CLib y AngelStyle versiones correctas

### 🎯 Deployment Plan
- [ ] Deployment notes claras
- [ ] Breaking changes documentados
- [ ] Migration steps escritos (si aplica)
- [ ] Rollback plan definido
- [ ] Smoke test checklist preparado

### 👥 Comunicación
- [ ] Equipo notificado del deployment
- [ ] Usuarios notificados (si cambios grandes)
- [ ] Downtime comunicado (si aplica)
- [ ] Support preparado para issues

Deployment Notes Template
markdown## Deployment Notes: [App] v[Version]

### 🎯 Release Type
- [ ] Feature release
- [ ] Bug fix
- [ ] Hotfix
- [ ] Breaking change

### 📝 Changes Summary
[2-3 líneas describiendo cambios principales]

### ⚠️ Breaking Changes
[Si aplica, listar breaking changes y cómo migrar]

### 📦 Dependencies
- CLib: v[X.Y.Z]
- AngelStyle: v[X.Y.Z]
- BigQuery API: v2
- Otras: [listar]

### 🔧 Configuration Changes
[Nuevas Script Properties, variables de entorno, etc.]

### 🗄️ Database Changes
[Nuevas tablas, cambios de schema, migrations]

### 🧪 Smoke Test Steps
1. [Paso 1]
2. [Paso 2]
3. [Paso 3]

### ⏱️ Estimated Downtime
[0 minutos / 5 minutos / etc.]

### 📞 Rollback Contact
[Tu nombre y contacto en caso de problemas]

🚀 Proceso de Deployment
PASO 1: Preparación (10 min)
1.1 Completar Pre-Deployment Checklist
bash# Revisar checklist completo
# Marcar cada item como completado

1.2 Crear Tag de Versión
bashcd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"

# Asegurarse estar en main actualizado
git checkout main
git pull origin main

# Crear tag con release notes
git tag -a v1.2.0 -m "Release v1.2.0 - Búsqueda Avanzada

Features:
- Búsqueda por múltiples criterios
- Exportación de resultados a PDF
- Filtros guardados

Fixes:
- Corregido bug de paginación
- Mejorado performance en mobile

BREAKING CHANGES:
- API searchItems() ahora requiere objeto de filtros
  Antes: searchItems(query)
  Ahora: searchItems({query, filters, page})
"

# Push tag
git push origin v1.2.0

# Verificar tag
git show v1.2.0

1.3 Actualizar CHANGELOG.md
bashcd apps/InventoryManager

# Editar CHANGELOG.md
cat >> CHANGELOG.md << 'CHANGELOG'

## [1.2.0] - 2025-10-09

### Added
- ✨ Búsqueda avanzada con múltiples filtros
- ✨ Exportación de resultados a PDF
- ✨ Filtros guardados por usuario

### Changed
- 🔄 API searchItems() ahora acepta objeto de filtros (BREAKING)

### Fixed
- 🐛 Corregido bug de paginación en resultados
- 🐛 Mejorado performance en dispositivos mobile

### Security
- 🔒 Validación adicional en parámetros de búsqueda
CHANGELOG

git add CHANGELOG.md
git commit -m "docs(inventory): actualizar CHANGELOG para v1.2.0"
git push origin main

PASO 2: Deploy a TEST (15 min)
2.1 Abrir Apps Script Editor
bash# Opción A: Desde script.google.com
open https://script.google.com

# Buscar tu proyecto: InventoryManager
# Click para abrir

# Opción B: Desde clasp (si configurado)
cd apps/InventoryManager
clasp open

2.2 Verificar Código Actualizado
javascript// En Code.gs, verificar que version está actualizada
const PROJECT_VERSION = '1.2.0';  // ✅ Debe coincidir con tag
Si el código no está actualizado:
bash# Pull desde online a local
clasp pull

# O push desde local a online
clasp push

2.3 Crear Test Deployment

En Apps Script Editor:

Click "Deploy" → "Test deployments"


Configurar:

Type: Web app
Description: TEST v1.2.0 - Búsqueda avanzada
Execute as: User accessing the web app
Who has access: Anyone (o Anyone within [domain])


Deploy:

Click "Deploy"
Copiar URL: https://script.google.com/macros/s/.../dev


Guardar URL en Deployment Notes


2.4 Smoke Test en TEST
markdown## Smoke Test: InventoryManager v1.2.0 TEST

### Login & Auth
- [ ] URL carga correctamente
- [ ] Login con cuenta test funciona
- [ ] Usuario correcto mostrado en header
- [ ] Rol correcto asignado

### Funcionalidad Core
- [ ] Dashboard carga con datos
- [ ] Registrar movimiento funciona
- [ ] Búsqueda simple funciona
- [ ] Ver detalles funciona

### Nuevas Features (v1.2.0)
- [ ] Búsqueda avanzada abre modal
- [ ] Filtros múltiples funcionan
- [ ] Resultados paginan correctamente
- [ ] Exportar PDF genera archivo
- [ ] Filtros guardados persisten

### UI/UX
- [ ] Estilos cargan correctamente
- [ ] No hay errores en console (F12)
- [ ] Responsive en mobile
- [ ] Todos los botones funcionan

### Performance
- [ ] Carga inicial < 5 segundos
- [ ] Búsqueda responde < 2 segundos
- [ ] Export PDF completa < 10 segundos

### Data Integrity
- [ ] Datos se guardan en BigQuery
- [ ] Schema correcto
- [ ] No hay data loss
- [ ] Timestamps correctos

**Resultado**: ✅ PASS / ❌ FAIL

**Notas**: [Cualquier observación]

2.5 Testing con Usuarios Beta (Opcional)
Si tienes usuarios beta:
markdown## Beta Testing

### Participantes
- Usuario 1: [nombre] - [rol]
- Usuario 2: [nombre] - [rol]

### Testing Period
- Inicio: 2025-10-09 10:00
- Fin: 2025-10-09 16:00

### Feedback Recibido
- [Feedback 1]
- [Feedback 2]

### Issues Encontrados
- [Issue 1] - Severidad: [High/Med/Low] - Status: [Fixed/Deferred]

### Decision
- [ ] Proceder a PROD
- [ ] Fix issues primero
- [ ] Rollback TEST

PASO 3: Deploy a PRODUCCIÓN (10 min)
⚠️ SOLO proceder si smoke test en TEST pasó completamente

3.1 Backup de Versión Actual
Antes de deploy nuevo, documentar versión actual:
markdown## Backup Info: Pre-v1.2.0

**Versión actual en PROD**: v1.1.0
**Deployment ID**: AKfyc...xyz123
**Deployment Date**: 2025-09-15
**URL**: https://script.google.com/macros/s/AKfyc...xyz123/exec

**Script Properties**:
- UPLOADS_FOLDER_ID: [ID]
- REPORTS_FOLDER_ID: [ID]

**Known State**:
- Users: ~50 activos
- Data: ~1,000 movimientos en BigQuery
- Performance: 3-5s average load

3.2 Crear Production Deployment

En Apps Script Editor:

Click "Deploy" → "New deployment"


Configurar:

Type: Web app
Description: v1.2.0 - Búsqueda avanzada (PRODUCTION)
Execute as: User accessing the web app
Who has access: Anyone (o domain)


Deploy:

Click "Deploy"
⚠️ Confirmar deployment a PROD
Copiar nuevo Deployment ID
Copiar nueva URL


Actualizar README.md con nueva URL:

markdown   ### Production URL
   https://script.google.com/macros/s/[NUEVO-ID]/exec
   
   ### Deployment History
   - v1.2.0 (2025-10-09): Búsqueda avanzada
   - v1.1.0 (2025-09-15): Sistema de aprobación
   - v1.0.0 (2025-09-01): Release inicial

3.3 Smoke Test en PRODUCCIÓN
⏱️ CRÍTICO: Ejecutar en primeros 5 minutos post-deploy
markdown## Smoke Test: PRODUCTION v1.2.0

### Critical Path (5 min max)
- [ ] URL PROD carga
- [ ] Login funciona
- [ ] Dashboard muestra datos reales
- [ ] Crear movimiento nuevo funciona
- [ ] Datos llegan a BigQuery
- [ ] No hay errores en console

**Resultado**: ✅ / ❌

**Timestamp**: [hora exacta]

3.4 Monitoring Inicial (30 min)
Después de deploy, monitorear:
markdown## Post-Deploy Monitoring

### Metrics (primeros 30 min)

**Errores**:
- Apps Script Executions → Failures: [número]
- Console errors reportados: [número]

**Performance**:
- Average load time: [segundos]
- API response time: [segundos]

**Usage**:
- Usuarios activos: [número]
- Requests totales: [número]

**Issues Reportados**:
- [Ninguno / Listar issues]

**Decision**: ✅ Deploy exitoso / ⚠️ Monitor continuo / ❌ Rollback necesario

PASO 4: Post-Deployment (15 min)
4.1 Actualizar Documentación
bash# 1. README.md
# Ya actualizado en Step 3.2

# 2. CURRENT_STATE.md
cd apps/InventoryManager
nano CURRENT_STATE.md

# Actualizar:
# - Versión actual
# - Deployment date
# - Deployment URL
# - Nuevas features implementadas
# - Métricas actualizadas

# 3. Commit cambios
git add README.md CURRENT_STATE.md
git commit -m "docs(inventory): actualizar docs post-deployment v1.2.0"
git push origin main

4.2 Crear GitHub Release
bash# 1. Ve a GitHub Releases
open https://github.com/ChristianLuciani/grupo-angel-apps/releases/new

# 2. Configurar release:
Form fields:
markdownTag: v1.2.0 (existente)

Release title: Version 1.2.0 - Advanced Search

Description:
## ✨ Features

- **Búsqueda Avanzada**: Filtra por múltiples criterios simultáneamente
- **Export PDF**: Exporta resultados de búsqueda a PDF
- **Filtros Guardados**: Guarda tus filtros favoritos

## 🐛 Fixes

- Corregido bug de paginación en resultados largos
- Mejorado performance en dispositivos mobile (50% más rápido)

## 🔄 Breaking Changes

⚠️ **API Change**: `searchItems()` ahora requiere objeto
```javascript
// Antes
searchItems(query)

// Ahora
searchItems({query, filters, page})
📦 Assets
Deployment Info:

Script ID: 1ABC...XYZ
URL: https://script.google.com/macros/s/.../exec
Deployed: 2025-10-09

📚 Documentation

README
CHANGELOG
API Docs


Full Changelog: https://github.com/.../compare/v1.1.0...v1.2.0

# 3. Publish release

4.3 Notificar Stakeholders
Template de email/mensaje:
markdownSubject: ✅ InventoryManager v1.2.0 Deployed

Hola equipo,

Se ha desplegado exitosamente **InventoryManager v1.2.0** a producción.

**Nuevas Funcionalidades**:
- 🔍 Búsqueda avanzada con filtros múltiples
- 📄 Exportación de resultados a PDF
- 💾 Filtros guardados

**Mejoras**:
- ⚡ 50% más rápido en mobile
- 🐛 Corregido bug de paginación

**URL**: https://script.google.com/macros/s/.../exec

**Documentación**: Ver [CHANGELOG](link)

**Soporte**: Cualquier issue, reportar en Slack #tech-support

Saludos,
[Tu nombre]

4.4 Archivar Deployment Anterior (Opcional)
En Apps Script:

Deploy → Manage deployments
Encontrar deployment anterior (v1.1.0)
Click "..." → Archive
Confirmar

⚠️ Nota: No eliminar, solo archivar. Permite rollback.

⏪ Rollback
Cuándo Hacer Rollback
Rollback inmediato si:

🔴 Error 500 en producción
🔴 Funcionalidad crítica no funciona
🔴 Pérdida de datos
🔴 Performance inaceptable (>10s load)
🔴 Múltiples reportes de usuarios

Monitor si:

🟡 Bug menor no crítico
🟡 Issue de UI cosmético
🟡 Performance levemente degradada


Proceso de Rollback (5 min)
Método 1: Activar Deployment Anterior
FASTEST - Recomendado para emergencias
markdown## Rollback Steps

1. **Apps Script Editor**
   - Deploy → Manage deployments
   
2. **Encontrar versión anterior**
   - v1.1.0 (archived)
   
3. **Activar**
   - Click "..." → "Set as active deployment"
   - Confirmar
   
4. **Verificar**
   - URL vuelve a versión anterior automáticamente
   - Smoke test rápido
   
5. **Notificar**
   - Informar a equipo del rollback
   - Investigar causa del problema

**Tiempo total**: ~2 minutos

Método 2: Redeploy Versión Anterior
Cuando Método 1 no es suficiente
bash# 1. Checkout tag anterior
git checkout v1.1.0

# 2. Push a Apps Script
cd apps/InventoryManager
clasp push

# 3. Deploy en Apps Script
# (seguir proceso normal de deployment)

# 4. Volver a main
git checkout main

# 5. Investigar problema en branch separado
git checkout -b hotfix/rollback-investigation

Post-Rollback
markdown## Post-Rollback Actions

### Immediate (10 min)
- [ ] Smoke test versión rolled back
- [ ] Notificar a usuarios (si aplica)
- [ ] Actualizar status page (si existe)
- [ ] Comunicar en Slack/Chat interno

### Short-term (1-4 hours)
- [ ] Identificar causa raíz del problema
- [ ] Reproducir issue en TEST
- [ ] Fix implementado
- [ ] Testing exhaustivo del fix

### Before Next Deploy
- [ ] Fix verificado en TEST
- [ ] Smoke test extendido
- [ ] Deployment notes actualizadas con lecciones
- [ ] Post-mortem document (si fue crítico)

Post-Mortem Template (Para Incidents Críticos)
markdown## Post-Mortem: Rollback v1.2.0

**Incident Date**: 2025-10-09 14:30
**Detection Time**: 2 minutos (usuario reportó)
**Resolution Time**: 5 minutos (rollback completado)
**Impact**: ~20 usuarios afectados, ~5 min downtime

### What Happened
[Descripción cronológica del incident]

### Root Cause
[Causa técnica específica]

### Timeline
- 14:30 - Deploy v1.2.0 a PROD
- 14:32 - Usuario reporta error 500
- 14:33 - Verificado en PROD, error reproducible
- 14:34 - Decision: Rollback inmediato
- 14:35 - Rollback a v1.1.0 completado
- 14:37 - Smoke test OK, sistema estable

### What Went Wrong
- [ ] Testing insuficiente en TEST
- [ ] Edge case no considerado
- [ ] Performance issue no detectado
- [ ] Breaking change no documentado

### What Went Right
- [x] Rollback rápido y efectivo
- [x] Monitoring detectó issue rápido
- [x] Comunicación clara con equipo
- [x] Backup disponible y funcional

### Action Items
- [ ] [Acción 1] - Owner: [nombre] - Due: [fecha]
- [ ] [Acción 2] - Owner: [nombre] - Due: [fecha]

### Lessons Learned
- [Lección 1]
- [Lección 2]

🚨 Hotfix Deployment
Proceso acelerado para fixes críticos en producción.

Criterios para Hotfix
Hotfix si:

🔴 Sistema caído o inutilizable
🔴 Pérdida de datos
🔴 Vulnerabilidad de seguridad
🔴 Error que afecta a mayoría de usuarios

NO hotfix si:

🟡 Bug menor que puede esperar
🟡 Issue cosmético
🟡 Feature request


Proceso Hotfix (30 min total)
Step 1: Identificación y Fix (15 min)
bash# 1. Crear hotfix branch desde main
git checkout main
git pull
git checkout -b hotfix/critical-search-error

# 2. Reproducir issue
# 3. Implementar fix MÍNIMO
# 4. Test local inmediato

# 5. Commit
git add .
git commit -m "hotfix(inventory): fix critical search error

Error 500 when searching with special characters.
Added input sanitization.

Fixes #234"

# 6. Push
git push origin hotfix/critical-search-error

Step 2: Deploy Inmediato (5 min)
markdown## Hotfix Deploy Checklist (Minimal)

- [ ] Fix implementado y testeado localmente
- [ ] Commit pushed
- [ ] Deploy a TEST
- [ ] Smoke test en TEST (solo funcionalidad afectada)
- [ ] Deploy a PROD
- [ ] Smoke test en PROD (solo funcionalidad afectada)
- [ ] Monitoring activo
⚠️ Skip: Testing extenso, PR review (hacer después)

Step 3: Deploy a PROD
bash# 1. Push a Apps Script
clasp push

# 2. Deploy con nota de HOTFIX
# Description: "HOTFIX v1.2.1 - Critical search error"

# 3. Smoke test inmediato
# 4. Monitor por 10 minutos

Step 4: Post-Hotfix Cleanup (10 min)
bash# 1. Crear tag
git tag -a v1.2.1 -m "Hotfix v1.2.1 - Critical search error"
git push origin v1.2.1

# 2. Merge hotfix a main
git checkout main
git merge hotfix/critical-search-error
git push origin main

# 3. Crear PR para review post-facto
# (para que equipo revise el fix)

# 4. Actualizar CHANGELOG
# 5. Notificar equipo
# 6. Delete hotfix branch
git branch -d hotfix/critical-search-error

📚 Deployment de Librerías
Proceso especial para CLib y AngelStyle.

CLib Deployment
Consideraciones

⚠️ Breaking changes afectan TODAS las apps
✅ Testing exhaustivo necesario
✅ Versioning estricto (Semantic Versioning)


Proceso
markdown## CLib v6.1.0 Deployment

### Pre-Deploy
- [ ] Todos los tests unitarios pasan
- [ ] Funciones documentadas con JSDoc
- [ ] CURRENT_STATE.md actualizado
- [ ] Breaking changes documentados
- [ ] Migration guide escrito (si aplica)

### Testing
- [ ] Probado en TicketProcessor
- [ ] Probado en xls2gsheet
- [ ] Probado en InventoryManager
- [ ] Probado en app nueva (sandbox)

### Deploy
1. Apps Script Editor → CLib project
2. Deploy → Manage deployments
3. Create version: v6.1.0
4. Description: [features/fixes]
5. Save new version

### Post-Deploy
- [ ] Actualizar apps a nueva versión:
```json
  "libraries": [{
    "version": "7",  // incrementar
    "libraryId": "..."
  }]

 Test cada app después de actualizar
 Create GitHub release
 Notify team

Rollback Plan

 Version anterior documented: v6.0.0
 Como volver: cambiar version en appsscript.json


---

### AngelStyle Deployment

#### Consideraciones

- 🎨 **Cambios visuales afectan todas las apps**
- ✅ Testing visual en todas las apps necesario
- ✅ Screenshots antes/después recomendados

---

#### Proceso
```markdown
## AngelStyle v4.1.0 Deployment

### Pre-Deploy
- [ ] CSS validado
- [ ] JavaScript helpers funcionan
- [ ] No hay breaking changes de clases
- [ ] Browser compatibility verificado

### Visual Testing (CRÍTICO)
- [ ] TicketProcessor → Operator view
- [ ] TicketProcessor → Supervisor view
- [ ] InventoryManager → todas las vistas
- [ ] xls2gsheet → interfaz principal
- [ ] Mobile testing todas las apps

### Deploy
1. CLibraryManager (script específico)
2. Deploy → New version: v4.1.0
3. Description: "Agregar componente .angel-badge"

### Update Apps
Cada app debe actualizar versión en appsscript.json:
```json
{
  "libraryId": "19iL...",
  "version": "5"  // nueva versión
}
Verification

 Hard refresh (Ctrl+F5) en cada app
 Estilos cargan correctamente
 No hay CSS conflicts
 Responsive OK


---

## 🔧 Mantenimiento de Esta Documentación

### Cuándo Actualizar

#### ✅ **Después de cada deployment problemático**

Si tuviste un deployment difícil:

1. Documenta qué salió mal
2. Agrega al troubleshooting:
```markdown
   ### Issue: [Descripción]
   
   **Síntomas**: [Qué viste]
   
   **Causa**: [Por qué pasó]
   
   **Solución**: [Cómo lo resolviste]
   
   **Prevención**: [Cómo evitarlo en futuro]

✅ Cuando cambies el proceso
Si mejoras el proceso:

Actualiza secciones relevantes
Agrega nota de cambio:

markdown   > **Actualizado 2025-10-15**: Ahora usamos GitHub Releases en todos los deploys

Notifica al equipo


✅ Cuando agregues nuevas herramientas
Si adoptas nueva herramienta de deployment:

Agrega sección nueva
Compara con método anterior
Documenta pros/cons
Incluye tutorial paso a paso


Template para Deployment Report
markdown## Deployment Report: [App] v[Version]

**Date**: 2025-10-09
**Time**: 14:30 - 15:00 (30 min total)
**Deployer**: [Tu nombre]

### Pre-Deploy
- Checklist completed: ✅
- Tests passing: ✅
- Documentation updated: ✅

### TEST Deploy
- Time: 14:35
- Smoke test: ✅ PASS
- Issues found: None

### PROD Deploy
- Time: 14:45
- Smoke test: ✅ PASS
- Initial monitoring: ✅ OK

### Post-Deploy
- GitHub Release: ✅ Created
- Team notified: ✅
- Documentation updated: ✅

### Metrics (24h)
- Errors: 0
- Performance: 3.2s avg (vs 3.5s before)
- User feedback: Positive

### Issues
None

### Lessons Learned
- [Algo nuevo que aprendiste]

### Next Steps
- [Próximos deployments planeados]

📊 Deployment Metrics
TrackRetryClaude does not have the ability to run the code it generates yet.CLContinue2 / 2estos metrics para mejorar proceso:
markdown## Deployment Metrics Dashboard

### Frequency
- Deployments last month: [número]
- Average time between deployments: [días]
- Hotfixes last month: [número]

### Success Rate
- Successful deployments: [%]
- Rollbacks required: [número]
- Average rollback time: [minutos]

### Performance
- Average deployment time: [minutos]
- Pre-deploy checklist time: [minutos]
- Testing time: [minutos]
- Monitoring time: [minutos]

### Quality
- Issues found in TEST: [número]
- Issues found in PROD: [número]
- Post-deploy bugs reported: [número]
- Time to fix post-deploy bugs: [horas]

### Goals
- [ ] 100% smoke test pass rate
- [ ] <1% rollback rate
- [ ] <30 min deployment time
- [ ] 0 critical issues in PROD

🚀 Advanced: Automated Deployment (Futuro)
GitHub Actions para Apps Script
Futuro enhancement - Automatizar deployment con CI/CD:
yaml# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    tags:
      - 'v*'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      
      - name: Install clasp
        run: npm install -g @google/clasp
      
      - name: Deploy to Apps Script
        run: |
          echo "${{ secrets.CLASP_CREDENTIALS }}" > ~/.clasprc.json
          clasp push
          clasp deploy -d "Auto-deploy ${{ github.ref_name }}"
      
      - name: Run smoke tests
        run: npm run test:smoke
      
      - name: Notify on failure
        if: failure()
        uses: actions/slack@v1
        with:
          status: ${{ job.status }}
⚠️ Nota: Esto es avanzado. Implementar solo cuando el proceso manual esté perfeccionado.

📋 Quick Reference
Deployment Commands
bash# === PRE-DEPLOY ===
git checkout main && git pull                    # Update main
git tag -a v1.2.0 -m "Release notes"            # Create tag
git push origin v1.2.0                           # Push tag

# === CLASP DEPLOY ===
cd apps/[AppName]                                # Navigate
clasp push                                       # Push code
clasp open                                       # Open in browser
# Then use UI to deploy

# === POST-DEPLOY ===
git add README.md CHANGELOG.md CURRENT_STATE.md  # Update docs
git commit -m "docs: update post-deployment"     # Commit
git push origin main                             # Push

# === ROLLBACK ===
git checkout v1.1.0                              # Checkout previous
clasp push                                       # Push old version
# Deploy in UI
git checkout main                                # Return to main

Emergency Contacts
markdown## Deployment Emergency Contacts

### Technical Issues
- Developer: Christian Luciani - [contact]
- Backup: [nombre] - [contact]

### Business Issues
- Product Owner: [nombre] - [contact]
- Stakeholder: [nombre] - [contact]

### Infrastructure
- GCP Admin: [nombre] - [contact]
- Apps Script Support: [link to support]

### Escalation Path
1. Try rollback (5 min)
2. Contact developer (immediately)
3. Contact backup developer (if unavailable)
4. Escalate to GCP admin (if infrastructure issue)

🔗 Links Relacionados

📖 Git Workflow
📖 Creating New App
📖 Troubleshooting (próximamente)
📖 Local Setup
🔗 Apps Script Deployment Docs
🔗 Semantic Versioning


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

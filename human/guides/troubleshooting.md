# 🔧 Troubleshooting Guide

> Solución de problemas comunes en Grupo Angel Apps

---

## 🎯 Objetivo

Resolver problemas rápidamente con soluciones probadas y documentadas.

**Cómo usar esta guía**:
1. Busca tu error específico con Ctrl+F
2. Sigue los pasos de solución
3. Si no funciona, ve a "Escalation"
4. Documenta nuevos problemas que encuentres

---

## 📋 Tabla de Contenidos

### Errores Comunes
1. [Authentication & Authorization](#authentication--authorization)
2. [Apps Script Errors](#apps-script-errors)
3. [BigQuery Issues](#bigquery-issues)
4. [CLib Errors](#clib-errors)
5. [AngelStyle UI Issues](#angelstyle-ui-issues)
6. [Performance Problems](#performance-problems)
7. [Deployment Issues](#deployment-issues)
8. [Git/GitHub Issues](#gitgithub-issues)

---

## 🔐 Authentication & Authorization

### Error: "Usuario no autenticado"

**Síntomas**:
- AccessDenied page aparece
- Mensaje: "No se pudo autenticar usuario"

**Causas**:
1. No estás logueado en Google
2. Sesión expirada
3. Cuenta incorrecta (personal vs corporativa)

**Soluciones**:
```markdown
### Paso 1: Verificar login
1. Abre ventana de incógnito
2. Ve a accounts.google.com
3. ¿Ves tu cuenta? → Logueado
4. ¿No ves tu cuenta? → Login necesario

### Paso 2: Verificar cuenta correcta
1. Debe ser cuenta @grupoangel.com
2. NO cuenta personal (@gmail.com)
3. Si estás con cuenta personal:
   - accounts.google.com
   - Cambiar a cuenta corporativa
   - Volver a abrir app

### Paso 3: Limpiar sesión
1. Ctrl+Shift+Delete (Chrome)
2. Seleccionar:
   - [x] Cookies y datos de sitios
   - [x] Imágenes y archivos en caché
3. Rango: Última hora
4. Borrar datos
5. Volver a login

Error: "Rol no autorizado"
Síntomas:

AccessDenied aparece
Mensaje: "Rol no autorizado: undefined"

Causa:
Tu email no está en las listas de roles (Config.gs)
Solución:
javascript// 1. Verificar Config.gs de la app
// Archivo: apps/[AppName]/Config.gs

const ADMIN_EMAILS = [
  'admin@grupoangel.com',
  'cluciani@gmail.com'
];

const SUPERVISOR_EMAILS = [
  'supervisor@grupoangel.com'
];

// 2. Tu email debe estar en una de estas listas

// 3. Si no está, agregar:
const SUPERVISOR_EMAILS = [
  'supervisor@grupoangel.com',
  'tunombre@grupoangel.com'  // AGREGAR AQUÍ
];

// 4. Deploy nueva versión de la app
// Ver: docs/human/workflows/deployment.md

Error: "getActiveUser() returns undefined"
Síntomas:

Error en logs: Cannot read property 'email' of undefined
App no carga

Causa:
OAuth scope faltante en manifest
Solución:
json// Verificar appsscript.json
{
  "oauthScopes": [
    "https://www.googleapis.com/auth/userinfo.email",  // ← NECESARIO
    "https://www.googleapis.com/auth/script.external_request",
    // ... otros scopes
  ]
}

// Si falta, agregar y re-deploy

🔴 Apps Script Errors
Error: "Exception: Service invoked too many times"
Síntomas:

Error 429 en logs
Mensaje: "Service invoked too many times for one day"

Causa:
Límite de cuota diaria excedido
Límites:

URLFetch: 20,000 requests/día (Workspace)
MailApp: 1,500 emails/día
Execution time: 6 min/ejecución

Soluciones:
markdown### Solución Inmediata:
1. Esperar 24 horas para reset de cuota
2. Implementar cache para reducir requests
3. Usar batch requests donde sea posible

### Solución Permanente:
```javascript
// Implementar cache con PropertiesService
function getCachedData(key, fetchFunction, ttl = 3600) {
  const cache = CacheService.getScriptCache();
  let data = cache.get(key);
  
  if (data === null) {
    data = fetchFunction();
    cache.put(key, JSON.stringify(data), ttl);
  } else {
    data = JSON.parse(data);
  }
  
  return data;
}

// Uso
const items = getCachedData('items_list', () => {
  return fetchItemsFromBigQuery();
}, 3600);  // Cache por 1 hora

---

### Error: "Exception: Timeout"

**Síntomas**:
- Error después de ~30 segundos
- Mensaje: "Exceeded maximum execution time"

**Causa**:
Script corriendo más de 6 minutos (límite Apps Script)

**Soluciones**:
```javascript
// Opción 1: Procesar en batches
function processLargeDataset(data) {
  const BATCH_SIZE = 100;
  
  for (let i = 0; i < data.length; i += BATCH_SIZE) {
    const batch = data.slice(i, i + BATCH_SIZE);
    processBatch(batch);
    
    // Checkpoint: guardar progreso
    PropertiesService.getScriptProperties()
      .setProperty('last_processed_index', i.toString());
    
    // Si timeout se acerca, detener y continuar después
    if (shouldPause()) {
      ScriptApp.newTrigger('continueProcessing')
        .timeBased()
        .after(1000)
        .create();
      return;
    }
  }
}

function shouldPause() {
  // Simple: revisar tiempo transcurrido
  // Avanzado: usar tiempo de ejecución actual
  return false;  // Implementar lógica
}

// Opción 2: Usar BigQuery para procesamiento pesado
// En lugar de procesar en Apps Script,
// hacer query complejo en BigQuery y solo leer resultado

Error: "ReferenceError: CLib is not defined"
Síntomas:

Error al llamar función de CLib
App no carga

Causa:
Librería no agregada o mal configurada
Solución:
json// 1. Verificar appsscript.json
{
  "dependencies": {
    "libraries": [
      {
        "userSymbol": "CLib",  // ← Debe ser exacto
        "version": "6",
        "libraryId": "1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij",
        "developmentMode": false
      }
    ]
  }
}

// 2. Si está correcto pero sigue fallando:
// - Apps Script Editor → Libraries (sidebar)
// - Verificar que CLib aparece
// - Click en CLib → Ver version
// - Si no aparece: Agregar manualmente con Library ID

Error: "Cannot call method 'getUserDetails' of undefined"
Síntomas:

Error al llamar CLib.getUserDetails()
CLib está agregado pero no funciona

Causa:
CLib tiene error o versión incompatible
Solución:
javascript// 1. Test directo de CLib
function testCLib() {
  try {
    Logger.log('Testing CLib...');
    const result = CLib.getUserDetails();
    Logger.log('Result: ' + JSON.stringify(result));
  } catch (error) {
    Logger.log('ERROR: ' + error.toString());
  }
}

// 2. Ejecutar testCLib() y revisar logs

// 3. Si CLib tiene error interno:
// - Reportar en grupo-angel-apps/issues
// - Usar versión anterior temporalmente:
{
  "version": "5",  // versión anterior estable
}

📊 BigQuery Issues
Error: "Invalid table name"
Síntomas:

Error al insertar a BigQuery
Mensaje: "Table not found: grupo-angel.casino.tickets"

Causa:
Tabla no existe en BigQuery
Solución:
sql-- 1. Verificar si tabla existe
-- Ve a: https://console.cloud.google.com/bigquery

-- 2. Si no existe, crear tabla:
CREATE TABLE `grupo-angel.casino.tickets` (
  ticket_id STRING,
  amount NUMERIC,
  date DATE,
  operator_email STRING,
  status STRING,
  created_at TIMESTAMP
);

-- 3. Grant permissions:
-- IAM → Service Accounts
-- Agregar role: BigQuery Data Editor

Error: "Access Denied: BigQuery"
Síntomas:

Error 403
Mensaje: "Access Denied: BigQuery BigQuery: Permission denied"

Causa:
Service account sin permisos
Solución:
markdown### Paso 1: Identificar service account
1. Apps Script Editor → Project Settings
2. Copiar "GCP Project Number"
3. Service account es:
   [project-number]@[project-id].iam.gserviceaccount.com

### Paso 2: Grant permissions
1. Ve a: https://console.cloud.google.com/iam-admin/iam
2. Click "Grant Access"
3. Principal: [service-account-email]
4. Role: BigQuery Data Editor
5. Save

### Paso 3: Verificar
1. Ejecutar query de test
2. Debe funcionar ahora

Error: "Query exceeded resource limits"
Síntomas:

Query muy lenta o falla
Mensaje: "Resources exceeded during query execution"

Causa:
Query procesando demasiados datos
Solución:
sql-- ❌ MAL: Escanea toda la tabla
SELECT * FROM `grupo-angel.casino.tickets`
WHERE date >= '2025-01-01';

-- ✅ BIEN: Particiona por fecha
SELECT * FROM `grupo-angel.casino.tickets`
WHERE DATE(created_at) >= '2025-01-01'
LIMIT 1000;

-- ✅ MEJOR: Usa partitioning
CREATE TABLE `grupo-angel.casino.tickets`
PARTITION BY DATE(created_at)
AS SELECT * FROM old_table;
javascript// En Apps Script, limita resultados:
function getRecentTickets(limit = 100) {
  const query = `
    SELECT * FROM \`grupo-angel.casino.tickets\`
    WHERE DATE(created_at) >= CURRENT_DATE() - 7
    ORDER BY created_at DESC
    LIMIT ${limit}
  `;
  
  return runQuery(query);
}

🔧 CLib Errors
Error: "CLib function returns {success: false}"
Síntomas:

Función CLib retorna error
result.success === false

Debugging:
javascript// 1. Log detallado del error
const result = CLib.someFunction(params);

if (!result.success) {
  Logger.log('❌ Error en CLib function');
  Logger.log('Function: someFunction');
  Logger.log('Params: ' + JSON.stringify(params));
  Logger.log('Error: ' + result.error);
  
  // 2. Ver stacktrace si está disponible
  if (result.stack) {
    Logger.log('Stack: ' + result.stack);
  }
}

// 3. Ver logs de CLib si están habilitados
// CLib.logDebugMessage() guarda en PropertiesService
const props = PropertiesService.getScriptProperties();
const logs = props.getProperty('DEBUG_LOGS');
if (logs) {
  Logger.log('CLib logs: ' + logs);
}

Error: "insertRowIntoBigQuery timeout"
Síntomas:

Inserción a BigQuery muy lenta
Timeout después de 30s

Causa:
BigQuery streaming insert puede ser lento con objetos grandes
Solución:
javascript// Opción 1: Reducir tamaño del objeto
function insertTicket(ticketData) {
  // ❌ MAL: Objeto muy grande
  const row = {
    ...ticketData,
    image_base64: largeImageString,  // Muy pesado
    raw_ocr: fullOcrResponse  // Muy pesado
  };
  
  // ✅ BIEN: Solo datos necesarios
  const row = {
    ticket_id: ticketData.id,
    amount: ticketData.amount,
    date: ticketData.date,
    // Guardar image en Drive, solo link aquí
    image_url: imageUrl,
    // Summarizar OCR
    ocr_confidence: ticketData.confidence
  };
  
  return CLib.insertRowIntoBigQuery(PROJECT, DATASET, TABLE, row);
}

// Opción 2: Batch inserts
function insertMultipleRows(rows) {
  const BATCH_SIZE = 100;
  
  for (let i = 0; i < rows.length; i += BATCH_SIZE) {
    const batch = rows.slice(i, i + BATCH_SIZE);
    // BigQuery acepta batch nativo
    BigQuery.Tabledata.insertAll({
      rows: batch.map(row => ({json: row}))
    }, PROJECT, DATASET, TABLE);
  }
}

🎨 AngelStyle UI Issues
Issue: "Estilos no cargan / página sin CSS"
Síntomas:

Página carga pero sin estilos
Todo aparece sin formato

Causa:
AngelStyle no cargado o error en getStyles()
Solución:
html<!-- 1. Verificar que está incluido -->
<!DOCTYPE html>
<html>
<head>
  <?!= AngelStyle.getStyles() ?>  <!-- ← Debe estar aquí -->
</head>
<body>
  ...
</body>
</html>

<!-- 2. Test inline: -->
<head>
  <script>
    console.log('Testing AngelStyle...');
    try {
      const styles = AngelStyle.getStyles();
      console.log('Styles loaded: ' + styles.length + ' chars');
    } catch (error) {
      console.error('AngelStyle ERROR: ' + error);
    }
  </script>
  <?!= AngelStyle.getStyles() ?>
</head>

<!-- 3. Si falla, agregar estilos inline temporalmente -->
<style>
  /* Estilos críticos inline como fallback */
  .angel-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }
</style>

Issue: "Botones muy pequeños en mobile"
Síntomas:

Botones difíciles de presionar en mobile
Tamaño < 44px

Causa:
Estilos no responsive
Solución:
css/* En AngelStyle, verificar: */
.angel-button {
  min-height: 44px;  /* ← Mínimo para touch */
  min-width: 44px;
  padding: 12px 24px;
}

/* Si no está, agregar media query: */
@media (max-width: 768px) {
  .angel-button {
    min-height: 48px;  /* Más grande en mobile */
    font-size: 16px;   /* Previene zoom en iOS */
  }
}

Issue: "Modal no cierra / background scroll activo"
Síntomas:

Modal abierto pero fondo scrolleable
Click fuera de modal no cierra

Solución:
javascript// Patrón correcto para modals:

function showModal(modalId) {
  const modal = document.getElementById(modalId);
  modal.classList.remove('hidden');
  
  // Prevenir scroll del body
  document.body.style.overflow = 'hidden';
}

function closeModal(modalId) {
  const modal = document.getElementById(modalId);
  modal.classList.add('hidden');
  
  // Restaurar scroll
  document.body.style.overflow = '';
}

// Click en background cierra modal
document.querySelector('.modal-bg').addEventListener('click', (e) => {
  if (e.target.classList.contains('modal-bg')) {
    closeModal('myModal');
  }
});

⚡ Performance Problems
Issue: "App carga muy lento (>10 segundos)"
Debugging:
javascript// 1. Agregar timing logs
function doGet(e) {
  const startTime = new Date().getTime();
  
  // ... código
  
  const endTime = new Date().getTime();
  Logger.log('doGet() took: ' + (endTime - startTime) + 'ms');
  
  return template.evaluate();
}

// 2. Identificar bottlenecks
function getDashboardData() {
  const t1 = Date.now();
  const user = CLib.getUserDetails();
  Logger.log('getUserDetails: ' + (Date.now() - t1) + 'ms');
  
  const t2 = Date.now();
  const stats = getStats();
  Logger.log('getStats: ' + (Date.now() - t2) + 'ms');
  
  const t3 = Date.now();
  const movements = getMovements();
  Logger.log('getMovements: ' + (Date.now() - t3) + 'ms');
}
Optimizaciones comunes:
javascript// ❌ MAL: Múltiples queries separados
function getDashboard() {
  const stats = queryBigQuery('SELECT COUNT(*) FROM items');
  const lowStock = queryBigQuery('SELECT COUNT(*) FROM items WHERE stock < 10');
  const pending = queryBigQuery('SELECT COUNT(*) FROM movements WHERE status = "pending"');
}

// ✅ BIEN: Un query con subqueries
function getDashboard() {
  const query = `
    SELECT 
      (SELECT COUNT(*) FROM items) as total_items,
      (SELECT COUNT(*) FROM items WHERE stock < 10) as low_stock,
      (SELECT COUNT(*) FROM movements WHERE status = "pending") as pending
  `;
  return queryBigQuery(query);
}

// ✅ MEJOR: Cache resultados
function getDashboard() {
  return getCachedData('dashboard_stats', () => {
    return queryBigQuery(query);
  }, 300);  // Cache 5 minutos
}

Issue: "Gemini API muy lento"
Síntomas:

OCR toma >30 segundos
Timeout frecuentes

Optimizaciones:
javascript// 1. Reducir tamaño de imagen antes de enviar
function compressImage(base64Image) {
  // Usar canvas para resize
  const maxWidth = 1024;
  const maxHeight = 1024;
  
  // ... código de compression
  
  return compressedBase64;
}

// 2. Usar modelo más rápido si aplicable
const result = CLib.callGenerativeApi(prompt, image, {
  model: 'gemini-1.5-flash',  // Más rápido que pro
  temperature: 0.1,
  maxOutputTokens: 512  // Menos tokens = más rápido
});

// 3. Timeout custom y retry logic
async function analyzeWithRetry(image, maxRetries = 2) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const result = await CLib.callGenerativeApi(prompt, image);
      if (result.success) return result;
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      Utilities.sleep(1000 * (i + 1));  // Exponential backoff
    }
  }
}

🚀 Deployment Issues
Issue: "Código local diferente a online"
Síntomas:

Cambios locales no reflejan en Apps Script
Confusión sobre qué versión está deployed

Solución:
bash# 1. Verificar qué hay en cada lugar

# Local
cd apps/InventoryManager
git log --oneline -1
# Output: abc1234 feat(inventory): nueva feature

# Online (pull para ver)
clasp pull
git diff

# 2. Decidir source of truth

# Si LOCAL es correcto:
clasp push  # Subir local → online

# Si ONLINE es correcto:
clasp pull  # Bajar online → local
git add .
git commit -m "sync: pull from Apps Script"

# 3. Establecer workflow claro:
# SIEMPRE: local → git → online → deploy
# NUNCA: editar directamente en online sin sync

Issue: "Deploy no actualiza la app"
Síntomas:

Hiciste deploy pero cambios no aparecen
Usuarios ven versión antigua

Causas y Soluciones:
markdown### Causa 1: Cache del navegador
**Solución**:
- Hard refresh: Ctrl+Shift+R (Windows) o Cmd+Shift+R (Mac)
- Incognito window

### Causa 2: Deployment incorrecto
**Verificar**:
1. Apps Script → Deploy → Manage deployments
2. Ver "Active deployments"
3. Verificar que el deployment activo es el más reciente

### Causa 3: URL incorrecta
**Verificar**:
1. URL debe ser del deployment activo
2. NO usar URL de "Test deployments"
3. Verificar en README.md que URL es correcta

### Causa 4: Propagación de Google
**Solución**:
- A veces Google toma 5-10 minutos en propagar
- Esperar y volver a probar

💻 Git/GitHub Issues
Error: "fatal: not a git repository"
Síntomas:

Git commands fallan
Mensaje: "fatal: not a git repository"

Solución:
bash# 1. Verificar que estás en carpeta correcta
pwd
# Debe mostrar: .../grupo-angel-apps

# 2. Si no estás en carpeta correcta:
cd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"

# 3. Verificar que es repo git:
ls -la | grep .git
# Debe mostrar: .git/

# 4. Si no hay .git/, inicializar:
git init
git remote add origin https://github.com/ChristianLuciani/grupo-angel-apps.git
git fetch
git checkout main

Error: "Your branch is behind origin/main"
Síntomas:

Git dice que estás atrás de origin
Push rechazado

Solución:
bash# 1. Ver diferencia
git status
# Output: Your branch is behind 'origin/main' by 3 commits

# 2. Opción A: Pull (recomendado)
git pull origin main

# 3. Si hay conflictos:
# - Resolver manualmente (ver Git Workflow guide)
# - O usar GitHub Desktop (más visual)

# 4. Después de resolver:
git add .
git commit -m "chore: merge main"
git push origin main

Error: "Permission denied (publickey)"
Síntomas:

Git push/pull falla
Mensaje: "Permission denied (publickey)"

Solución:
bash# 1. Verificar SSH key
ls -la ~/.ssh/
# Buscar: id_rsa, id_ed25519, o similar

# 2. Si no existe, crear:
ssh-keygen -t ed25519 -C "cluciani@gmail.com"
# Press Enter 3 veces (default location, no passphrase)

# 3. Agregar a GitHub:
cat ~/.ssh/id_ed25519.pub
# Copiar output completo

# 4. GitHub → Settings → SSH Keys → New SSH key
# Paste key y guardar

# 5. Test:
ssh -T git@github.com
# Debe decir: "Hi ChristianLuciani! You've successfully authenticated"

# 6. Si sigue fallando, cambiar URL a HTTPS:
git remote set-url origin https://github.com/ChristianLuciani/grupo-angel-apps.git

🆘 Escalation Process
Cuando Escalar
Escalar si:

⏰ Has intentado troubleshooting por >30 minutos
🔴 Es problema crítico (producción caída)
❓ No encuentras solución en docs
🆕 Es problema nuevo no documentado


Escalation Levels
markdown### Level 1: Self-Service (0-30 min)
- Buscar en esta guía
- Buscar en documentación del proyecto
- Google el error específico
- Revisar logs detalladamente

### Level 2: Team Help (30-60 min)
- Slack/Chat interno: Canal #tech-support
- Incluir:
  - Error exacto (screenshot o copy-paste)
  - Pasos para reproducir
  - Qué ya intentaste
  - Urgencia (critical/high/medium/low)

### Level 3: External Support (>60 min)
- Apps Script Support: https://support.google.com/code/
- BigQuery Support: https://cloud.google.com/support
- Stack Overflow (tag: google-apps-script)

### Level 4: Vendor Escalation (Critical)
- Google Cloud Support (si tienes plan)
- Contact: [support email/phone]

Reportar Bug Template
markdown## 🐛 Bug Report

### Descripción
[DescripciónRetryClaude does not have the ability to run the code it generates yet.CLContinueclara del problema]
Ambiente

App: [InventoryManager / TicketProcessor / etc.]
Versión: [v1.2.0]
Browser: [Chrome 120 / Firefox 115 / etc.]
Device: [Desktop / Mobile / Tablet]
OS: [macOS / Windows / iOS / Android]

Pasos para Reproducir

[Paso 1]
[Paso 2]
[Paso 3]

Comportamiento Esperado
[Qué debería pasar]
Comportamiento Actual
[Qué está pasando]
Screenshots/Logs
[Agregar screenshots o copy-paste de errores]
Console Errors (F12)
[Copy-paste de errores en console]
Apps Script Logs
[Copy-paste de Logger.log() o Executions]
Ya Intenté

 [Solución 1]
 [Solución 2]

Severidad

 Critical (producción caída)
 High (funcionalidad importante no funciona)
 Medium (workaround posible)
 Low (cosmético o edge case)

Workaround Actual
[Si existe workaround temporal, describirlo]

---

## 📝 Documentar Nuevos Problemas

### Cuando Encuentres Problema Nuevo

**Si lo resuelves**:

1. **Documenta aquí**:
```markdown
   ### Error: "[Título descriptivo]"
   
   **Síntomas**:
   - [Síntoma 1]
   - [Síntoma 2]
   
   **Causa**:
   [Qué lo causa]
   
   **Solución**:
   \```bash
   # Comandos o código
   \```
   
   **Prevención**:
   [Cómo evitarlo en futuro]

Commit a docs:

bash   git add human/guides/troubleshooting.md
   git commit -m "docs(troubleshooting): agregar solución para [error]"
   git push origin main

Notifica al equipo (Slack/Chat)


Si NO lo resuelves:

Crear issue en GitHub:

bash   open https://github.com/ChristianLuciani/grupo-angel-apps/issues/new

Usar Bug Report template (arriba)
Tag apropiado: bug, help wanted, question
Escalar según Escalation Process


🔍 Debugging Tips
Apps Script Debugging
1. Logger.log() es tu amigo
javascriptfunction debugFunction(data) {
  Logger.log('=== START debugFunction ===');
  Logger.log('Input data: ' + JSON.stringify(data, null, 2));
  
  try {
    const result = processData(data);
    Logger.log('Process result: ' + JSON.stringify(result, null, 2));
    return result;
  } catch (error) {
    Logger.log('ERROR: ' + error.toString());
    Logger.log('Stack: ' + error.stack);
    throw error;
  } finally {
    Logger.log('=== END debugFunction ===');
  }
}

// Ver logs: Apps Script Editor → Executions (sidebar)

2. Breakpoint Debugging
javascriptfunction doGet(e) {
  debugger;  // ← Breakpoint (solo funciona en online editor)
  
  const user = CLib.getUserDetails();
  debugger;  // Otro breakpoint
  
  return renderView(user);
}

// En Apps Script Editor:
// 1. Agregar debugger;
// 2. Run → Debug
// 3. Step through con buttons

3. Execution Logs
markdown### Ver Executions:
1. Apps Script Editor → Executions (⚡ icon)
2. Ver todas las ejecuciones recientes
3. Click en ejecución → Ver logs completos
4. Filtrar por:
   - Status: Success / Error
   - Date range
   - Function name

### Útil para:
- Ver qué errores están pasando en producción
- Identificar funciones lentas
- Ver frecuencia de ejecuciones

4. Console en Frontend
javascript// En HTML files:
<script>
  console.log('Testing frontend...');
  
  google.script.run
    .withSuccessHandler((result) => {
      console.log('✅ Success:', result);
    })
    .withFailureHandler((error) => {
      console.error('❌ Error:', error);
    })
    .myFunction();
</script>

// Ver en browser:
// F12 → Console tab

BigQuery Debugging
1. Test Queries Directamente
sql-- En BigQuery Console, test queries:
-- https://console.cloud.google.com/bigquery

-- Add LIMIT para testing rápido
SELECT * FROM `grupo-angel.casino.tickets`
LIMIT 10;

-- Check schema
SELECT column_name, data_type
FROM `grupo-angel.casino`.INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'tickets';

-- Validate data
SELECT 
  COUNT(*) as total_rows,
  COUNT(DISTINCT ticket_id) as unique_tickets,
  MIN(created_at) as oldest,
  MAX(created_at) as newest
FROM `grupo-angel.casino.tickets`;

2. DRY_RUN para estimar costos
javascript// En Apps Script, usar dry run:
function testQuery() {
  const query = 'SELECT * FROM `grupo-angel.casino.tickets` WHERE date >= "2025-01-01"';
  
  const request = {
    query: query,
    dryRun: true,  // No ejecuta, solo estima
    useLegacySql: false
  };
  
  const queryResults = BigQuery.Jobs.query(request, 'grupo-angel');
  Logger.log('Bytes processed: ' + queryResults.totalBytesProcessed);
  Logger.log('Estimated cost: $' + (queryResults.totalBytesProcessed / 1000000000000 * 5));
}

Performance Profiling
javascript// Clase simple de profiler
class Profiler {
  constructor() {
    this.timings = {};
  }
  
  start(label) {
    this.timings[label] = Date.now();
  }
  
  end(label) {
    if (this.timings[label]) {
      const duration = Date.now() - this.timings[label];
      Logger.log(`⏱️ ${label}: ${duration}ms`);
      return duration;
    }
  }
  
  report() {
    Logger.log('=== PERFORMANCE REPORT ===');
    for (const [label, startTime] of Object.entries(this.timings)) {
      Logger.log(`${label}: ${startTime}ms`);
    }
  }
}

// Uso:
function getDashboard() {
  const profiler = new Profiler();
  
  profiler.start('getUserDetails');
  const user = CLib.getUserDetails();
  profiler.end('getUserDetails');
  
  profiler.start('getStats');
  const stats = getStats();
  profiler.end('getStats');
  
  profiler.start('getMovements');
  const movements = getMovements();
  profiler.end('getMovements');
  
  profiler.report();
  
  return {user, stats, movements};
}

🧪 Testing Strategies
Unit Testing (Manual)
javascript// Test individual functions
function testGetUserDetails() {
  Logger.log('=== TEST: getUserDetails ===');
  
  const result = CLib.getUserDetails();
  
  // Assertions
  if (!result.success) {
    Logger.log('❌ FAIL: success should be true');
    return false;
  }
  
  if (!result.data.email) {
    Logger.log('❌ FAIL: email should exist');
    return false;
  }
  
  Logger.log('✅ PASS: getUserDetails works correctly');
  return true;
}

// Run all tests
function runAllTests() {
  const tests = [
    testGetUserDetails,
    testGetUserRole,
    testRegisterMovement
    // ... más tests
  ];
  
  let passed = 0;
  let failed = 0;
  
  tests.forEach(test => {
    if (test()) {
      passed++;
    } else {
      failed++;
    }
  });
  
  Logger.log(`\n=== RESULTS ===`);
  Logger.log(`✅ Passed: ${passed}`);
  Logger.log(`❌ Failed: ${failed}`);
}

Integration Testing
javascript// Test flujo completo
function testCompleteFlow() {
  Logger.log('=== INTEGRATION TEST: Complete Movement Flow ===');
  
  try {
    // 1. Auth
    const user = CLib.getUserDetails();
    if (!user.success) throw new Error('Auth failed');
    Logger.log('✅ Step 1: Auth OK');
    
    // 2. Create movement
    const movement = {
      type: 'entrada',
      itemId: 'TEST-001',
      quantity: 10,
      locationTo: 'ALMACEN-TEST'
    };
    
    const createResult = registerMovement(movement);
    if (!createResult.success) throw new Error('Create failed: ' + createResult.error);
    Logger.log('✅ Step 2: Create movement OK');
    
    // 3. Verify in BigQuery
    const query = `SELECT * FROM \`grupo-angel.inventory.movements\` WHERE movement_id = '${createResult.data.movementId}'`;
    const verifyResult = queryBigQuery(query);
    if (verifyResult.length === 0) throw new Error('Movement not found in BigQuery');
    Logger.log('✅ Step 3: Verify in BigQuery OK');
    
    // 4. Cleanup
    deleteTestMovement(createResult.data.movementId);
    Logger.log('✅ Step 4: Cleanup OK');
    
    Logger.log('\n✅ INTEGRATION TEST PASSED');
    return true;
    
  } catch (error) {
    Logger.log('\n❌ INTEGRATION TEST FAILED');
    Logger.log('Error: ' + error.toString());
    return false;
  }
}

🔧 Mantenimiento de Esta Documentación
Template para Nuevo Problema
markdown### Error: "[Título Descriptivo]"

**Síntomas**:
- [Qué ve el usuario]
- [Mensaje de error exacto]

**Causa**:
[Por qué pasa esto]

**Solución**:

\```[javascript|bash|sql]
// Código o comandos paso a paso
\```

**Prevención**:
[Cómo evitar este problema en futuro]

**Related Issues**:
- [Link a issue de GitHub si existe]
- [Link a discusión en Slack]

**Added**: [Fecha] por [Nombre]
**Last Updated**: [Fecha]

Actualización Regular
Cada mes:

Revisar issues cerrados en GitHub
Agregar problemas comunes que aparecieron
Actualizar soluciones que cambiaron
Eliminar problemas obsoletos (con nota)

Después de cada deployment problemático:

Documentar qué falló
Documentar cómo se resolvió
Agregar a sección apropiada


Métricas de Troubleshooting
markdown## Troubleshooting Metrics (Mensual)

### Top 5 Problemas Este Mes
1. [Problema] - [Veces reportado]
2. [Problema] - [Veces reportado]
3. [Problema] - [Veces reportado]

### Tiempo Promedio de Resolución
- Level 1 (self-service): [X] minutos
- Level 2 (team help): [X] minutos
- Level 3 (external): [X] horas

### Mejoras Necesarias
- [ ] [Área que necesita mejor documentación]
- [ ] [Tool que ayudaría]
- [ ] [Training necesario]

📞 Emergency Contacts
markdown## Emergency Contacts

### Development Team
- **Primary Developer**: Christian Luciani
  - Email: cluciani@gmail.com
  - Slack: @christian
  - Phone: [número] (solo emergencias)

### Infrastructure
- **GCP Admin**: [Nombre]
  - Email: [email]
  - Available: 24/7

### Business
- **Product Owner**: [Nombre]
  - Email: [email]
  - Available: Business hours

### Escalation Chain
1. Developer (0-30 min response)
2. GCP Admin (30-60 min response)
3. Product Owner (1-4 hour response)

### After Hours
- Critical issues: Call developer directly
- Non-critical: Slack message, respond next business day

🎓 Learning Resources
Apps Script

Official Documentation
Best Practices
Troubleshooting Guide

BigQuery

Query Syntax
Best Practices
Troubleshooting

Git

Git Basics
GitHub Docs
Oh Shit, Git! (fixing mistakes)

Internal Docs

📖 System Overview
📖 Git Workflow
📖 Deployment Guide
📖 CLib API


🆘 Still Stuck?
Si después de revisar esta guía todavía tienes problemas:
Checklist Final

 Leí sección relevante de troubleshooting
 Busqué el error en Google
 Revisé logs detalladamente
 Intenté reproducir en TEST
 Documenté pasos exactos para reproducir
 Tomé screenshots de errores

Crear Issue
bash# Create detailed issue in GitHub
open https://github.com/ChristianLuciani/grupo-angel-apps/issues/new

# Include:
# - Bug Report template filled
# - Screenshots/logs
# - Steps to reproduce
# - What you already tried
Contactar Team
markdown**Slack Message Template**:

🆘 **Need Help**: [Brief description]

**App**: [InventoryManager]
**Error**: [Error message]
**Impact**: [Critical/High/Medium/Low]
**Already tried**: [List what you tried]

**Full details**: [Link to GitHub issue]

Remember:

🧠 Every problem solved makes you better
📝 Always document solutions for next time
🤝 Don't hesitate to ask for help
💡 Your struggle might help others later


🔗 Links Relacionados

📖 Local Setup
📖 Creating New App
📖 Git Workflow
📖 Deployment Guide
📖 System Overview
📖 CLib API
📖 AngelStyle API


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

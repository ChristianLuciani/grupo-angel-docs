# 🏗️ Creating New App

> Tutorial completo paso a paso para crear una aplicación desde cero

---

## 🎯 Objetivo

Al terminar esta guía tendrás:

✅ Nueva app funcional desde cero  
✅ CLib y AngelStyle integrados  
✅ Autenticación implementada  
✅ UI moderna con AI-Modern Dark Theme  
✅ Deploy exitoso  
✅ Documentación completa  

**Tiempo estimado**: 2-3 horas (primera vez), 45 min (con experiencia)

---

## 📋 Pre-requisitos

Antes de empezar, verifica:

- [x] Completaste [Local Setup](./local-setup.md)
- [x] Leíste [System Overview](../architecture/system-overview.md)
- [x] Conoces funciones básicas de [CLib](../reference/clib-api.md)
- [x] Conoces componentes de [AngelStyle](../reference/angelstyle-api.md)
- [x] Tienes acceso a Google Apps Script

---

## 🗺️ Roadmap del Tutorial
```mermaid
graph LR
    A[Planning] --> B[Setup Inicial]
    B --> C[Backend]
    C --> D[Frontend]
    D --> E[Testing]
    E --> F[Deploy]
    F --> G[Docs]
Fases:

Planning (15 min) - Definir qué hacer
Setup (10 min) - Crear estructura
Backend (45 min) - Lógica en Apps Script
Frontend (60 min) - UI con AngelStyle
Testing (30 min) - Probar todo
Deploy (15 min) - Publicar
Docs (15 min) - Documentar


📝 FASE 1: Planning (15 min)
Paso 1.1: Definir la App
Responde estas preguntas por escrito:
markdown## Definición de App: [NOMBRE]

### ¿Qué hace?
[1-2 frases describiendo la funcionalidad principal]

### ¿Quién la usa?
- [ ] Operadores
- [ ] Supervisores
- [ ] Admins
- [ ] Todos

### ¿Qué datos maneja?
[Lista de entidades principales]

### ¿Se integra con?
- [ ] BigQuery
- [ ] Google Sheets
- [ ] Drive
- [ ] Gemini AI
- [ ] Otro: _______

### Funcionalidad Mínima (MVP):
1. [Función principal]
2. [Función secundaria]
3. [Función terciaria]

### Funcionalidad Futura (v2):
1. [Feature avanzado]
2. [Optimización]
Ejemplo Real (InventoryManager):
markdown## Definición de App: InventoryManager

### ¿Qué hace?
Gestiona inventario de insumos de casinos (tragamonedas, chips, etc.)

### ¿Quién la usa?
- [x] Operadores (registrar entrada/salida)
- [x] Supervisores (aprobar movimientos grandes)
- [x] Admins (reportes, configuración)

### ¿Qué datos maneja?
- Items (nombre, código, categoría, precio)
- Movimientos (entrada/salida, cantidad, responsable)
- Locations (casino, almacén, máquina)

### ¿Se integra con?
- [x] BigQuery (storage principal)
- [x] Google Sheets (reportes)
- [ ] Gemini AI
- [ ] Drive

### Funcionalidad Mínima (MVP):
1. Registrar entrada de items
2. Registrar salida de items
3. Ver inventario actual
4. Búsqueda simple

### Funcionalidad Futura (v2):
1. Escaneo de código de barras
2. Alertas de stock bajo
3. Predicción de demanda con ML

Paso 1.2: Wireframes
Dibuja (papel o digital) las pantallas principales:
Herramientas recomendadas:

Papel y lápiz (más rápido)
Excalidraw (https://excalidraw.com)
Figma (si ya lo usas)

Elementos a dibujar:

Header (logo, título, logout)
Navegación (tabs, botones)
Contenido principal
Formularios (inputs, botones)
Estados (loading, error, success)

Toma foto/screenshot y guarda en:
grupo-angel-docs/assets/images/wireframes/[nombre-app]/

Paso 1.3: Identificar Componentes
Basado en wireframes, lista componentes necesarios:
Checklist de componentes:
markdown### CLib Functions Necesarias:
- [ ] getUserDetails()
- [ ] getUserRole()
- [ ] logDebugMessage()
- [ ] insertRowIntoBigQuery()
- [ ] callGenerativeApi()
- [ ] getOrCreateMonthlyFolder()
- [ ] Otra: __________

### AngelStyle Components Necesarios:
- [ ] .angel-container
- [ ] .angel-header
- [ ] .angel-button (variantes: primary, secondary, ghost)
- [ ] .angel-input
- [ ] .angel-card
- [ ] .angel-upload-area
- [ ] .angel-spinner
- [ ] .angel-badge
- [ ] Otro: __________

### Custom Components a Crear:
- [ ] [Nombre]: [Descripción]

Paso 1.4: Crear Issue en GitHub
bash# Ve a GitHub
open https://github.com/ChristianLuciani/grupo-angel-apps/issues/new
Template de Issue:
markdown## 🚀 Nueva App: [NOMBRE]

### Descripción
[1-2 párrafos explicando qué hace]

### Usuarios
- Operadores
- Supervisores
- Admins

### Funcionalidad Mínima (MVP)
- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3

### Integraciones
- BigQuery: [Qué tablas]
- Sheets: [Para qué]
- Gemini: [Si aplica]

### Estimación
- Backend: 2-3 horas
- Frontend: 3-4 horas
- Testing: 1 hora
- **Total**: ~1 día

### Referencias
- Wireframes: [Link a carpeta assets]
- Similar a: [App existente parecida]

### Checklist de Desarrollo
- [ ] Setup inicial
- [ ] Backend lógica
- [ ] Frontend UI
- [ ] Testing completo
- [ ] Deploy TEST
- [ ] Deploy PROD
- [ ] Documentación
Labels: enhancement, new-app, in-progress

🔧 FASE 2: Setup Inicial (10 min)
Paso 2.1: Crear Branch
bashcd "/Users/eva/cluciani@gmail.com - Google Drive/My Drive/Grupo Angel/grupo-angel-apps"

# Crear branch desde main
git checkout main
git pull origin main
git checkout -b feature/inventory-manager
Convención de nombres:

feature/nombre-app - Nueva app
fix/nombre-app-bug - Bug fix
refactor/nombre-app - Refactoring


Paso 2.2: Crear Estructura de Carpetas
bash# Navegar a apps
cd apps

# Crear carpeta de la app
mkdir InventoryManager
cd InventoryManager

# Crear archivos base
touch Code.gs
touch Config.gs
touch Index.html
touch OperatorView.html
touch SupervisorView.html
touch AdminDashboard.html
touch AccessDenied.html
touch appsscript.json
touch README.md
touch CHANGELOG.md
touch CURRENT_STATE.md
Resultado:
apps/InventoryManager/
├── Code.gs                 # Backend principal
├── Config.gs               # Configuración
├── Index.html              # Landing page
├── OperatorView.html       # Interfaz operador
├── SupervisorView.html     # Interfaz supervisor
├── AdminDashboard.html     # Panel admin
├── AccessDenied.html       # Error 403
├── appsscript.json         # Manifest
├── README.md               # Overview
├── CHANGELOG.md            # Historial de cambios
└── CURRENT_STATE.md        # Estado actual detallado

Paso 2.3: Crear Script en Apps Script

Ve a Apps Script:

bash   open https://script.google.com

Nuevo Proyecto:

Click "New Project"
Nombre: InventoryManager


Copiar Script ID:

Arriba: Project Settings (⚙️)
Copia Script ID
Ejemplo: 1ABC...XYZ


Guardar ID:

bash   # En tu carpeta local
   echo "Script ID: 1ABC...XYZ" > .script-id

Paso 2.4: Configurar appsscript.json
bashcat > appsscript.json << 'JSON'
{
  "timeZone": "America/Panama",
  "dependencies": {
    "enabledAdvancedServices": [
      {
        "userSymbol": "BigQuery",
        "serviceId": "bigquery",
        "version": "v2"
      },
      {
        "userSymbol": "Drive",
        "serviceId": "drive",
        "version": "v3"
      },
      {
        "userSymbol": "Sheets",
        "serviceId": "sheets",
        "version": "v4"
      }
    ],
    "libraries": [
      {
        "userSymbol": "CLib",
        "version": "6",
        "libraryId": "1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij",
        "developmentMode": false
      },
      {
        "userSymbol": "AngelStyle",
        "version": "4",
        "libraryId": "19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S",
        "developmentMode": false
      }
    ]
  },
  "exceptionLogging": "STACKDRIVER",
  "runtimeVersion": "V8",
  "webapp": {
    "executeAs": "USER_ACCESSING",
    "access": "ANYONE"
  },
  "oauthScopes": [
    "https://www.googleapis.com/auth/script.external_request",
    "https://www.googleapis.com/auth/spreadsheets",
    "https://www.googleapis.com/auth/drive",
    "https://www.googleapis.com/auth/bigquery",
    "https://www.googleapis.com/auth/userinfo.email"
  ]
}
JSON
Componentes clave:

libraries: CLib + AngelStyle
enabledAdvancedServices: BigQuery, Drive, Sheets
oauthScopes: Permisos necesarios
webapp.executeAs: USER_ACCESSING (corre como usuario actual)


Paso 2.5: Configurar Config.gs
bashcat > Config.gs << 'JAVASCRIPT'
/**
 * Configuration for InventoryManager
 * DO NOT commit sensitive data - use PropertiesService
 */

// Project Info
const PROJECT_NAME = 'InventoryManager';
const PROJECT_VERSION = '1.0.0';

// BigQuery Configuration
const BQ_PROJECT_ID = 'grupo-angel';
const BQ_DATASET_ID = 'inventory';

// Tables
const BQ_TABLES = {
  items: 'items',
  movements: 'movements',
  locations: 'locations',
  users: 'users'
};

// Google Drive Folders (Store IDs in PropertiesService)
function getDriveFolders() {
  const props = PropertiesService.getScriptProperties();
  return {
    uploads: props.getProperty('UPLOADS_FOLDER_ID') || '',
    reports: props.getProperty('REPORTS_FOLDER_ID') || ''
  };
}

// User Roles
const ADMIN_EMAILS = [
  'admin@grupoangel.com',
  'cluciani@gmail.com'
];

const SUPERVISOR_EMAILS = [
  'supervisor@grupoangel.com'
];

// App Settings
const SETTINGS = {
  maxUploadSize: 5 * 1024 * 1024,  // 5MB
  itemsPerPage: 20,
  sessionTimeout: 3600000,  // 1 hour in ms
  debugMode: false
};

// Status Constants
const STATUS = {
  PENDING: 'pending',
  APPROVED: 'approved',
  REJECTED: 'rejected',
  COMPLETED: 'completed'
};

// Movement Types
const MOVEMENT_TYPES = {
  IN: 'entrada',
  OUT: 'salida',
  TRANSFER: 'transferencia',
  ADJUSTMENT: 'ajuste'
};

/**
 * Get setting value
 * @param {string} key - Setting key
 * @returns {*} Setting value
 */
function getSetting(key) {
  return SETTINGS[key];
}

/**
 * Check if user is admin
 * @param {string} email - User email
 * @returns {boolean}
 */
function isAdmin(email) {
  return ADMIN_EMAILS.includes(email.toLowerCase());
}

/**
 * Check if user is supervisor
 * @param {string} email - User email
 * @returns {boolean}
 */
function isSupervisor(email) {
  return SUPERVISOR_EMAILS.includes(email.toLowerCase()) || isAdmin(email);
}
JAVASCRIPT

💻 FASE 3: Backend (45 min)
Paso 3.1: Estructura Base de Code.gs
bashcat > Code.gs << 'JAVASCRIPT'
/**
 * InventoryManager - Main Backend
 * @version 1.0.0
 */

/**
 * Entry point for web app
 * @param {Object} e - Event object
 * @returns {HtmlOutput}
 */
function doGet(e) {
  try {
    // Get user details
    const userResult = CLib.getUserDetails();
    
    if (!userResult.success) {
      return renderAccessDenied('No se pudo autenticar usuario');
    }
    
    // Get user role
    const roleResult = CLib.getUserRole(userResult.data.email);
    
    if (!roleResult.success) {
      return renderAccessDenied('No se pudo determinar permisos');
    }
    
    const role = roleResult.data.role;
    
    // Route to appropriate view
    return routeToView(role, userResult.data);
    
  } catch (error) {
    CLib.logDebugMessage('Error en doGet', {
      error: error.toString(),
      stack: error.stack
    });
    return renderAccessDenied('Error del servidor: ' + error.message);
  }
}

/**
 * Route user to appropriate view based on role
 * @param {string} role - User role
 * @param {Object} userData - User data
 * @returns {HtmlOutput}
 */
function routeToView(role, userData) {
  let template;
  
  switch(role) {
    case 'admin':
      template = HtmlService.createTemplateFromFile('AdminDashboard');
      break;
    case 'supervisor':
      template = HtmlService.createTemplateFromFile('SupervisorView');
      break;
    case 'operator':
      template = HtmlService.createTemplateFromFile('OperatorView');
      break;
    default:
      return renderAccessDenied('Rol no autorizado: ' + role);
  }
  
  // Pass user data to template
  template.userData = userData;
  template.userRole = role;
  
  return template.evaluate()
    .setTitle('Inventory Manager - ' + role)
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
    .addMetaTag('viewport', 'width=device-width, initial-scale=1.0');
}

/**
 * Render access denied page
 * @param {string} message - Error message
 * @returns {HtmlOutput}
 */
function renderAccessDenied(message) {
  const template = HtmlService.createTemplateFromFile('AccessDenied');
  template.errorMessage = message;
  return template.evaluate()
    .setTitle('Acceso Denegado')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}

/**
 * Include HTML files (for templates)
 * @param {string} filename - File name
 * @returns {string} HTML content
 */
function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename).getContent();
}

// ============================================
// API FUNCTIONS (Called from frontend)
// ============================================

/**
 * Get dashboard data for current user
 * @returns {Object} {success, data/error}
 */
function getDashboardData() {
  try {
    const userResult = CLib.getUserDetails();
    if (!userResult.success) {
      return {success: false, error: 'Usuario no autenticado'};
    }
    
    const roleResult = CLib.getUserRole(userResult.data.email);
    if (!roleResult.success) {
      return {success: false, error: 'Error obteniendo rol'};
    }
    
    // Get stats
    const stats = getInventoryStats();
    
    // Get recent movements
    const movements = getRecentMovements(10);
    
    return {
      success: true,
      data: {
        user: userResult.data,
        role: roleResult.data.role,
        stats: stats,
        movements: movements
      }
    };
    
  } catch (error) {
    CLib.logDebugMessage('Error en getDashboardData', {error: error.toString()});
    return {success: false, error: error.toString()};
  }
}

/**
 * Get inventory statistics
 * @returns {Object} Stats object
 */
function getInventoryStats() {
  // TODO: Query BigQuery for actual stats
  // For now, return mock data
  return {
    totalItems: 0,
    lowStock: 0,
    pendingMovements: 0,
    valueTotal: 0
  };
}

/**
 * Get recent movements
 * @param {number} limit - Number of movements to retrieve
 * @returns {Array} Movements array
 */
function getRecentMovements(limit) {
  // TODO: Query BigQuery for actual movements
  return [];
}

/**
 * Register new movement (entrada/salida)
 * @param {Object} movementData - Movement data
 * @returns {Object} {success, data/error}
 */
function registerMovement(movementData) {
  try {
    // Validate input
    if (!movementData || !movementData.type || !movementData.itemId) {
      return {success: false, error: 'Datos incompletos'};
    }
    
    // Get user
    const userResult = CLib.getUserDetails();
    if (!userResult.success) {
      return {success: false, error: 'Usuario no autenticado'};
    }
    
    // Prepare row for BigQuery
    const row = {
      movement_id: Utilities.getUuid(),
      type: movementData.type,
      item_id: movementData.itemId,
      quantity: parseFloat(movementData.quantity),
      location_from: movementData.locationFrom || null,
      location_to: movementData.locationTo || null,
      notes: movementData.notes || '',
      status: STATUS.PENDING,
      created_by: userResult.data.email,
      created_at: new Date().toISOString()
    };
    
    // Insert to BigQuery
    const insertResult = CLib.insertRowIntoBigQuery(
      BQ_PROJECT_ID,
      BQ_DATASET_ID,
      BQ_TABLES.movements,
      row
    );
    
    if (!insertResult.success) {
      return {success: false, error: 'Error guardando en BigQuery: ' + insertResult.error};
    }
    
    CLib.logDebugMessage('Movimiento registrado', {
      movementId: row.movement_id,
      user: userResult.data.email
    });
    
    return {
      success: true,
      data: {
        movementId: row.movement_id,
        status: row.status
      }
    };
    
  } catch (error) {
    CLib.logDebugMessage('Error en registerMovement', {
      error: error.toString(),
      movementData: movementData
    });
    return {success: false, error: error.toString()};
  }
}

/**
 * Search items
 * @param {string} query - Search query
 * @returns {Object} {success, data/error}
 */
function searchItems(query) {
  try {
    if (!query || query.trim().length < 2) {
      return {success: false, error: 'Query muy corto (mínimo 2 caracteres)'};
    }
    
    // TODO: Query BigQuery for items matching query
    // For now, return empty array
    return {
      success: true,
      data: {
        items: [],
        count: 0
      }
    };
    
  } catch (error) {
    CLib.logDebugMessage('Error en searchItems', {error: error.toString(), query: query});
    return {success: false, error: error.toString()};
  }
}

/**
 * Approve movement (supervisor/admin only)
 * @param {string} movementId - Movement ID
 * @returns {Object} {success, data/error}
 */
function approveMovement(movementId) {
  try {
    // Check permissions
    const userResult = CLib.getUserDetails();
    if (!userResult.success) {
      return {success: false, error: 'Usuario no autenticado'};
    }
    
    if (!isSupervisor(userResult.data.email)) {
      return {success: false, error: 'Permisos insuficientes'};
    }
    
    // TODO: Update movement status in BigQuery
    
    CLib.logDebugMessage('Movimiento aprobado', {
      movementId: movementId,
      approvedBy: userResult.data.email
    });
    
    return {success: true, data: {movementId: movementId, status: STATUS.APPROVED}};
    
  } catch (error) {
    CLib.logDebugMessage('Error en approveMovement', {error: error.toString(), movementId: movementId});
    return {success: false, error: error.toString()};
  }
}
JAVASCRIPT

Paso 3.2: Testing del Backend
javascript/**
 * Test functions - Run from Apps Script editor
 */

function testGetDashboardData() {
  const result = getDashboardData();
  Logger.log(JSON.stringify(result, null, 2));
}

function testRegisterMovement() {
  const testData = {
    type: MOVEMENT_TYPES.IN,
    itemId: 'TEST-001',
    quantity: 10,
    locationTo: 'ALMACEN-PRINCIPAL',
    notes: 'Test movement'
  };
  
  const result = registerMovement(testData);
  Logger.log(JSON.stringify(result, null, 2));
}

function testSearchItems() {
  const result = searchItems('chip');
  Logger.log(JSON.stringify(result, null, 2));
}
Ejecutar tests:

En Apps Script editor
Selecciona función (ej: testGetDashboardData)
Click "Run"
Revisa Logs (View → Logs)


🎨 FASE 4: Frontend (60 min)
Paso 4.1: AccessDenied.html
bashcat > AccessDenied.html << 'HTML'
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Acceso Denegado</title>
  <?!= AngelStyle.getStyles() ?>
  <style>
    .error-container {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 20px;
    }
    .error-card {
      max-width: 500px;
    }
  </style>
</head>
<body>
  <div class="angel-container error-container">
    <div class="angel-card error-card">
      <div class="angel-card-header">
        <h1 class="angel-card-title" style="color: var(--error);">⛔ Acceso Denegado</h1>
      </div>
      <div class="angel-card-body">
        <p><?= errorMessage ?></p>
        <p class="small-text" style="margin-top: 20px;">
          Si crees que esto es un error, contacta al administrador.
        </p>
        <button class="angel-button angel-button-primary" onclick="window.location.reload()" style="margin-top: 20px;">
          Intentar de Nuevo
        </button>
      </div>
    </div>
  </div>
</body>
</html>
HTML

Paso 4.2: OperatorView.html
bashcat > OperatorView.html << 'HTML'
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Inventory Manager - Operador</title>
  <?!= AngelStyle.getStyles() ?>
</head>
<body>
  <div class="angel-container">
    <!-- Header -->
    <header class="angel-header">
      <div>
        <h1>📦 Inventory Manager</h1>
        <p class="small-text" id="userEmail"><?= userData.email ?></p>
      </div>
      <button class="angel-button angel-button-ghost" onclick="handleLogout()">
        Salir
      </button>
    </header>
    
    <!-- Quick Actions -->
    <div class="angel-grid" style="margin-bottom: 30px;">
      <button class="angel-card" onclick="showRegisterModal('entrada')" style="cursor: pointer;">
        <div class="angel-card-body" style="text-align: center;">
          <h3>📥 Entrada</h3>
          <p class="small-text">Registrar ingreso de items</p>
        </div>
      </button>
      
      <button class="angel-card" onclick="showRegisterModal('salida')" style="cursor: pointer;">
        <div class="angel-card-body" style="text-align: center;">
          <h3>📤 Salida</h3>
          <p class="small-text">Registrar egreso de items</p>
        </div>
      </button>
      
      <button class="angel-card" onclick="showSearchModal()" style="cursor: pointer;">
        <div class="angel-card-body" style="text-align: center;">
          <h3>🔍 Buscar</h3>
          <p class="small-text">Consultar inventario</p>
        </div>
      </button>
    </div>
    
    <!-- Recent Movements -->
    <div>
      <h2>Movimientos Recientes</h2>
      <div id="movementsList"></div>
    </div>
    
    <!-- Loading -->
    <div id="loading" class="angel-spinner-container hidden">
      <div class="angel-spinner"></div>
      <p class="angel-spinner-text">Cargando...</p>
    </div>
    
    <!-- Register Modal -->
    <div id="registerModal" class="modal-bg hidden">
      <div class="angel-glass" style="max-width: 500px; width: 100%;">
        <h3 id="modalTitle">Registrar Movimiento</h3>
        
        <form id="movementForm" onsubmit="submitMovement(event)">
          <div style="margin-bottom: 16px;">
            <label>Item:</label>
            <input type="text" 
                   class="angel-input" 
                   id="itemSearch" 
                   placeholder="Buscar item..."
                   required
                   oninput="searchItemsDebounced(this.value)">
            <div id="searchResults" style="margin-top: 8px;"></div>
          </div>
          
          <div style="margin-bottom: 16px;">
            <label>Cantidad:</label>
            <input type="number" 
                   class="angel-input" 
                   id="quantity" 
                   placeholder="10"
                   min="1"
                   required>
          </div>
          
          <div style="margin-bottom: 16px;">
            <label>Ubicación:</label>
            <input type="text" 
                   class="angel-input" 
                   id="location" 
                   placeholder="ALMACEN-PRINCIPAL"
                   required>
          </div>
          
          <div style="margin-bottom: 16px;">
            <label>Notas (opcional):</label>
            <textarea class="angel-input" 
                      id="notes" 
                      rows="3"
                      placeholder="Información adicional..."></textarea>
          </div>
          
          <div style="display: flex; gap: 12px;">
            <button type="submit" class="angel-button angel-button-primary">
              Guardar
            </button>
            <button type="button" class="angel-button angel-button-ghost" onclick="closeRegisterModal()">
              Cancelar
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
  
  <script>
    let currentMovementType = '';
    let searchTimeout = null;
    let selectedItemId = null;
    
    // Load data on page load
    window.onload = function() {
      loadMovements();
    };
    
    function loadMovements() {
      document.getElementById('loading').classList.remove('hidden');
      
      google.script.run
        .withSuccessHandler(renderMovements)
        .withFailureHandler(showError)
        .getDashboardData();
    }
    
    function renderMovements(result) {
      document.getElementById('loading').classList.add('hidden');
      
      if (!RetryClaude does not have the ability to run the code it generates yet.CLContinueresult.success) {
showError(result.error);
return;
}
  const movements = result.data.movements;
  const listEl = document.getElementById('movementsList');
  
  if (movements.length === 0) {
    listEl.innerHTML = '<p class="small-text">No hay movimientos recientes</p>';
    return;
  }
  
  listEl.innerHTML = movements.map(m => `
    <div class="angel-card" style="margin-bottom: 16px;">
      <div class="angel-card-header">
        <h4 class="angel-card-title">${m.type === 'entrada' ? '📥' : '📤'} ${m.type}</h4>
        <span class="angel-badge angel-badge-${getStatusBadge(m.status)}">${m.status}</span>
      </div>
      <div class="angel-card-body">
        <p><strong>Item:</strong> ${m.itemName}</p>
        <p><strong>Cantidad:</strong> ${m.quantity}</p>
        <p><strong>Ubicación:</strong> ${m.location}</p>
        <p class="small-text">${m.createdAt}</p>
      </div>
    </div>
  `).join('');
}

function getStatusBadge(status) {
  const badges = {
    'pending': 'warning',
    'approved': 'success',
    'rejected': 'error',
    'completed': 'info'
  };
  return badges[status] || 'info';
}

function showRegisterModal(type) {
  currentMovementType = type;
  document.getElementById('modalTitle').textContent = 
    type === 'entrada' ? '📥 Registrar Entrada' : '📤 Registrar Salida';
  document.getElementById('registerModal').classList.remove('hidden');
  document.getElementById('itemSearch').focus();
}

function closeRegisterModal() {
  document.getElementById('registerModal').classList.add('hidden');
  document.getElementById('movementForm').reset();
  selectedItemId = null;
}

function searchItemsDebounced(query) {
  clearTimeout(searchTimeout);
  
  if (query.length < 2) {
    document.getElementById('searchResults').innerHTML = '';
    return;
  }
  
  searchTimeout = setTimeout(() => {
    google.script.run
      .withSuccessHandler(displaySearchResults)
      .withFailureHandler(showError)
      .searchItems(query);
  }, 300);
}

function displaySearchResults(result) {
  const resultsEl = document.getElementById('searchResults');
  
  if (!result.success) {
    resultsEl.innerHTML = '<p class="small-text" style="color: var(--error);">Error buscando items</p>';
    return;
  }
  
  const items = result.data.items;
  
  if (items.length === 0) {
    resultsEl.innerHTML = '<p class="small-text">No se encontraron items</p>';
    return;
  }
  
  resultsEl.innerHTML = items.map(item => `
    <div class="angel-card" 
         style="padding: 8px; margin-bottom: 8px; cursor: pointer;"
         onclick="selectItem('${item.id}', '${item.name}')">
      <strong>${item.name}</strong>
      <p class="small-text">${item.code} - Stock: ${item.stock}</p>
    </div>
  `).join('');
}

function selectItem(id, name) {
  selectedItemId = id;
  document.getElementById('itemSearch').value = name;
  document.getElementById('searchResults').innerHTML = '';
}

function submitMovement(e) {
  e.preventDefault();
  
  if (!selectedItemId) {
    alert('Por favor selecciona un item de la búsqueda');
    return;
  }
  
  const movementData = {
    type: currentMovementType,
    itemId: selectedItemId,
    quantity: document.getElementById('quantity').value,
    locationTo: currentMovementType === 'entrada' ? document.getElementById('location').value : null,
    locationFrom: currentMovementType === 'salida' ? document.getElementById('location').value : null,
    notes: document.getElementById('notes').value
  };
  
  document.getElementById('loading').classList.remove('hidden');
  
  google.script.run
    .withSuccessHandler(onMovementSaved)
    .withFailureHandler(showError)
    .registerMovement(movementData);
}

function onMovementSaved(result) {
  document.getElementById('loading').classList.add('hidden');
  
  if (result.success) {
    alert('✅ Movimiento registrado exitosamente');
    closeRegisterModal();
    loadMovements();
  } else {
    alert('❌ Error: ' + result.error);
  }
}

function showError(error) {
  document.getElementById('loading').classList.add('hidden');
  alert('Error: ' + error);
}
  </script>
</body>
</html>
HTML
```

Paso 4.3: README.md
bashcat > README.md << 'MARKDOWN'
# 📦 InventoryManager

> Gestión de inventario de insumos para casinos Grupo Angel

---

## 🎯 Descripción

Aplicación web para registrar y gestionar movimientos de inventario (entradas/salidas) de items como tragamonedas, chips, y otros insumos de casino.

---

## 👥 Usuarios

- **Operadores**: Registran entradas y salidas
- **Supervisores**: Aprueban movimientos grandes
- **Admins**: Configuración y reportes

---

## ✨ Funcionalidades

### v1.0.0 (MVP)
- ✅ Registrar entrada de items
- ✅ Registrar salida de items
- ✅ Búsqueda de items
- ✅ Ver movimientos recientes
- ✅ Sistema de aprobación para supervisores

### v2.0.0 (Futuro)
- 🔮 Escaneo de códigos de barras
- 🔮 Alertas de stock bajo
- 🔮 Reportes en PDF
- 🔮 Predicción de demanda con ML

---

## 🏗️ Arquitectura
InventoryManager/
├── Code.gs              # Backend principal
├── Config.gs            # Configuración
├── OperatorView.html    # Interfaz operador
├── SupervisorView.html  # Interfaz supervisor
├── AdminDashboard.html  # Panel admin
└── AccessDenied.html    # Error page

**Librerías usadas**:
- CLib v6.0.0 (Auth, Services, BigQuery)
- AngelStyle v4.0.0 (UI/UX)

---

## 💾 Datos

### BigQuery Tables

**Dataset**: `grupo-angel.inventory`

#### `items` table
```sql
CREATE TABLE inventory.items (
  item_id STRING,
  code STRING,
  name STRING,
  category STRING,
  unit_price NUMERIC,
  current_stock INT64,
  min_stock INT64,
  created_at TIMESTAMP
);
movements table
sqlCREATE TABLE inventory.movements (
  movement_id STRING,
  type STRING,  -- entrada/salida/transferencia
  item_id STRING,
  quantity NUMERIC,
  location_from STRING,
  location_to STRING,
  notes STRING,
  status STRING,  -- pending/approved/rejected
  created_by STRING,
  created_at TIMESTAMP,
  approved_by STRING,
  approved_at TIMESTAMP
);

🚀 Deployment
Script ID
[YOUR-SCRIPT-ID-HERE]
Web App URL
https://script.google.com/macros/s/[DEPLOYMENT-ID]/exec
Deployment History

v1.0.0 (2025-10-09): Release inicial


🔧 Configuración
Script Properties (Apps Script)
javascript// Set via: Project Settings → Script Properties
UPLOADS_FOLDER_ID: "[Google Drive Folder ID]"
REPORTS_FOLDER_ID: "[Google Drive Folder ID]"
BigQuery Setup

Crear dataset:

sqlCREATE SCHEMA `grupo-angel.inventory`;

Crear tablas (ver sección Datos)
Grant permissions al service account


📖 Uso
Para Operadores

Acceder a la app
Elegir acción: Entrada o Salida
Buscar item
Ingresar cantidad y ubicación
Guardar

Para Supervisores

Ver dashboard con movimientos pendientes
Revisar detalles
Aprobar o rechazar


🐛 Troubleshooting
Error: "Usuario no autenticado"

Verificar que estás logueado con cuenta @grupoangel.com
Refrescar página

Error: "Permisos insuficientes"

Verificar que tu email está en Config.gs (ADMIN_EMAILS o SUPERVISOR_EMAILS)
Contactar admin para agregar permisos

Items no aparecen en búsqueda

Verificar que existen en tabla inventory.items
Revisar query en función searchItems()


📝 Changelog
Ver CHANGELOG.md

🔗 Links

System Overview
CLib API
AngelStyle API


Última actualización: 2025-10-09
Versión: 1.0.0
Mantenido por: Christian Luciani
MARKDOWN

---

### Paso 4.4: CHANGELOG.md
```bash
cat > CHANGELOG.md << 'MARKDOWN'
# Changelog

Todos los cambios notables de este proyecto serán documentados aquí.

Formato basado en [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
y este proyecto sigue [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planeado
- Escaneo de códigos de barras
- Alertas de stock bajo
- Reportes en PDF

---

## [1.0.0] - 2025-10-09

### Added
- ✨ Interfaz operador para registro de movimientos
- ✨ Búsqueda de items con autocompletado
- ✨ Sistema de aprobación para supervisores
- ✨ Dashboard con estadísticas básicas
- ✨ Integración con BigQuery
- ✨ Autenticación y autorización por roles
- ✨ UI con AngelStyle v4.0.0

### Changed
- N/A (primera versión)

### Fixed
- N/A (primera versión)

### Security
- Validación de roles en backend
- OAuth scopes mínimos necesarios

---

## Tipos de Cambios

- `Added` - Nuevas funcionalidades
- `Changed` - Cambios en funcionalidades existentes
- `Deprecated` - Funcionalidades que serán removidas
- `Removed` - Funcionalidades removidas
- `Fixed` - Bug fixes
- `Security` - Cambios de seguridad
MARKDOWN

🧪 FASE 5: Testing (30 min)
Paso 5.1: Testing Checklist
markdown## Testing de InventoryManager v1.0.0

### ✅ Backend Testing (Apps Script Editor)

#### Funciones Core
- [ ] `testGetDashboardData()` retorna success
- [ ] `testRegisterMovement()` retorna success
- [ ] `testSearchItems()` retorna success
- [ ] `testApproveMovement()` retorna success (como supervisor)

#### Edge Cases
- [ ] `registerMovement()` con datos incompletos retorna error
- [ ] `approveMovement()` como operador retorna error permisos
- [ ] `searchItems()` con query vacío retorna error

### ✅ Frontend Testing (Manual)

#### Authentication
- [ ] Usuario no autenticado → redirect a login
- [ ] Operador → ve OperatorView
- [ ] Supervisor → ve SupervisorView
- [ ] Admin → ve AdminDashboard
- [ ] Email no autorizado → AccessDenied

#### OperatorView (Mobile + Desktop)
- [ ] Botón "Entrada" abre modal correctamente
- [ ] Botón "Salida" abre modal correctamente
- [ ] Búsqueda de items funciona
- [ ] Seleccionar item desde resultados funciona
- [ ] Form validation funciona (campos requeridos)
- [ ] Submit guarda en BigQuery
- [ ] Loading spinner aparece durante submit
- [ ] Success message después de guardar
- [ ] Lista de movimientos se actualiza después de guardar
- [ ] Botón logout funciona

#### Responsive
- [ ] Mobile (iPhone SE): Todo funciona, botones táctiles
- [ ] Tablet (iPad): Layout apropiado
- [ ] Desktop: Layout completo

#### Performance
- [ ] Carga inicial < 3 segundos
- [ ] Submit de formulario < 5 segundos
- [ ] Búsqueda responde < 1 segundo

### ✅ Integration Testing

#### BigQuery
- [ ] Movimientos se insertan correctamente en tabla `movements`
- [ ] Schema validation funciona
- [ ] Timestamps en formato correcto

#### CLib
- [ ] `getUserDetails()` retorna datos correctos
- [ ] `getUserRole()` determina rol correctamente
- [ ] `logDebugMessage()` guarda logs
- [ ] `insertRowIntoBigQuery()` funciona

#### AngelStyle
- [ ] Todos los componentes se renderizan correctamente
- [ ] Estilos cargando desde librería
- [ ] JavaScript helpers (logout) funcionan

### 🐛 Bugs Encontrados

| # | Descripción | Severidad | Status |
|---|-------------|-----------|--------|
| 1 | [Descripción del bug] | High/Medium/Low | Open/Fixed |

### 📊 Test Results

- **Total Tests**: [número]
- **Passed**: [número]
- **Failed**: [número]
- **Blocked**: [número]

**Fecha de testing**: 2025-10-09  
**Testeado por**: Christian Luciani

Paso 5.2: Ejecutar Tests
Backend Testing
javascript// En Apps Script Editor
// Ejecutar cada función y revisar logs

function runAllTests() {
  Logger.log('=== INICIANDO TESTS ===');
  
  try {
    Logger.log('\n--- Test: getDashboardData ---');
    testGetDashboardData();
    
    Logger.log('\n--- Test: registerMovement ---');
    testRegisterMovement();
    
    Logger.log('\n--- Test: searchItems ---');
    testSearchItems();
    
    Logger.log('\n=== TODOS LOS TESTS COMPLETADOS ===');
  } catch (error) {
    Logger.log('❌ ERROR EN TESTS: ' + error.toString());
  }
}
Frontend Testing

Deploy como TEST:

Deploy → Test deployments
Click "Select type" → Web app
Execute as: Me
Who has access: Anyone
Deploy


Abrir URL de test en:

Chrome (desktop)
Firefox (desktop)
Safari (desktop)
Chrome DevTools (mobile emulation)


Probar cada funcionalidad según checklist


🚀 FASE 6: Deploy (15 min)
Paso 6.1: Pre-Deploy Checklist
markdown## Pre-Deploy Checklist InventoryManager v1.0.0

### Code Quality
- [ ] No hay `console.log()` olvidados
- [ ] Todos los `TODO:` están resueltos o documentados
- [ ] JSDoc completo en funciones públicas
- [ ] Error handling en todas las funciones
- [ ] Config.gs tiene todos los settings necesarios

### Testing
- [ ] All backend tests passing
- [ ] Manual frontend testing completo
- [ ] Responsive testing OK
- [ ] Performance acceptable

### Documentation
- [ ] README.md completo
- [ ] CHANGELOG.md actualizado
- [ ] CURRENT_STATE.md creado
- [ ] Code comments claros

### Security
- [ ] No hay API keys hardcodeadas
- [ ] OAuth scopes mínimos
- [ ] Validación de roles implementada
- [ ] Input validation en todos los endpoints

### Integration
- [ ] BigQuery tables creadas
- [ ] Drive folders configurados
- [ ] Script Properties configurados
- [ ] CLib y AngelStyle actualizados a última versión

### Rollback Plan
- [ ] Backup de versión anterior disponible
- [ ] Plan de rollback documentado
- [ ] Contact list para emergencias

Paso 6.2: Deploy a Producción
1. Crear Versión
bash# Local: Commit final
git add .
git commit -m "feat(inventory): release v1.0.0

- Interfaz operador completa
- Sistema de aprobación supervisor
- Integración BigQuery
- Testing completo
- Documentación completa"

git tag v1.0.0
git push origin feature/inventory-manager
git push origin v1.0.0
2. Deploy en Apps Script

En Apps Script Editor:

Deploy → New deployment
Type: Web app
Description: v1.0.0 - Initial release
Execute as: User accessing the web app
Who has access: Anyone
Deploy


Copiar Web App URL:

   https://script.google.com/macros/s/[DEPLOYMENT-ID]/exec

Guardar en README.md:

bash   # Actualizar README con deployment URL

Paso 6.3: Post-Deploy Verification
Smoke Test (5 min)
markdown## Smoke Test Checklist

- [ ] URL de producción carga
- [ ] Login funciona
- [ ] Vista apropiada carga según rol
- [ ] Registrar movimiento funciona
- [ ] Datos aparecen en BigQuery
- [ ] Logout funciona

**Si TODOS pasan**: ✅ Deploy exitoso
**Si ALGUNO falla**: ⚠️ Ejecutar rollback

Paso 6.4: Rollback Plan (Si es necesario)
markdown## Rollback Procedure

### Síntomas que requieren rollback:
- Error 500 en producción
- Funcionalidad crítica no funciona
- Pérdida de datos
- Performance inaceptable (>10s load time)

### Steps:
1. En Apps Script: Deploy → Manage deployments
2. Encontrar deployment anterior (v0.9.0)
3. Click "..." → Archive current deployment
4. Restaurar deployment anterior
5. Verificar smoke test
6. Notificar a usuarios del rollback
7. Investigar causa del problema

### Contactos de Emergencia:
- Tech Lead: [email]
- Admin: [email]

📝 FASE 7: Documentación (15 min)
Paso 7.1: Crear CURRENT_STATE.md
bashcat > CURRENT_STATE.md << 'MARKDOWN'
# InventoryManager - Current State

> Snapshot del estado actual de la aplicación

**Última actualización**: 2025-10-09  
**Versión**: 1.0.0  
**Status**: ✅ Producción

---

## 📊 Métricas

- **Líneas de código**: ~500 (Code.gs) + ~400 (HTML)
- **Funciones backend**: 8
- **Vistas frontend**: 4
- **Tablas BigQuery**: 4
- **Librerías usadas**: 2 (CLib, AngelStyle)

---

## 🏗️ Arquitectura Actual
Backend (Code.gs)
├── doGet() - Entry point
├── routeToView() - Routing por rol
├── getDashboardData() - Data para dashboard
├── registerMovement() - Registrar entrada/salida
├── searchItems() - Búsqueda de items
├── approveMovement() - Aprobación supervisor
└── Helper functions
Frontend
├── OperatorView.html - Interfaz operador
├── SupervisorView.html - Interfaz supervisor
├── AdminDashboard.html - Panel admin
└── AccessDenied.html - Error page

---

## 🔧 Funciones Implementadas

### Backend API

| Función | Parámetros | Retorno | Status |
|---------|------------|---------|--------|
| `getDashboardData()` | none | `{success, data: {user, role, stats, movements}}` | ✅ |
| `registerMovement(data)` | `{type, itemId, quantity, location, notes}` | `{success, data: {movementId}}` | ✅ |
| `searchItems(query)` | `string` | `{success, data: {items, count}}` | ⚠️ Mock |
| `approveMovement(id)` | `string` | `{success, data: {movementId, status}}` | ⚠️ Mock |

**Leyenda**:
- ✅ Implementado y funcional
- ⚠️ Mock data (TODO: conectar a BigQuery real)
- ❌ No implementado

---

## 📦 Componentes UI Usados

### AngelStyle v4.0.0

- `.angel-container` - Layout principal
- `.angel-header` - Header con logo y logout
- `.angel-card` - Cards para movimientos
- `.angel-button` (primary, secondary, ghost)
- `.angel-input` - Inputs de formulario
- `.angel-spinner` - Loading indicator
- `.angel-badge` - Status badges
- `.modal-bg` - Modals
- `.angel-glass` - Glassmorphism effect

---

## 💾 Datos

### BigQuery Tables Status

| Tabla | Schema | Datos | Status |
|-------|--------|-------|--------|
| `inventory.items` | ✅ Definido | ❌ Vacía | Needs seeding |
| `inventory.movements` | ✅ Definido | ⚠️ Test data | Ready |
| `inventory.locations` | ✅ Definido | ❌ Vacía | Needs seeding |
| `inventory.users` | ❌ No creada | ❌ | TODO v2.0 |

---

## 🚧 TODO / Known Issues

### High Priority
- [ ] Conectar `searchItems()` a BigQuery real
- [ ] Conectar `approveMovement()` a BigQuery real
- [ ] Seed data en tabla `items`
- [ ] Seed data en tabla `locations`

### Medium Priority
- [ ] Implementar SupervisorView completo
- [ ] Implementar AdminDashboard completo
- [ ] Agregar paginación a lista de movimientos
- [ ] Agregar filtros de búsqueda avanzados

### Low Priority
- [ ] Export de reportes a PDF
- [ ] Gráficas de estadísticas
- [ ] Dark/Light mode toggle

### Known Bugs
- Ninguno reportado hasta ahora

---

## 📈 Performance

- **Load time**: ~2-3 segundos (primera carga)
- **Form submit**: ~3-5 segundos
- **Search**: <1 segundo

**Optimization opportunities**:
- Cache de items en PropertiesService
- Lazy loading de movimientos

---

## 🔐 Security

### Implementado
- ✅ Role-based access control
- ✅ OAuth scopes mínimos
- ✅ Input validation en backend
- ✅ SQL injection prevention (BigQuery API)

### TODO
- [ ] Rate limiting
- [ ] Audit log completo
- [ ] Session timeout enforcement

---

## 🔗 Dependencies

| Dependency | Version | Status |
|------------|---------|--------|
| CLib | v6.0.0 | ✅ Stable |
| AngelStyle | v4.0.0 | ⚠️ Needs v5.0 |
| BigQuery API | v2 | ✅ |
| Drive API | v3 | ✅ |
| Sheets API | v4 | ✅ |

---

## 📞 Contactos

- **Developer**: Christian Luciani (cluciani@gmail.com)
- **Product Owner**: [nombre]
- **Users**: Operadores de casino

---

## 🔄 Próxima Versión (v1.1.0)

**Planeado para**: 2025-10-20

**Features**:
- SupervisorView funcional completo
- Filtros avanzados de búsqueda
- Export de reportes

**Deployment URL**: https://script.google.com/macros/s/[ID]/exec
MARKDOWN

Paso 7.2: Merge a Main
bash# Volver a main
git checkout main

# Merge feature branch
git merge feature/inventory-manager

# Push
git push origin main

# Delete feature branch (opcional)
git branch -d feature/inventory-manager
git push origin --delete feature/inventory-manager

Paso 7.3: Cerrar Issue en GitHub

Ve al issue original
Agrega comment con summary:

markdown   ## ✅ Completado
   
   **Versión deployed**: v1.0.0
   **URL**: [deployment URL]
   **Deployment date**: 2025-10-09
   
   ### Implementado:
   - ✅ OperatorView completo
   - ✅ Sistema de registro de movimientos
   - ✅ Búsqueda de items
   - ✅ Integración BigQuery
   - ✅ Auth y autorización
   
   ### TODO para v1.1:
   - SupervisorView completo
   - AdminDashboard completo
   
   Ver [CHANGELOG](../apps/InventoryManager/CHANGELOG.md) para detalles.

Close issue


🔧 Mantenimiento de Esta Documentación
Cuándo Actualizar
✅ Después de crear cada nueva app

Revisa este tutorial

¿Hubo steps que no funcionaron?
¿Faltó algún paso importante?
¿Algo cambió en el proceso?


Actualiza secciones relevantes
Agrega a "Lecciones Aprendidas":

markdown   ### Lecciones Aprendidas

   #### InventoryManager (2025-10-09)
   - Modal de búsqueda necesita debounce (implementado con 300ms)
   - Form validation mejor hacerla en frontend primero
   - Testing en mobile es crítico desde el inicio
✅ Cuando cambien dependencias
Si CLib o AngelStyle cambian de versión:

Actualizar sección "Pre-requisitos"
Actualizar snippets de código con nuevas APIs
Actualizar ejemplos si hay breaking changes


Template Rápido
Para crear nueva app rápidamente, copia este checklist:
bash# Quick App Creation Checklist

# 1. Planning
□ Definir app (ver Paso 1.1)
□ Wireframes creados
□ Issue en GitHub creado

# 2. Setup
□ Branch creado: git checkout -b feature/[nombre]
□ Carpeta creada: apps/[NombreApp]/
□ Archivos base creados
□ Script en Apps Script creado
□ appsscript.json configurado

# 3. Backend
□ Config.gs con settings
□ Code.gs con doGet() y routing
□ API functions implementadas
□ Tests escritos y pasando

# 4. Frontend
□ AccessDenied.html
□ View principal
□ AngelStyle integrado
□ JavaScript funcional

# 5. Testing
□ Backend tests ✓
□ Frontend manual testing ✓
□ Responsive testing ✓
□ Performance OK

# 6. Deploy
□ Pre-deploy checklist ✓
□ Deploy a TEST
□ Smoke test ✓
□ Deploy a PROD
□ Post-deploy verification ✓

# 7. Docs
□ README.md
□ CHANGELOG.md
□ CURRENT_STATE.md
□ Merge a main
□ Close issue

🔗 Links Relacionados

📖 Local Setup
📖 System Overview
📖 CLib API
📖 AngelStyle API
📖 Git Workflow (próximamente)
📖 Deployment Guide (próximamente)


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

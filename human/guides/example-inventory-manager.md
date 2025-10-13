# 🚀 Ejemplo Detallado: InventoryManager

> Guía de Implementación Paso a Paso de una App de Inventario

---

## 🎯 Objetivo de este Documento

Este documento proporciona el código y los pasos concretos de una aplicación **real** (`InventoryManager`) construida bajo el ecosistema de Grupo Angel Apps.

Sirve como el complemento detallado de la guía general [Creando Nueva App](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/creating-new-app.md).

## 📝 1. Definición Inicial (FASE 1)

El primer paso es tener la idea y el alcance. Este fue el resultado de la fase de planificación:

### Definición de App: InventoryManager
Gestión de inventario de insumos de casinos (tragamonedas, chips, etc.)

*   **¿Quién la usa?**: Operadores, Supervisores, Admins
*   **Funcionalidad Mínima (MVP)**: Registrar entrada/salida de items, Ver inventario actual.

## 📁 2. Estructura de Carpetas (FASE 2)

Así debe quedar la estructura final en el repositorio de código `grupo-angel-apps/`:

```plaintext
apps/InventoryManager/
├── 📁 Code.gs                 # Backend principal
├── 📁 Config.gs               # Configuración
├── 📄 Index.html              # Landing page
├── 📄 OperatorView.html       # Interfaz operador
├── 📄 SupervisorView.html     # Interfaz supervisor
├── 📄 AdminDashboard.html     # Panel admin
├── 📄 AccessDenied.html       # Error 403
├── 📄 appsscript.json         # Manifest
├── 📄 README.md               # Overview
├── 📄 CHANGELOG.md            # Historial de cambios
└── 📄 CURRENT_STATE.md        # Estado actual detallado
⚙️ 3. Archivos de Configuración y Manifiesto
A. Archivo appsscript.json (Manifiesto)
Se configuran las librerías Core (CLib, AngelStyle) y los servicios avanzados (BigQuery, Drive, Sheets).
code
JSON
{
  "timeZone": "America/Panama",
  "dependencies": {
    "enabledAdvancedServices": [
      { "userSymbol": "BigQuery", "serviceId": "bigquery", "version": "v2" },
      { "userSymbol": "Drive", "serviceId": "drive", "version": "v3" },
      { "userSymbol": "Sheets", "serviceId": "sheets", "version": "v4" }
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
  "runtimeVersion": "V8",
  "webapp": { /* ... */ },
  "oauthScopes": [
    "https://www.googleapis.com/auth/spreadsheets",
    // ... otros scopes
  ]
}
B. Archivo Config.gs
Define constantes como el BQ_DATASET_ID y las listas de correos electrónicos para roles fijos (ADMIN_EMAILS, SUPERVISOR_EMAILS).
code
JavaScript
/**
 * Configuration for InventoryManager
 * DO NOT commit sensitive data - use PropertiesService
 */

// Project Info & BigQuery
const PROJECT_NAME = 'InventoryManager';
const BQ_PROJECT_ID = 'grupo-angel';
const BQ_DATASET_ID = 'inventory';

// User Roles - Hardcoded para MVP 
const ADMIN_EMAILS = [
  'admin@grupoangel.com',
  'cluciani@gmail.com'
];

// Status & Movement Types Constants
const STATUS = {
  PENDING: 'pending',
  APPROVED: 'approved',
};
const MOVEMENT_TYPES = {
  IN: 'entrada',
  OUT: 'salida',
};

/**
 * Check if user is supervisor (or admin)
 * @param {string} email - User email
 * @returns {boolean}
 */
function isSupervisor(email) {
  // ... (implementation)
}
💻 4. Implementación Backend: Code.gs (FASE 3)
Aquí está el corazón del backend de Apps Script, divido por funcionalidad clave.
A. Autenticación y Routing (doGet, routeToView)
code
JavaScript
/**
 * Entry point for web app
 * @param {Object} e - Event object
 * @returns {HtmlOutput}
 */
function doGet(e) {
  try {
    const userResult = CLib.getUserDetails(); // 1. Auth
    const roleResult = CLib.getUserRole(userResult.data.email); // 2. Get Role
    
    // Check results and route to view or deny access
    if (!userResult.success || !roleResult.success) {
      return renderAccessDenied('No se pudo autenticar/determinar permisos');
    }
    
    return routeToView(roleResult.data.role, userResult.data);
    
  } catch (error) { /* ... error logging ... */ }
}

/**
 * Route user to appropriate view based on role
 * ...
 */
function routeToView(role, userData) {
  // switch-case logic based on role
  // e.g., case 'admin': template = HtmlService.createTemplateFromFile('AdminDashboard');
}
B. Función de Negocio (registerMovement)
Utiliza CLib.insertRowIntoBigQuery para la operación atómica de guardar.
code
JavaScript
/**
 * Register new movement (entrada/salida)
 * @param {Object} movementData - Movement data
 * @returns {Object} {success, data/error}
 */
function registerMovement(movementData) {
  try {
    // 1. Validate Input and User Auth
    const userResult = CLib.getUserDetails();
    
    // 2. Prepare Row (using STATUS.PENDING)
    const row = {
      movement_id: Utilities.getUuid(),
      status: STATUS.PENDING,
      created_by: userResult.data.email,
      // ... (other data from movementData)
    };
    
    // 3. Insert to BigQuery using CLib
    const insertResult = CLib.insertRowIntoBigQuery(
      BQ_PROJECT_ID,
      BQ_DATASET_ID,
      BQ_TABLES.movements, // BQ_TABLES definido en Config.gs
      row
    );
    
    if (!insertResult.success) { /* ... return error ... */ }
    
    return { success: true, data: { movementId: row.movement_id } };
    
  } catch (error) { /* ... error handling ... */ }
}
🎨 5. Implementación Frontend: OperatorView.html (FASE 4)
La interfaz utiliza componentes AngelStyle para la Neuro Accesibilidad y consistencia visual.
A. HTML Base y Formularios
Contiene el header (.angel-header), las acciones rápidas (.angel-grid) y el Modal de Registro de movimientos, incluyendo el input para la búsqueda de items.
B. JavaScript (Búsqueda y Envío)
Se extrae la parte clave que contiene el Fix Crítico (!result.success) y la lógica de búsqueda con debounce.
code
Html
<script>
  let selectedItemId = null;
  let searchTimeout = null;

  // Carga inicial y Renderizado
  function loadMovements() { /* ... google.script.run.getDashboardData() ... */ }

  function renderMovements(result) {
    // 💡 Aquí estaba el artefacto de Chatbot que fue corregido en el documento padre
    if (!result.success) { 
        showError(result.error); 
        return;
    }
    // ... Renderizado de la lista
  }

  // Lógica de Búsqueda Debounce
  function searchItemsDebounced(query) {
    clearTimeout(searchTimeout);
    searchTimeout = setTimeout(() => {
      google.script.run
        .withSuccessHandler(displaySearchResults)
        .searchItems(query);
    }, 300);
  }

  // Lógica de Envío de Formulario
  function submitMovement(e) {
    e.preventDefault();
    if (!selectedItemId) { alert('Selecciona un item'); return; }
    
    // Prepara datos y llama al backend:
    google.script.run
      .withSuccessHandler(onMovementSaved)
      .registerMovement(movementData);
  }

</script>
💾 6. Documentación Anidada (FASE 7)
A. CURRENT_STATE.md (Extracto)
El snapshot documenta el estado real (incluso con la presencia de Mock Data).
Función	Parámetros	Retorno	Status
getDashboardData()	none	{success, data: {user, role, stats, movements}}	✅
registerMovement(data)	{type, itemId, quantity, location, notes}	{success, data: {movementId}}	✅
searchItems(query)	string	{success, data: {items, count}}	⚠️ Mock
Leyenda: ✅ Implementado / ⚠️ Mock data.
(Nota: Se omite el código completo de README.md y CHANGELOG.md del ejemplo para enfocarse en la estructura de la aplicación. Se asume que existen y contienen el contenido completo original del Paso 4.3 y 4.4.)

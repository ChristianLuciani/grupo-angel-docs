# 🏗️ System Overview

> Arquitectura completa del ecosistema Grupo Angel Apps

---

## 🎯 Visión General

Sistema modular de aplicaciones web construido sobre **Google Apps Script** para automatizar procesos y centralizar datos de Grupo Angel.

### Objetivo Principal:
Crear un **Departamento de Datos y Automatización** con:
- ✅ BigQuery como fuente única de verdad
- ✅ Business Intelligence centralizado
- ✅ Preparación para Machine Learning
- ✅ Interfaces modernas y consistentes

---

## 📊 Arquitectura de 3 Capas
```mermaid
graph TB
    subgraph "CAPA 3: Data Layer"
        BQ[BigQuery<br/>Data Warehouse]
        SHEETS[Google Sheets<br/>Cache/Buffer]
        DRIVE[Google Drive<br/>Files Storage]
    end
    
    subgraph "CAPA 2: Libraries"
        CLIB[CLib v6.0.0<br/>Auth + Services + ETL]
        ASTYLE[AngelStyle v4.0.0<br/>UI/UX Components]
    end
    
    subgraph "CAPA 1: Applications"
        TICKET[TicketProcessor<br/>OCR + Approval]
        XLS[xls2gsheet<br/>File Converter]
        CASINO[CasinoETL<br/>Data Transform]
    end
    
    subgraph "CAPA 0: External"
        USER[👤 Users<br/>Mobile/Desktop]
        GEMINI[🤖 Gemini AI<br/>Vision + Text]
    end
    
    USER -->|Interact| TICKET
    USER -->|Interact| XLS
    USER -->|Interact| CASINO
    
    TICKET -->|Uses| CLIB
    TICKET -->|Uses| ASTYLE
    XLS -->|Uses| CLIB
    XLS -->|Uses| ASTYLE
    CASINO -->|Uses| CLIB
    
    CLIB -->|Query| BQ
    CLIB -->|Store| DRIVE
    CLIB -->|Call| GEMINI
    CLIB -->|Read/Write| SHEETS
    
    ASTYLE -->|Renders| USER
    ```

🔍 Detalle por Capa
CAPA 0: External (Usuarios y Servicios)
ComponenteTipoPropósitoUsuariosHumanosOperadores, supervisores, adminsGemini AIAPIOCR, clasificación, análisis de textoBigQuery APIGCP ServiceInsert, query de datosDrive APIGoogle ServiceStorage de archivos

CAPA 1: Applications (Apps Específicas)
TicketProcessor v13.0.0 ✅ Producción
Propósito: OCR de tickets de casino con aprobación supervisor
Flujo:
mermaidsequenceDiagram
    participant O as Operador
    participant TP as TicketProcessor
    participant G as Gemini Vision
    participant S as Supervisor
    participant BQ as BigQuery
    
    O->>TP: Sube foto de ticket
    TP->>G: Analiza imagen
    G->>TP: Retorna datos extraídos
    TP->>O: Muestra preview editable
    O->>TP: Confirma datos
    TP->>S: Solicita aprobación
    S->>TP: Aprueba/Rechaza
    TP->>BQ: Inserta registro
Componentes:

Code.gs: Lógica principal, routing
Operator.html: Interfaz móvil (SPA)
Supervisor.html: Dashboard de aprobación
AdminDashboard.html: Panel de configuración
Config.gs: Variables de entorno

Librerías usadas: CLib v6 (completo) + AngelStyle v4 (completo)

xls2gsheet v1.0.0 ⚠️ Standalone
Propósito: Convierte archivos XLS de SIELCON (HTML disfrazado) a Google Sheets
Flujo:
mermaidgraph LR
    A[Usuario sube .xls] --> B[Detecta que es HTML]
    B --> C[Extrae tablas]
    C --> D[Crea nueva Sheet]
    D --> E[Formatea datos]
    E --> F[Retorna link]
Estado actual:

❌ NO usa CLib (código standalone)
❌ NO usa AngelStyle (estilos inline)
⚠️ Tiene función extraerDatosDeHTML() duplicada con CLib
🎯 Candidato #1 para migración

Por qué migrar:

Mantener consistencia arquitectónica
Eliminar duplicación de código
Usar componentes UI modernos
Facilitar mantenimiento


CasinoETL v1.0.0 🚧 En desarrollo
Propósito: Transformaciones ETL para archivos de sistemas casino
Arquitectura especial:

NO es app standalone
Es módulo dentro de CLib (CasinoETL.gs)
Apps futuras lo usarán como servicio

Transformaciones disponibles:
javascript// AXES Gaming
transformAxesGamingVoucher(htmlContent)
transformAxesTITOBreakdown(htmlContent)

// SIELCON
transformSielconTITO(htmlContent)
transformSielconByPlayer(htmlContent)
Flujo típico:
mermaidgraph LR
    A[App sube archivo] --> B[CLib.detectFileType]
    B --> C{Tipo?}
    C -->|AXES| D[transformAxesGamingVoucher]
    C -->|SIELCON| E[transformSielconTITO]
    D --> F[Retorna JSON normalizado]
    E --> F
    F --> G[App inserta a BigQuery]

CAPA 2: Libraries (Lógica Compartida)
CLib v6.0.0 ✅ Stable
ID: 1hysGl2FQqsVDB1KOqkvPg5gLqCaXv4-4RgjvNYYdnlS0_-MamZ2kZ6Ij
Propósito: Core business logic compartida
Módulos:
🔐 Auth Module (Code.gs)
javascriptgetUserDetails()          // Info del usuario actual
getUserRole()            // Rol: admin/operator/supervisor
setImpersonatedRole()    // Simular otro rol (testing)
clearImpersonatedRole()  // Limpiar simulación
getLogoutUrl()          // URL de logout
Flujo de autenticación:
mermaidgraph TD
    A[Usuario accede app] --> B[getUserDetails]
    B --> C{¿Authenticated?}
    C -->|NO| D[Redirect a login]
    C -->|SÍ| E[getUserRole]
    E --> F{¿Role permitido?}
    F -->|NO| G[AccessDenied.html]
    F -->|SÍ| H[Cargar interfaz]

🛠️ Services Module (Code.gs)
javascript// Logging
logDebugMessage(message, context)

// File Management
getOrCreateMonthlyFolder(parentId, prefix)

// AI Services
callGenerativeApi(prompt, imageBase64)  // Gemini 2.5 Flash

// BigQuery
insertRowIntoBigQuery(projectId, datasetId, tableId, row)
Ejemplo de uso:
javascript// En cualquier app
function processTicket(imageData) {
  try {
    // 1. Log inicio
    CLib.logDebugMessage('Procesando ticket', {user: Session.getActiveUser().getEmail()});
    
    // 2. Llamar Gemini
    const prompt = "Extrae: monto, fecha, número";
    const result = CLib.callGenerativeApi(prompt, imageData);
    
    // 3. Insertar a BigQuery
    const row = {
      amount: result.amount,
      date: result.date,
      ticket_number: result.number
    };
    CLib.insertRowIntoBigQuery('grupo-angel', 'casino', 'tickets', row);
    
    return {success: true, data: result};
  } catch (error) {
    CLib.logDebugMessage('Error en processTicket', {error: error.toString()});
    return {success: false, error: error.toString()};
  }
}

🔄 ETL Module (CasinoETL.gs)
javascript// AXES Gaming
transformAxesGamingVoucher(htmlContent)
transformAxesTITOBreakdown(htmlContent)

// SIELCON
transformSielconTITO(htmlContent)
transformSielconByPlayer(htmlContent)

// Helpers
detectFileType(content)
normalizeAmount(stringAmount)
parseDate(stringDate)
Formato de salida normalizado:
javascript{
  success: true,
  data: {
    type: 'TITO_VOUCHER',
    system: 'AXES',
    records: [
      {
        date: '2025-10-08',
        amount: 100.50,
        ticket_number: 'ABC123',
        // ... más campos
      }
    ]
  }
}

📦 Legacy Module (Code.gs)
javascriptxls2gsheet(fileId)           // Convierte XLS→Sheet (legacy)
extraerDatosDeHTML(html)     // Parser HTML (legacy)
⚠️ Nota: Estas funciones existen por compatibilidad, pero están deprecated. Nuevas apps deben usar módulo ETL.

AngelStyle v4.0.0 ⚠️ Needs Refactor
ID: 19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S
Propósito: UI/UX consistente en todas las apps
Archivos actuales:

Code.gs: Función getStyles() que retorna CSS+JS
main.css.html: Estilos globales
main.js.html: Helpers JavaScript
casino.css.html: Estilos específicos casino

Componentes actuales (v4.0.0):
css/* Buttons */
.btn { /* Base button */ }
.btn-primary { /* Acción principal */ }
.btn-secondary { /* Acción secundaria */ }
.btn-success { /* Confirmar */ }
.btn-danger { /* Cancelar/Eliminar */ }

/* Loading */
.spinner-circle { /* Loading animation */ }

/* Modals */
.modal-bg { /* Fondo de modal */ }

/* Utilities */
.hidden { display: none; }
JavaScript Helpers:
javascripthandleLogout()              // Logout y redirect
checkImpersonation()        // Verifica si está simulando rol
stopImpersonating()         // Detiene simulación
Uso en apps:
html<!DOCTYPE html>
<html>
  <head>
    <?!= AngelStyle.getStyles() ?>
  </head>
  <body>
    <button class="btn btn-primary">Guardar</button>
    <div class="spinner-circle hidden" id="loading"></div>
  </body>
</html>

⚠️ Refactor Planeado: AI-Modern Dark Theme
Nuevos componentes a implementar:
css/* Paleta Grupo Angel */
:root {
  --angel-green-dark: #1a5f3f;
  --angel-green-medium: #2d8659;
  --angel-green-light: #4a9d6f;
  --angel-lime: #a8b446;
  --angel-cyan: #00d9ff;
  --bg-primary: #0a0f0d;
  --text-primary: #e8f5e9;
}

/* Componentes nuevos */
.angel-container { /* Layout principal */ }
.angel-button { /* Botón moderno */ }
.angel-input { /* Input con glow */ }
.angel-card { /* Card con glassmorphism */ }
.angel-glass { /* Efecto vidrio */ }
.angel-glow-soft { /* Glow sutil */ }
.angel-spinner { /* Loading moderno */ }
.angel-logo-container { /* Logo con efecto */ }
Fuente: Lexend (weights: 300-800)

CAPA 3: Data Layer
BigQuery (Data Warehouse)
Proyecto GCP: grupo-angel (ID específico en Config)
Datasets:
grupo-angel/
├── casino/              # Datos de casinos
│   ├── tickets         # Tickets procesados
│   ├── vouchers        # Vouchers TITO
│   └── logs            # Logs de operaciones
├── restaurants/         # (futuro)
└── analytics/          # Reportería agregada
Schema típico (tickets):
sqlCREATE TABLE casino.tickets (
  ticket_id STRING,
  amount NUMERIC,
  date DATE,
  operator_email STRING,
  supervisor_email STRING,
  status STRING,  -- pending/approved/rejected
  ocr_confidence FLOAT64,
  created_at TIMESTAMP,
  approved_at TIMESTAMP
);
Flujo de inserción:
mermaidgraph LR
    A[App valida datos] --> B[CLib.insertRowIntoBigQuery]
    B --> C[BigQuery API]
    C --> D{Éxito?}
    D -->|SÍ| E[Return success]
    D -->|NO| F[Log error + retry]

Google Sheets (Cache/Buffer)
Uso:

📊 Cache temporal antes de BigQuery
📋 Reportes visuales para usuarios
🔄 Buffer para validación manual

Ejemplo: TicketProcessor crea Sheet diaria con tickets pendientes

Google Drive (File Storage)
Estructura:
Grupo Angel/
├── Uploads/              # Archivos subidos por usuarios
│   ├── 2025-10/         # Carpetas mensuales (auto)
│   │   ├── Tickets/
│   │   └── Reports/
├── Processed/           # Archivos procesados
└── Backups/             # Respaldos automáticos
Función helper:
javascript// Crea carpeta del mes automáticamente
const folderId = CLib.getOrCreateMonthlyFolder(
  'ROOT_FOLDER_ID',
  'Tickets'
);
// Retorna: ID de carpeta "2025-10-Tickets"

🔄 Flujos de Datos Completos
Flujo 1: Procesar Ticket
mermaidsequenceDiagram
    participant U as Usuario
    participant UI as TicketProcessor UI
    participant AS as Apps Script Backend
    participant CL as CLib
    participant G as Gemini API
    participant D as Drive
    participant BQ as BigQuery
    
    U->>UI: Sube foto
    UI->>AS: POST /upload
    AS->>D: Guarda imagen
    D-->>AS: file_id
    AS->>CL: callGenerativeApi(prompt, image)
    CL->>G: Analyze image
    G-->>CL: JSON con datos
    CL-->>AS: {amount, date, number}
    AS->>UI: Muestra preview
    U->>UI: Confirma datos
    UI->>AS: POST /submit
    AS->>CL: insertRowIntoBigQuery(...)
    CL->>BQ: INSERT INTO tickets
    BQ-->>CL: success
    CL-->>AS: {success: true}
    AS->>UI: Muestra confirmación
Tiempo típico: 3-5 segundos

Flujo 2: Convertir Archivo Casino
mermaidsequenceDiagram
    participant U as Usuario
    participant UI as xls2gsheet UI
    participant AS as Apps Script Backend
    participant CL as CLib
    participant ETL as CasinoETL Module
    participant SH as Sheets API
    
    U->>UI: Sube archivo .xls
    UI->>AS: POST /convert
    AS->>CL: detectFileType(content)
    CL-->>AS: type: 'AXES_VOUCHER'
    AS->>ETL: transformAxesGamingVoucher(html)
    ETL-->>AS: normalized JSON
    AS->>SH: Crea nueva Sheet
    SH-->>AS: spreadsheet_id
    AS->>SH: Inserta datos formateados
    SH-->>AS: success
    AS->>UI: Retorna link a Sheet
    UI->>U: Muestra link clickeable
Tiempo típico: 5-10 segundos (depende de tamaño)

Flujo 3: Autenticación y Autorización
mermaidgraph TD
    A[Usuario accede URL] --> B[Apps Script: doGet]
    B --> C[CLib.getUserDetails]
    C --> D{¿Email válido?}
    D -->|NO| E[Redirect Google Login]
    D -->|SÍ| F[CLib.getUserRole]
    F --> G{¿Role permitido?}
    G -->|NO| H[AccessDenied.html]
    G -->|SÍ| I[Cargar interfaz apropiada]
    I --> J{Role}
    J -->|operator| K[Operator.html]
    J -->|supervisor| L[Supervisor.html]
    J -->|admin| M[AdminDashboard.html]

🔧 Principios de Diseño
1. DRY (Don't Repeat Yourself)
❌ Mal:
javascript// En App1
function getUserEmail() {
  return Session.getActiveUser().getEmail();
}

// En App2 (duplicado)
function getUserEmail() {
  return Session.getActiveUser().getEmail();
}
✅ Bien:
javascript// En CLib (una sola vez)
function getUserDetails() {
  return {
    email: Session.getActiveUser().getEmail(),
    // ... más info
  };
}

// En apps
const user = CLib.getUserDetails();

2. Separation of Concerns
mermaidgraph TD
    A[User Action] -->|UI Layer| B[HTML/CSS/JS]
    B -->|Logic Layer| C[Apps Script Backend]
    C -->|Service Layer| D[CLib Functions]
    D -->|Data Layer| E[BigQuery/Drive/Sheets]
Cada capa tiene una responsabilidad:

UI: Capturar input, mostrar output
Backend: Routing, validación, orquestación
Services: Lógica reutilizable
Data: Persistencia


3. Fail Fast & Loud
javascriptfunction processData(data) {
  // Validar PRIMERO
  if (!data || !data.amount) {
    throw new Error('Data inválida: falta amount');
  }
  
  // Luego procesar
  try {
    // ...lógica
  } catch (error) {
    CLib.logDebugMessage('ERROR en processData', {error, data});
    throw error;  // Re-throw para que caller maneje
  }
}
Por qué: Errores tempranos son más fáciles de debuggear

4. Consistent Error Handling
Todas las funciones CLib retornan:
javascript{
  success: true/false,
  data: {...},        // Si success: true
  error: "mensaje"    // Si success: false
}
Beneficio: Apps pueden manejar errores de manera uniforme

📈 Escalabilidad
Horizontal: Agregar Apps
mermaidgraph LR
    A[Nueva App] -->|Usa| B[CLib v6]
    A -->|Usa| C[AngelStyle v4]
    B --> D[BigQuery]
    C --> E[Users]
Esfuerzo: Bajo (librerías ya existen)

Vertical: Agregar Funcionalidad
mermaidgraph TD
    A[Nueva función necesaria] --> B{¿Dónde?}
    B -->|Lógica compartida| C[Agregar a CLib]
    B -->|UI compartida| D[Agregar a AngelStyle]
    B -->|Específico de app| E[Agregar a app]
    C --> F[Incrementar versión CLib]
    D --> G[Incrementar versión AngelStyle]
    F --> H[Actualizar apps que la usen]
    G --> H

🔮 Roadmap Arquitectónico
Q1 2025 (Actual)

✅ CLib v6.0.0 stable
✅ AngelStyle v4.0.0 base
⚠️ Refactor AngelStyle → AI-Modern Dark Theme
⚠️ Migrar xls2gsheet a arquitectura modular

Q2 2025

🔮 CLib v7.0.0 con módulos separados
🔮 AngelStyle v5.0.0 completo refactored
🔮 CasinoETL como app standalone
🔮 InventoryManager (nueva app)

Q3 2025

🔮 BigQuery ML models
🔮 Dashboard ejecutivo BI
🔮 API Gateway para integraciones externas


🔗 Links Relacionados

📖 Project Structure
📖 CLib API Reference (próximamente)
📖 AngelStyle Components (próximamente)
📖 Data Flow Diagrams (próximamente)
📖 Deployment Architecture (próximamente)


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

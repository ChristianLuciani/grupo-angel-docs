# 📚 CLib API Reference

> Documentación completa de todas las funciones de CLib v6.0.0

---

## 📋 Índice Rápido

### 🔐 [Auth Module](#auth-module)
- [getUserDetails()](#getuserdetails)
- [getUserRole()](#getuserrole)
- [setImpersonatedRole()](#setimpersonatedrole)
- [clearImpersonatedRole()](#clearimpersonatedrole)
- [getLogoutUrl()](#getlogouturl)

### 🛠️ [Services Module](#services-module)
- [logDebugMessage()](#logdebugmessage)
- [getOrCreateMonthlyFolder()](#getorcreateMonthlyfolder)
- [callGenerativeApi()](#callgenerativeapi)
- [insertRowIntoBigQuery()](#insertrowtobigquery)

### 🔄 [ETL Module](#etl-module)
- [detectFileType()](#detectfiletype)
- [transformAxesGamingVoucher()](#transformaxesgamingvoucher)
- [transformAxesTITOBreakdown()](#transformaxestitobreakdown)
- [transformSielconTITO()](#transformsielcontito)
- [transformSielconByPlayer()](#transformsielconbyplayer)

### 📦 [Legacy Module](#legacy-module) ⚠️
- [xls2gsheet()](#xls2gsheet) (deprecated)
- [extraerDatosDeHTML()](#extraerdatosdehtml) (deprecated)

---

## 🔐 Auth Module

Funciones para autenticación y autorización de usuarios.

---

### `getUserDetails()`

Obtiene información completa del usuario autenticado.

#### Sintaxis
```javascript
CLib.getUserDetails()
Parámetros
Ninguno.
Retorno
javascript{
  success: true,
  data: {
    email: "usuario@grupoangel.com",
    name: "Christian Luciani",
    photoUrl: "https://...",
    isAuthenticated: true
  }
}
Ejemplo de Uso
javascriptfunction checkUser() {
  const result = CLib.getUserDetails();
  
  if (!result.success) {
    return ContentService.createTextOutput('Error de autenticación');
  }
  
  const user = result.data;
  Logger.log('Usuario: ' + user.name);
  Logger.log('Email: ' + user.email);
}
Casos de Error
javascript{
  success: false,
  error: "Usuario no autenticado"
}
Notas

✅ Usa Session.getActiveUser() internamente
✅ Cache la respuesta si la llamas múltiples veces
⚠️ Requiere que el usuario esté logueado en Google


getUserRole()
Determina el rol del usuario basado en su email.
Sintaxis
javascriptCLib.getUserRole(email)
Parámetros
NombreTipoRequeridoDescripciónemailStringNoEmail a verificar. Si se omite, usa usuario actual
Retorno
javascript{
  success: true,
  data: {
    role: "admin",  // admin | supervisor | operator
    permissions: {
      canApprove: true,
      canDelete: true,
      canViewReports: true
    }
  }
}
Ejemplo de Uso
javascriptfunction doGet(e) {
  const roleResult = CLib.getUserRole();
  
  if (!roleResult.success) {
    return HtmlService.createHtmlOutput('AccessDenied.html');
  }
  
  const role = roleResult.data.role;
  
  // Cargar interfaz según rol
  switch(role) {
    case 'admin':
      return HtmlService.createHtmlOutput('AdminDashboard.html');
    case 'supervisor':
      return HtmlService.createHtmlOutput('Supervisor.html');
    case 'operator':
      return HtmlService.createHtmlOutput('Operator.html');
    default:
      return HtmlService.createHtmlOutput('AccessDenied.html');
  }
}
Lógica de Roles
javascript// Definida en Config.gs de cada app
const ADMIN_EMAILS = ['admin@grupoangel.com'];
const SUPERVISOR_EMAILS = ['supervisor@grupoangel.com'];
// Resto = operator
Notas

✅ Verifica contra listas definidas en Config.gs
✅ Case-insensitive (normaliza a lowercase)
⚠️ Modo "impersonation" tiene prioridad (ver siguiente función)


setImpersonatedRole()
Simula un rol diferente (solo para testing).
Sintaxis
javascriptCLib.setImpersonatedRole(role)
Parámetros
NombreTipoRequeridoDescripciónroleStringSíRol a simular: 'admin', 'supervisor', 'operator'
Retorno
javascript{
  success: true,
  data: {
    impersonatedRole: "supervisor",
    originalRole: "admin"
  }
}
Ejemplo de Uso
javascript// En AdminDashboard.html
function testAsSupervisor() {
  CLib.setImpersonatedRole('supervisor');
  location.reload();  // Recarga con rol simulado
}

function stopTesting() {
  CLib.clearImpersonatedRole();
  location.reload();
}
Notas

⚠️ SOLO PARA TESTING - No usar en producción
✅ Se guarda en PropertiesService (sesión)
✅ Se limpia automáticamente al cerrar sesión
🔒 Solo admins pueden impersonar

Casos de Error
javascript{
  success: false,
  error: "Solo admins pueden usar impersonation"
}

clearImpersonatedRole()
Limpia el rol simulado y vuelve al rol real.
Sintaxis
javascriptCLib.clearImpersonatedRole()
Parámetros
Ninguno.
Retorno
javascript{
  success: true,
  data: {
    clearedRole: "supervisor",
    currentRole: "admin"
  }
}
Ejemplo de Uso
javascript// Botón "Stop Impersonating" en UI
google.script.run
  .withSuccessHandler(() => location.reload())
  .withFailureHandler((error) => alert('Error: ' + error))
  .clearImpersonatedRole();

getLogoutUrl()
Genera URL de logout de Google.
Sintaxis
javascriptCLib.getLogoutUrl(redirectUrl)
Parámetros
NombreTipoRequeridoDescripciónredirectUrlStringNoURL donde redirigir después de logout
Retorno
javascript{
  success: true,
  data: {
    logoutUrl: "https://accounts.google.com/Logout?continue=..."
  }
}
Ejemplo de Uso
javascript// En main.js.html (AngelStyle)
function handleLogout() {
  google.script.run
    .withSuccessHandler((result) => {
      if (result.success) {
        window.top.location.href = result.data.logoutUrl;
      }
    })
    .getLogoutUrl(window.location.href);
}

🛠️ Services Module
Funciones de servicios generales.

logDebugMessage()
Registra mensajes de debug con contexto.
Sintaxis
javascriptCLib.logDebugMessage(message, context)
Parámetros
NombreTipoRequeridoDescripciónmessageStringSíMensaje principalcontextObjectNoInformación adicional
Retorno
javascript{
  success: true,
  data: {
    logged: true,
    timestamp: "2025-10-09T10:30:00Z"
  }
}
Ejemplo de Uso
javascriptfunction processTicket(ticketData) {
  try {
    CLib.logDebugMessage('Iniciando proceso de ticket', {
      user: Session.getActiveUser().getEmail(),
      ticketId: ticketData.id
    });
    
    // ... lógica de procesamiento
    
    CLib.logDebugMessage('Ticket procesado exitosamente', {
      ticketId: ticketData.id,
      duration: '3.2s'
    });
    
  } catch (error) {
    CLib.logDebugMessage('ERROR en processTicket', {
      error: error.toString(),
      stack: error.stack,
      ticketData: ticketData
    });
    throw error;
  }
}
Formato de Log
[2025-10-09 10:30:00] Iniciando proceso de ticket
Context: {
  "user": "christian@grupoangel.com",
  "ticketId": "TKT-12345"
}
Dónde se Guardan

✅ Logger.log() (visible en Executions)
✅ PropertiesService (últimos 100 logs)
🔮 Futuro: BigQuery logs table


getOrCreateMonthlyFolder()
Crea o encuentra carpeta mensual en Drive.
Sintaxis
javascriptCLib.getOrCreateMonthlyFolder(parentFolderId, prefix)
Parámetros
NombreTipoRequeridoDescripciónparentFolderIdStringSíID de carpeta padreprefixStringNoPrefijo para nombre (ej: 'Tickets')
Retorno
javascript{
  success: true,
  data: {
    folderId: "1ABC...XYZ",
    folderName: "2025-10-Tickets",
    wasCreated: false  // true si se creó nueva
  }
}
Ejemplo de Uso
javascriptfunction saveTicketImage(imageBlob) {
  const UPLOADS_FOLDER_ID = PropertiesService.getScriptProperties()
    .getProperty('UPLOADS_FOLDER_ID');
  
  // Obtener/crear carpeta del mes actual
  const folderResult = CLib.getOrCreateMonthlyFolder(
    UPLOADS_FOLDER_ID,
    'Tickets'
  );
  
  if (!folderResult.success) {
    throw new Error('No se pudo crear carpeta: ' + folderResult.error);
  }
  
  // Guardar imagen en carpeta
  const folder = DriveApp.getFolderById(folderResult.data.folderId);
  const file = folder.createFile(imageBlob);
  
  return file.getId();
}
Estructura Creada
Parent Folder/
└── 2025-10-Tickets/    ← Creada automáticamente
    ├── ticket1.jpg
    ├── ticket2.jpg
    └── ...
Notas

✅ Si carpeta ya existe, retorna su ID
✅ Formato: YYYY-MM-Prefix
✅ Thread-safe (maneja concurrencia)


callGenerativeApi()
Llama a Gemini AI para análisis de texto/imagen.
Sintaxis
javascriptCLib.callGenerativeApi(prompt, imageBase64, options)
Parámetros
NombreTipoRequeridoDescripciónpromptStringSíInstrucciones para el modeloimageBase64StringNoImagen en base64 (para Vision)optionsObjectNoConfiguración adicional
Options
javascript{
  model: 'gemini-2.5-flash',  // Default
  temperature: 0.1,           // 0-1, default 0.1
  maxOutputTokens: 2048,      // Default 2048
  responseFormat: 'json'      // 'json' o 'text'
}
Retorno
javascript{
  success: true,
  data: {
    response: {...},  // JSON parseado si responseFormat='json'
    tokensUsed: 456,
    model: 'gemini-2.5-flash'
  }
}
Ejemplo 1: OCR de Ticket
javascriptfunction analyzeTicket(imageBase64) {
  const prompt = `
    Analiza esta imagen de ticket de casino y extrae:
    - Monto (número decimal)
    - Fecha (formato YYYY-MM-DD)
    - Número de ticket
    - Nombre de casino (si visible)
    
    Retorna JSON con estructura:
    {
      "amount": 100.50,
      "date": "2025-10-08",
      "ticketNumber": "ABC123",
      "casino": "Casino Del Mar"
    }
  `;
  
  const result = CLib.callGenerativeApi(prompt, imageBase64, {
    responseFormat: 'json'
  });
  
  if (!result.success) {
    throw new Error('Error en OCR: ' + result.error);
  }
  
  return result.data.response;
}
Ejemplo 2: Clasificación de Texto
javascriptfunction classifyDocument(text) {
  const prompt = `
    Clasifica el siguiente documento en una de estas categorías:
    - AXES_VOUCHER
    - SIELCON_TITO
    - SIELCON_BY_PLAYER
    - UNKNOWN
    
    Documento:
    ${text}
    
    Retorna solo el nombre de la categoría.
  `;
  
  const result = CLib.callGenerativeApi(prompt, null, {
    responseFormat: 'text'
  });
  
  return result.data.response.trim();
}
Casos de Error
javascript{
  success: false,
  error: "API quota exceeded" // o "Invalid image format", etc.
}
Límites y Cuotas

✅ Google Workspace: 1000 requests/día (corporativo)
⚠️ Imágenes: Max 4MB
⚠️ Timeout: 60 segundos


insertRowIntoBigQuery()
Inserta fila en tabla de BigQuery.
Sintaxis
javascriptCLib.insertRowIntoBigQuery(projectId, datasetId, tableId, row)
Parámetros
NombreTipoRequeridoDescripciónprojectIdStringSíID del proyecto GCPdatasetIdStringSíID del datasettableIdStringSíID de la tablarowObjectSíDatos a insertar
Retorno
javascript{
  success: true,
  data: {
    inserted: true,
    rowId: "abc123xyz"
  }
}
Ejemplo de Uso
javascriptfunction saveTicketToBigQuery(ticketData) {
  const PROJECT_ID = 'grupo-angel';
  const DATASET_ID = 'casino';
  const TABLE_ID = 'tickets';
  
  // Preparar row con schema correcto
  const row = {
    ticket_id: ticketData.id,
    amount: parseFloat(ticketData.amount),
    date: ticketData.date,
    operator_email: Session.getActiveUser().getEmail(),
    status: 'pending',
    ocr_confidence: ticketData.confidence || 0.95,
    created_at: new Date().toISOString()
  };
  
  const result = CLib.insertRowIntoBigQuery(
    PROJECT_ID,
    DATASET_ID,
    TABLE_ID,
    row
  );
  
  if (!result.success) {
    CLib.logDebugMessage('Error insertando a BigQuery', {
      error: result.error,
      row: row
    });
    throw new Error('No se pudo guardar en BigQuery');
  }
  
  return result.data.rowId;
}
Schema Validation
La función valida tipos automáticamente:

✅ Convierte strings a números si el schema lo requiere
✅ Valida fechas en formato ISO 8601
✅ Rechaza campos no definidos en schema

Casos de Error
javascript{
  success: false,
  error: "Schema mismatch: field 'amount' expected NUMERIC, got STRING"
}
Notas

⚠️ Requiere que la tabla ya exista
⚠️ No crea datasets automáticamente
✅ Usa streaming insert (inmediato, no batch)


🔄 ETL Module
Funciones para transformación de datos de casino.

detectFileType()
Detecta el tipo de archivo/sistema de casino.
Sintaxis
javascriptCLib.detectFileType(content)
Parámetros
NombreTipoRequeridoDescripcióncontentStringSíContenido del archivo (HTML o texto)
Retorno
javascript{
  success: true,
  data: {
    type: "AXES_GAMING_VOUCHER",
    confidence: 0.95,
    system: "AXES"
  }
}
Tipos Detectables

AXES_GAMING_VOUCHER
AXES_TITO_BREAKDOWN
SIELCON_TITO
SIELCON_BY_PLAYER
UNKNOWN

Ejemplo de Uso
javascriptfunction processUploadedFile(fileId) {
  const file = DriveApp.getFileById(fileId);
  const content = file.getBlob().getDataAsString();
  
  // Detectar tipo
  const typeResult = CLib.detectFileType(content);
  
  if (!typeResult.success || typeResult.data.type === 'UNKNOWN') {
    throw new Error('Tipo de archivo no soportado');
  }
  
  // Transformar según tipo
  let transformResult;
  switch(typeResult.data.type) {
    case 'AXES_GAMING_VOUCHER':
      transformResult = CLib.transformAxesGamingVoucher(content);
      break;
    case 'SIELCON_TITO':
      transformResult = CLib.transformSielconTITO(content);
      break;
    // ... otros casos
  }
  
  return transformResult;
}
Lógica de Detección
javascript// Busca patrones específicos en HTML
if (content.includes('AXES Gaming')) return 'AXES_GAMING_VOUCHER';
if (content.includes('SIELCON') && content.includes('TITO')) return 'SIELCON_TITO';
// ... más patterns

transformAxesGamingVoucher()
Transforma archivo de vouchers de AXES Gaming.
Sintaxis
javascriptCLib.transformAxesGamingVoucher(htmlContent)
Parámetros
NombreTipoRequeridoDescripciónhtmlContentStringSíHTML del archivo AXES
Retorno
javascript{
  success: true,
  data: {
    type: 'GAMING_VOUCHER',
    system: 'AXES',
    records: [
      {
        date: '2025-10-08',
        time: '14:30:00',
        voucherNumber: 'V123456',
        amount: 100.50,
        machineNumber: 'M001',
        status: 'PAID'
      },
      // ... más records
    ],
    summary: {
      totalRecords: 150,
      totalAmount: 15000.75,
      dateRange: {
        from: '2025-10-01',
        to: '2025-10-08'
      }
    }
  }
}
Ejemplo de Uso
javascriptfunction importAxesVouchers(fileId) {
  const file = DriveApp.getFileById(fileId);
  const html = file.getBlob().getDataAsString();
  
  const result = CLib.transformAxesGamingVoucher(html);
  
  if (!result.success) {
    throw new Error('Error transformando AXES: ' + result.error);
  }
  
  // Insertar cada record a BigQuery
  const PROJECT_ID = 'grupo-angel';
  const DATASET_ID = 'casino';
  const TABLE_ID = 'vouchers';
  
  result.data.records.forEach(record => {
    CLib.insertRowIntoBigQuery(PROJECT_ID, DATASET_ID, TABLE_ID, record);
  });
  
  return result.data.summary;
}

transformAxesTITOBreakdown()
Transforma breakdown de tickets TITO de AXES.
Sintaxis
javascriptCLib.transformAxesTITOBreakdown(htmlContent)
Estructura Similar a transformAxesGamingVoucher()
Campos Específicos
javascript{
  ticketNumber: 'T789012',
  denomination: 25.00,
  quantity: 4,
  subtotal: 100.00,
  // ...
}

transformSielconTITO()
Transforma reportes TITO de SIELCON.
Sintaxis
javascriptCLib.transformSielconTITO(htmlContent)
Retorno
javascript{
  success: true,
  data: {
    type: 'TITO_REPORT',
    system: 'SIELCON',
    records: [
      {
        date: '2025-10-08',
        ticketNumber: 'TITO-12345',
        amount: 50.00,
        machineId: 'SLT-045',
        status: 'REDEEMED'
      }
    ]
  }
}

transformSielconByPlayer()
Transforma reportes por jugador de SIELCON.
Sintaxis
javascriptCLib.transformSielconByPlayer(htmlContent)
Retorno
javascript{
  success: true,
  data: {
    type: 'PLAYER_REPORT',
    system: 'SIELCON',
    records: [
      {
        playerId: 'P12345',
        playerName: 'John Doe',
        totalBets: 1000.00,
        totalWins: 850.00,
        netResult: -150.00,
        sessionsCount: 5
      }
    ]
  }
}

📦 Legacy Module
⚠️ Funciones deprecadas - No usar en código nuevo

xls2gsheet() ⚠️ DEPRECATED
Razón: Lógica duplicada, usar transformSielconTITO() en su lugar
Sintaxis
javascriptCLib.xls2gsheet(fileId)
Migración
javascript// ❌ Viejo (deprecated)
const result = CLib.xls2gsheet(fileId);

// ✅ Nuevo (usar esto)
const file = DriveApp.getFileById(fileId);
const content = file.getBlob().getDataAsString();
const type = CLib.detectFileType(content);
const result = CLib.transformSielconTITO(content);

extraerDatosDeHTML() ⚠️ DEPRECATED
Razón: Reemplazado por funciones ETL específicas
Migración
javascript// ❌ Viejo
const data = CLib.extraerDatosDeHTML(html);

// ✅ Nuevo
const type = CLib.detectFileType(html);
const data = CLib['transform' + type](html);  // Dynamic call

🔧 Mantenimiento de Esta Documentación
Cuándo Actualizar
✅ Agregar Nueva Función

Agrega código a CLib

javascript   /**
    * Nueva función descriptiva
    * @param {string} param1 - Descripción
    * @returns {Object} {success, data/error}
    */
   function nuevaFuncion(param1) {
     // ... código
   }

Actualiza este documento

Agrega entrada en índice
Crea sección completa con ejemplos
Incluye casos de error


Commit

bash   git add human/reference/clib-api.md
   git commit -m "docs(clib): agregar documentación de nuevaFuncion()"
✅ Modificar Función Existente

Actualiza JSDoc en código
Actualiza sección en este doc
Agrega nota de cambio:

markdown   > **Actualizado en v6.1.0**: Ahora acepta parámetro opcional `timeout`
✅ Deprecar Función

Marca en código:

javascript   /**
    * @deprecated Usar nuevaFuncion() en su lugar
    */

Mueve a sección Legacy
Agrega guía de migración

Template para Nueva Función
markdown### `nombreFuncion()`

Descripción breve de qué hace.

#### Sintaxis
\```javascript
CLib.nombreFuncion(param1, param2)
\```

#### Parámetros
| Nombre | Tipo | Requerido | Descripción |
|--------|------|-----------|-------------|
| `param1` | String | Sí | Qué es este parámetro |

#### Retorno
\```javascript
{
  success: true,
  data: {
    // estructura de respuesta
  }
}
\```

#### Ejemplo de Uso
\```javascript
function ejemploUso() {
  const result = CLib.nombreFuncion('valor');
  // ... usar result
}
\```

#### Casos de Error
\```javascript
{
  success: false,
  error: "Descripción del error"
}
\```

#### Notas
- ✅ Nota importante 1
- ⚠️ Advertencia importante
Checklist Pre-Commit
Antes de hacer commit de cambios a este doc:

 JSDoc actualizado en código fuente
 Índice actualizado
 Ejemplos funcionan (probados)
 Casos de error documentados
 Links internos funcionan
 Formato consistente con otras funciones


🔗 Links Relacionados

📖 System Overview
📖 Creating New App (próximamente)
📖 AngelStyle API Reference (próximamente)
💻 CLib Source Code


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)
Versión CLib: v6.0.0

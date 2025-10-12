
🔌 External Integrations Architecture

Estrategia y patrones para integraciones con APIs y servicios externos


🎯 Filosofía
Apps Script es poderoso pero limitado. Para extender funcionalidad:
Principios Core

API REST First - Preferir APIs REST sobre librerías npm
Wrappers Consistentes - Todo en XUtil con formato estándar
Fail Gracefully - Siempre tener fallback
Cache Agresivo - Respetar rate limits
Security First - Secrets en PropertiesService


📊 Criterios de Selección de APIs
Checklist de Evaluación
Use esta checklist ANTES de integrar una API externa:
```markdown
API Evaluation: [Nombre de API]
Requisitos Técnicos

 ✅ API REST bien documentada
 ✅ HTTPS (seguro)
 ✅ Response en JSON
 ✅ Error codes claros (HTTP status)
 ✅ Stable (no beta)

Autenticación

 Sin auth (público) → ⭐⭐⭐ Ideal
 API Key simple → ⭐⭐ Bueno
 OAuth 2.0 → ⭐ Aceptable
 OAuth complejo → ❌ Evitar

Rate Limits

 >100 requests/día → ✅ Suficiente
 10-100 requests/día → ⚠️ Limitado
 <10 requests/día → ❌ Insuficiente

Costo

 Gratis → ⭐⭐⭐
 Freemium con tier gratis → ⭐⭐
 Solo pago → ⭐ (evaluar ROI)

Confiabilidad

 SLA >99% → ✅
 Sin SLA formal → ⚠️ (si open source puede ser OK)
 Downtime frecuente → ❌

Soporte

 Documentación completa → ✅
 Ejemplos de código → ✅
 Community activa → ✅
 Support tickets → ⭐ Bonus

VEREDICTO: [APROBADA / RECHAZADA / EVALUAR MÁS]
```

🏗️ Arquitectura de XUtil
Estructura de Librería
```
libraries/XUtil/
├── Code.gs                 # Entry point, main wrapper
├── ProductAPIs.gs          # Product lookup APIs
├── ZohoIntegration.gs      # Zoho Books/Inventory
├── ImageProcessing.gs      # Image download/upload
├── CacheManager.gs         # Caching logic
├── ErrorHandling.gs        # Error wrappers
├── Config.gs               # API endpoints, keys
├── README.md
└── CURRENT_STATE.md
```
Pattern de Wrapper
TODO wrapper function debe seguir este pattern:
```javascript
/**

[Descripción de la función]
@param {type} param - Descripción
@returns {Object} {success: boolean, data/error}
*/
function wrapperFunction(param) {
try {
// 1. Validar input
if (!param) {
return {success: false, error: 'Parámetro requerido'};
}
// 2. Check cache (si aplica)
const cached = checkCache(cacheKey);
if (cached) return {success: true, data: cached};
// 3. Llamar API externa
const url = buildUrl(param);
const options = buildOptions();
const response = UrlFetchApp.fetch(url, options);
// 4. Parse response
const data = JSON.parse(response.getContentText());
// 5. Transform a formato estándar
const normalized = normalizeData(data);
// 6. Cache result (si aplica)
saveCache(cacheKey, normalized);
// 7. Return success
return {success: true, data: normalized};

} catch (error) {
// 8. Log error
CLib.logDebugMessage('Error en wrapperFunction', {error, param});
// 9. Try fallback (si existe)
if (hasFallback) {
  return fallbackFunction(param);
}

// 10. Return error
return {success: false, error: error.toString()};
}
}
```

🔄 Caching Strategy
Cuándo Cachear
```javascript
// Cache si:
// - Data cambia poco (product info)
// - Rate limits estrictos
// - Latency alta de API
// NO cache si:
// - Data en tiempo real (stock, precios dinámicos)
// - Data sensible (tokens, passwords)
```
Implementación
```javascript
// CacheManager.gs
function getCached(key, ttl = 3600) {
const cache = CacheService.getScriptCache();
const cached = cache.get(key);
if (cached) {
const data = JSON.parse(cached);
const age = Date.now() - data.timestamp;
if (age < ttl * 1000) {
  return data.value;
}
}
return null;
}
function saveCache(key, value, ttl = 3600) {
const cache = CacheService.getScriptCache();
const data = {
value: value,
timestamp: Date.now()
};
cache.put(key, JSON.stringify(data), ttl);
}
```

🔒 Secrets Management
NUNCA Hardcodear Secrets
```javascript
// ❌ MAL
const ZOHO_TOKEN = 'abc123xyz';
// ✅ BIEN
const ZOHO_TOKEN = PropertiesService.getScriptProperties()
.getProperty('ZOHO_ACCESS_TOKEN');
```
Setup de Secrets
```bash
En Apps Script Editor:
Project Settings → Script Properties → Add
Key: ZOHO_ACCESS_TOKEN
Value: [tu token]
Key: ZOHO_ORG_ID
Value: [tu org id]
```
Rotación de Secrets
```markdown
Proceso de Rotación (Cada 90 días)

Generar nuevo token en Zoho
Agregar como ZOHO_ACCESS_TOKEN_NEW
Testear con nuevo token
Si OK:

Eliminar ZOHO_ACCESS_TOKEN (viejo)
Renombrar ZOHO_ACCESS_TOKEN_NEW → ZOHO_ACCESS_TOKEN


Revocar token viejo en Zoho
```


📊 APIs Aprobadas
Production-Ready
APIVersiónUsoAuthRate LimitDocsOpen Food Factsv0Product lookupNone∞LinkZoho Booksv3AccountingOAuth 2.01000/díaLinkZoho Inventoryv1InventoryOAuth 2.01000/díaLink
Under Evaluation
APIPropósitoStatusNotasGoogle ShoppingProduct search🟡 EvaluandoRequiere Merchant accountUPC DatabaseBarcode lookup🟡 EvaluandoFreemium, 100 req/día gratis
Rejected
APIRazón de RechazoAlternativaAmazon Product APIOAuth complejoOpen Food FactsBarcodeLookupSolo 100 req/mes gratisUPC Database

🔧 Mantenimiento
Checklist Mensual

 Verificar que todas las APIs funcionan
 Revisar logs de errores
 Verificar rate limits no excedidos
 Rotar secrets si necesario
 Actualizar documentación si cambió algo

Cuando una API Falla
```markdown
Incident Response: API Down

Detectar: Monitoring o usuario reporta
Verificar: Test manual de API
Activar Fallback: Si existe fallback, activar
Notificar: Informar a usuarios afectados
Monitor: Revisar cada hora hasta recuperación
Post-Mortem: Documentar incident
```


🔗 Links Relacionados

📖 XUtil API Reference
📖 Adding External API Guide
📖 System Overview


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)

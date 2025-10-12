# 🔌 XUtil API Reference

> Documentación completa de la librería XUtil (External Utilities)

---

## 🎯 Objetivo

XUtil maneja integraciones con APIs y servicios externos, manteniendo código limpio y consistente.

---

## 📋 Índice Rápido

### 🛒 [Product APIs](#product-apis)
- [searchOpenFoodFacts()](#searchopenfoodfacts) - Búsqueda por barcode
- [searchGoogleShopping()](#searchgoogleshopping) - Búsqueda alternativa
- [enrichProductInfo()](#enrichproductinfo) - Enriquecer con AI

### 📊 [Zoho Integration](#zoho-integration)
- [createZohoItem()](#createzohoitem) - Crear artículo
- [updateZohoItem()](#updatezohoitem) - Actualizar artículo
- [getZohoCategories()](#getzohocategories) - Listar categorías

### 🖼️ [Image Processing](#image-processing)
- [downloadImage()](#downloadimage) - Descargar imagen
- [optimizeImage()](#optimizeimage) - Optimizar tamaño
- [uploadToDrive()](#uploadtodrive) - Subir a Drive

---

## 🛒 Product APIs

### `searchOpenFoodFacts()`

Busca producto en base de datos Open Food Facts por código de barras.

#### Sintaxis
\```javascript
XUtil.searchOpenFoodFacts(barcode)
\```

#### Parámetros
| Nombre | Tipo | Requerido | Descripción |
|--------|------|-----------|-------------|
| `barcode` | String | Sí | Código UPC/EAN (8-13 dígitos) |

#### Retorno
\```javascript
{
  success: true,
  data: {
    name: "Coca-Cola Original",
    brand: "Coca-Cola",
    category: "Bebidas",
    image: "https://...",
    nutrition: {...}
  }
}
\```

#### Ejemplo de Uso
\```javascript
function lookupProduct() {
  const barcode = '5449000000996'; // Coca-Cola
  const result = XUtil.searchOpenFoodFacts(barcode);
  
  if (result.success) {
    Logger.log('Producto: ' + result.data.name);
    Logger.log('Marca: ' + result.data.brand);
  } else {
    Logger.log('Error: ' + result.error);
  }
}
\```

#### Casos de Error
- Barcode inválido: `{success: false, error: "Barcode debe tener 8-13 dígitos"}`
- Producto no encontrado: `{success: false, error: "Product not found"}`
- API no disponible: `{success: false, error: "API request failed"}`

#### Notas
- ✅ API gratuita, sin límites
- ✅ Base de datos global (700k+ productos)
- ⚠️ Mejor cobertura en Europa
- 💡 Usar como primera opción

---

## 📊 Zoho Integration

### `createZohoItem()`

Crea nuevo artículo en Zoho Books/Inventory.

#### Sintaxis
\```javascript
XUtil.createZohoItem(itemData)
\```

#### Parámetros
| Nombre | Tipo | Requerido | Descripción |
|--------|------|-----------|-------------|
| `itemData` | Object | Sí | Datos del artículo |

#### itemData Structure
\```javascript
{
  name: "Coca-Cola 500ml",
  description: "Bebida gaseosa",
  rate: 2.50,              // Precio
  category: "Bebidas",
  unit: "unidad",
  account_id: "123456",    // Zoho account ID
  image_url: "https://..." // Opcional
}
\```

#### Retorno
\```javascript
{
  success: true,
  data: {
    item_id: "982000000567001",
    item_name: "Coca-Cola 500ml",
    created_time: "2025-10-11T10:30:00Z"
  }
}
\```

#### Ejemplo de Uso
\```javascript
function createProduct(productInfo) {
  const itemData = {
    name: productInfo.name,
    description: productInfo.description,
    rate: productInfo.price,
    category: "Bebidas",
    unit: "unidad"
  };
  
  const result = XUtil.createZohoItem(itemData);
  
  if (result.success) {
    Logger.log('✅ Item creado con ID: ' + result.data.item_id);
    return result.data.item_id;
  } else {
    throw new Error('Error creando item: ' + result.error);
  }
}
\```

#### Casos de Error
- Auth inválido: `{success: false, error: "Invalid Zoho token"}`
- Datos incompletos: `{success: false, error: "Missing required field: name"}`
- Item duplicado: `{success: false, error: "Item already exists"}`

#### Notas
- ⚠️ Requiere Zoho OAuth token válido
- ⚠️ Token debe refrescarse cada hora
- 💡 Rate limit: 1000 requests/día

---

## 🖼️ Image Processing

### `downloadImage()`

Descarga imagen desde URL y la guarda en Drive.

#### Sintaxis
\```javascript
XUtil.downloadImage(imageUrl, fileName)
\```

#### Parámetros
| Nombre | Tipo | Requerido | Descripción |
|--------|------|-----------|-------------|
| `imageUrl` | String | Sí | URL completa de la imagen |
| `fileName` | String | No | Nombre personalizado (opcional) |

#### Retorno
\```javascript
{
  success: true,
  data: {
    fileId: "1ABC...XYZ",
    fileName: "product-12345.jpg",
    fileUrl: "https://drive.google.com/...",
    thumbnailUrl: "https://..."
  }
}
\```

#### Ejemplo de Uso
\```javascript
function saveProductImage(barcode, imageUrl) {
  const fileName = `product-${barcode}.jpg`;
  const result = XUtil.downloadImage(imageUrl, fileName);
  
  if (result.success) {
    Logger.log('✅ Imagen guardada: ' + result.data.fileUrl);
    return result.data;
  }
}
\```

---

## 🔧 Mantenimiento de Esta Documentación

### Cuándo Actualizar

#### ✅ **Cuando se agregue nueva API**
1. Usar comando: `/agregar-api [nombre]` del GEM
2. GEM generará sección completa
3. Insertar en orden alfabético

#### ✅ **Cuando cambie API existente**
1. Actualizar sección específica
2. Agregar nota:
```markdown
   > **Actualizado 2025-XX-XX**: [Descripción del cambio]

Actualizar ejemplos si necesario

✅ Periódicamente (cada 3 meses)

Verificar que todas las APIs siguen funcionando
Actualizar rate limits si cambiaron
Revisar casos de error comunes

Template para Nueva API
```markdown
nombreFuncion()
Descripción breve.
Sintaxis
```javascript
XUtil.nombreFuncion(param1, param2)
```
Parámetros
[Tabla de parámetros]
Retorno
[Estructura de respuesta]
Ejemplo de Uso
[Código funcional]
Casos de Error
[Lista de errores posibles]
Notas
[Consideraciones importantes]
```
Checklist Pre-Commit

 Función testeada y funcional
 JSDoc completo en código
 Ejemplos funcionan (copy-paste)
 Casos de error documentados
 Rate limits actualizados
 Links a API externa (si aplica)


🔗 Links Relacionados

📖 CLib API
📖 AngelStyle API
📖 External Integrations Architecture
📖 Adding External API Guide


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)
Versión XUtil: v0.1.0 (en desarrollo)

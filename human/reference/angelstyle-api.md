# 🎨 AngelStyle API Reference

> Documentación completa de componentes UI/UX v4.0.0 y AI-Modern Dark Theme

---

## 📋 Índice Rápido

### 🎨 [Paleta de Colores](#paleta-de-colores)
### 🔤 [Tipografía](#tipografía)
### 🧩 [Componentes v4.0.0 (Actual)](#componentes-v40-actual)
- [Buttons](#buttons)
- [Loading](#loading)
- [Modals](#modals)
- [Utilities](#utilities)

### ✨ [Componentes v5.0.0 (AI-Modern Dark)](#componentes-v50-ai-modern-dark)
- [Layout](#layout)
- [Forms](#forms)
- [Cards](#cards)
- [Effects](#effects)

### 🔧 [JavaScript Helpers](#javascript-helpers)
### 📱 [Responsive Breakpoints](#responsive-breakpoints)
### 🛠️ [Cómo Usar AngelStyle](#cómo-usar-angelstyle)

---

## 🎨 Paleta de Colores

### Colores Corporativos (AI-Modern Dark Theme)
```css
:root {
  /* Verdes Grupo Angel */
  --angel-green-dark: #1a5f3f;      /* Primario corporativo */
  --angel-green-medium: #2d8659;    /* Hover states */
  --angel-green-light: #4a9d6f;     /* Borders, accents */
  
  /* Acento distintivo */
  --angel-lime: #a8b446;            /* CTAs secundarios */
  
  /* Tech/AI */
  --angel-cyan: #00d9ff;            /* Elementos tech, AI indicators */
  
  /* Backgrounds */
  --bg-primary: #0a0f0d;            /* Fondo principal */
  --bg-secondary: #151a17;          /* Cards, panels */
  --bg-tertiary: #1f2621;           /* Inputs, modals */
  
  /* Text */
  --text-primary: #e8f5e9;          /* Texto principal */
  --text-secondary: #a5d6a7;        /* Texto secundario */
  --text-muted: #66bb6a;            /* Placeholders, hints */
  
  /* Status */
  --success: #4caf50;
  --warning: #ff9800;
  --error: #f44336;
  --info: var(--angel-cyan);
  
  /* Glassmorphism */
  --glass-bg: rgba(26, 95, 63, 0.1);
  --glass-border: rgba(74, 157, 111, 0.2);
  
  /* Shadows & Glows */
  --glow-green: 0 0 20px rgba(26, 95, 63, 0.5);
  --glow-cyan: 0 0 20px rgba(0, 217, 255, 0.5);
  --shadow-soft: 0 4px 20px rgba(0, 0, 0, 0.3);
}```

### Uso de Colores (Neuro Accesibilidad: OK)

| Color | Uso Recomendado | Ejemplo |
| :--- | :--- | :--- |
| `--angel-green-dark` | Botones primarios, headers | `Login button, nav bar` |
| `--angel-green-medium` | Hover states, focus | `Button hover` |
| `--angel-lime` | CTAs secundarios, badges | `"Ver más", status badges` |
| `--angel-cyan` | Indicadores AI/tech, links | `"Procesando con AI..."` |
| `--bg-primary` | Fondo de página | `body background` |
| `--bg-secondary` | Cards, contenedores | `.angel-card` |


### 🔤 Tipografía
Fuente: **Lexend**

```html
<!-- Incluir en <head> -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
```

```css
body {
  font-family: 'Lexend', sans-serif;
  font-weight: 400;
  font-size: 16px;
  line-height: 1.6;
  color: var(--text-primary);
}
```

**Jerarquía Tipográfica**

```css
h1 {
  font-size: 2.5rem;      /* 40px */
  font-weight: 700;
  letter-spacing: -0.02em;
}

h2 {
  font-size: 2rem;        /* 32px */
  font-weight: 600;
}

h3 {
  font-size: 1.5rem;      /* 24px */
  font-weight: 600;
}

h4 {
  font-size: 1.25rem;     /* 20px */
  font-weight: 500;
}

p, .body-text {
  font-size: 1rem;        /* 16px */
  font-weight: 400;
}

small, .small-text {
  font-size: 0.875rem;    /* 14px */
  font-weight: 300;
}
```

**Por Qué Lexend**

- ✅ Diseñada para legibilidad (ideal para dislexia)
- ✅ Espaciado generoso entre letras 
- ✅ Formas de letras distintivas 
- ✅ Variable font (weights 300-800) 
- ✅ Moderna y profesional 


### 🧩 Componentes v4.0.0 (Actual)
⚠️ Nota: Estos son los componentes actuales. Ver sección v5.0.0 para nuevos componentes.

**Buttons**

`.btn` (Base)

```css
.btn {
  display: inline-block;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: 500;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
}
```

```html
<button class="btn">Base Button</button>
```

**`.btn-primary`**

```css
.btn-primary {
  background-color: #2e7d32;
  color: white;
}

.btn-primary:hover {
  background-color: #1b5e20;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
```

```html
<button class="btn btn-primary">Guardar</button>
```
Uso: Acción principal (guardar, enviar, confirmar)

**`.btn-secondary`**

```css
.btn-secondary {
  background-color: #757575;
  color: white;
}
```

```html
<button class="btn btn-secondary">Cancelar</button>```
Uso: Acción secundaria (cancelar, volver)

**`.btn-success`**
```css
.btn-success {
  background-color: #4caf50;
  color: white;
}
```

```html
<button class="btn btn-success">Aprobar</button>
```
Uso: Confirmación positiva (aprobar, aceptar)

**`.btn-danger`**
```css
.btn-danger {
  background-color: #f44336;
  color: white;
}
```

```html
<button class="btn btn-danger">Rechazar</button>
```
Uso: Acción destructiva (eliminar, rechazar)


**Loading**

`.spinner-circle`

```css
.spinner-circle {
  border: 4px solid rgba(255,255,255,0.3);
  border-top: 4px solid #4caf50;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 20px auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
```

```html
<div class="spinner-circle"></div>
```
Uso: Indicador de carga durante procesamiento

**Modals**

`.modal-bg`

```css
.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}
```

```html
<div class="modal-bg hidden" id="myModal">
  <div style="background: white; padding: 20px; border-radius: 8px;">
    <h3>Título del Modal</h3>
    <p>Contenido...</p>
    <button class="btn btn-primary" onclick="closeModal()">Cerrar</button>
  </div>
</div>
```

**JavaScript:**

```javascript
function showModal() {
  document.getElementById('myModal').classList.remove('hidden');
}

function closeModal() {
  document.getElementById('myModal').classList.add('hidden');
}
```

**Utilities**

`.hidden`

```css
.hidden {
  display: none !important;
}
```

```html
<div class="spinner-circle hidden" id="loading"></div>

<script>
  // Mostrar
  document.getElementById('loading').classList.remove('hidden');
  
  // Ocultar
  document.getElementById('loading').classList.add('hidden');
</script>
```

### ✨ Componentes v5.0.0 (AI-Modern Dark)
⚠️ Estado: En desarrollo - Templates listos para implementar

**Layout**

`.angel-container`

```css
.angel-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background: var(--bg-primary);
  min-height: 100vh;
}
```

```html
<div class="angel-container">
  <!-- Contenido de la app -->
</div>```
Uso: Wrapper principal de cada página

**`.angel-header`**

```css
.angel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid var(--glass-border);
  margin-bottom: 30px;
}
```

```html
<header class="angel-header">
  <div class="angel-logo-container">
    <h1>Grupo Angel</h1>
  </div>
  <nav>
    <button class="angel-button angel-button-ghost" onclick="handleLogout()">
      Salir
    </button>
  </nav>
</header>
```

**Forms**

`.angel-input`

```css
.angel-input {
  width: 100%;
  padding: 12px 16px;
  background: var(--bg-tertiary);
  border: 1px solid var(--glass-border);
  border-radius: 8px;
  color: var(--text-primary);
  font-family: 'Lexend', sans-serif;
  font-size: 16px;
  transition: all 0.3s ease;
}
.angel-input:focus {
  outline: none;
  border-color: var(--angel-green-light);
  box-shadow: var(--glow-green);
}
.angel-input::placeholder {
  color: var(--text-muted);
}
```

**`.angel-button`**

```css
.angel-button {
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  font-family: 'Lexend', sans-serif;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.angel-button-primary {
  background: linear-gradient(135deg, var(--angel-green-dark), var(--angel-green-medium));
  color: white;
}
.angel-button-primary:hover {
  transform: translateY(-2px);
  box-shadow: var(--glow-green), var(--shadow-soft);
}
.angel-button-secondary {
  background: transparent;
  border: 1px solid var(--angel-lime);
  color: var(--angel-lime);
}
.angel-button-ghost {
  background: transparent;
  color: var(--text-secondary);
}
```

**Cards**

`.angel-card`

```css
.angel-card {
  background: var(--bg-secondary);
  border: 1px solid var(--glass-border);
  border-radius: 12px;
  padding: 20px;
  transition: all 0.3s ease;
}
.angel-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-soft);
  border-color: var(--angel-green-light);
}
.angel-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--glass-border);
}
.angel-card-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text-primary);
}
.angel-card-body {
  color: var(--text-secondary);
  line-height: 1.6;
}
```

**Effects**

`.angel-glass`

```css
.angel-glass {
  background: var(--glass-bg);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid var(--glass-border);
  border-radius: 16px;
  padding: 24px;
}
```

`.angel-badge`

```css
.angel-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
.angel-badge-success {
  background: rgba(76, 175, 80, 0.2);
  color: var(--success);
  border: 1px solid var(--success);
}
.angel-badge-warning {
  background: rgba(255, 152, 0, 0.2);
  color: var(--warning);
  border: 1px solid var(--warning);
}
.angel-badge-error {
  background: rgba(244, 67, 54, 0.2);
  color: var(--error);
  border: 1px solid var(--error);
}
.angel-badge-info {
  background: rgba(0, 217, 255, 0.2);
  color: var(--angel-cyan);
  border: 1px solid var(--angel-cyan);
}
```


### 🔧 JavaScript Helpers
Funciones JS incluidas en AngelStyle.

**handleLogout()**

```javascript
function handleLogout() {
  if (!confirm('¿Seguro que deseas salir?')) return;
  
  google.script.run
    .withSuccessHandler((result) => {
      if (result.success) {
        window.top.location.href = result.data.logoutUrl;
      }
    })
    .withFailureHandler((error) => {
      alert('Error al cerrar sesión: ' + error);
    })
    .getLogoutUrl(window.location.href);
}
```
Uso en HTML:
```html
<button class="angel-button angel-button-ghost" onclick="handleLogout()">
  Salir
</button>
```

**checkImpersonation()**

```javascript
function checkImpersonation() {
  google.script.run
    .withSuccessHandler((result) => {
      if (result.success && result.data.isImpersonating) {
        showImpersonationBanner(result.data);
      }
    })
    .getUserRole();
}

function showImpersonationBanner(data) {
  const banner = document.createElement('div');
  banner.className = 'impersonation-banner';
  banner.innerHTML = `
    <p>⚠️ Simulando rol: <strong>${data.impersonatedRole}</strong></p>
    <button onclick="stopImpersonating()">Detener Simulación</button>
  `;
  document.body.prepend(banner);
}
```

### 📱 Responsive Breakpoints

```css
/* Mobile First Approach */

/* Base: Mobile (< 768px) */
.angel-container { padding: 16px; }

/* Tablet (≥ 768px) */
@media (min-width: 768px) { .angel-container { padding: 24px; } }

/* Desktop (≥ 1024px) */
@media (min-width: 1024px) { .angel-container { padding: 32px; } }
```

**Grid Responsive**

```css
.angel-grid {
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr;
}
@media (min-width: 768px) { .angel-grid { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 1024px) { .angel-grid { grid-template-columns: repeat(3, 1fr); } }
```

### 🛠️ Cómo Usar AngelStyle

**Método 1: En Apps Script**

1. Agregar librería al proyecto

```json
// appsscript.json
{
  "dependencies": {
    "libraries": [
      {
        "userSymbol": "AngelStyle",
        "version": "4",
        "libraryId": "19iLSI3Y48FhoBaL9EwMbktyIYBOLEEMC1hePYoMemm7KkCsJ3vY7eS5S",
        "developmentMode": false
      }
    ]
  }
}
```

2. Incluir en HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- ... (metas) ... -->
    
    <!-- Incluir AngelStyle -->
    <?!= AngelStyle.getStyles() ?>
  </head>
  <body>
    <div class="angel-container">
      <button class="angel-button angel-button-primary">
        Hola Mundo
      </button>
    </div>
  </body>
</html>
```

*... (resto del Método 1 y Método 2) ...*


### 🎯 Ejemplos Completos

**Ejemplo 1: Formulario de Upload**

```html
<!DOCTYPE html>
<html>
<head>
  <?!= AngelStyle.getStyles() ?>
</head>
<body>
  <div class="angel-container">
    <!-- ... (Header, Upload Area, Spinner, Preview Div) ... -->
    
    <div id="preview" class="angel-card hidden">
        <!-- ... (Inputs) ... -->
        <div style="margin-top: 20px; display: flex; gap: 10px;">
          <button class="angel-button angel-button-primary" onclick="submitTicket()">Confirmar</button>
        </div>
    </div>
  </div>
  
  <script>
    function handleFile(e) { 
      const file = e.target.files;
      if (!file) return;
      
      // Mostrar loading
      document.getElementById('loading').classList.remove('hidden');
      
      // Leer como base64
      const reader = new FileReader();
      reader.onload = function(event) {
        const base64 = event.target.result.split(',');
        
        // Llamar backend
        google.script.run
          .withSuccessHandler(showPreview)
          .withFailureHandler(showError)
          .analyzeTicket(base64);
      };
      reader.readAsDataURL(file);
    }

    function showPreview(result) {
      document.getElementById('loading').classList.add('hidden');
      
      if (result.success) {
        const data = result.data;
        document.getElementById('amount').value = data.amount;
        document.getElementById('date').value = data.date;
        document.getElementById('ticketNumber').value = data.ticketNumber;
        document.getElementById('preview').classList.remove('hidden');
      } else {
        showError(result.error);
      }
    }
    
    function showError(error) {
      document.getElementById('loading').classList.add('hidden');
      alert('Error: ' + error);
    }
    
    function submitTicket() {
      const data = { 
        amount: document.getElementById('amount').value,
        date: document.getElementById('date').value,
        ticketNumber: document.getElementById('ticketNumber').value
      };
      
      // FIX CRÍTICO APLICADO (LLM Artifact Removed)
      google.script.run
        .withSuccessHandler((result) => {
          if (result.success) {
            alert('✅ Ticket guardado exitosamente');
            location.reload();
          } else {
            alert('❌ Error: ' + result.error);
          }
        })
        .submitTicket(data);
    }
  </script>
</body>
</html>
```

**Ejemplo 2: Dashboard de Supervisor**
(Se incluye el ejemplo del Dashboard que usa los badges, el header y las tarjetas).

### 🔧 Mantenimiento de Esta Documentación

*   Agregar Nuevo Componente: Modificar CSS, HTML de ejemplo, JS si aplica.
*   Deprecar Componente: Marcar en CSS, advertir en Doc.

### 🔗 Links Relacionados

📖 [System Overview](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/architecture/system-overview.md)
📖 [CLib API Reference](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/reference/clib-api.md)
📖 [Creating New App](https://github.com/ChristianLuciani/grupo-angel-docs/blob/main/human/guides/creating-new-app.md)
💻 AngelStyle Source Code
🎨 Lexend Font

Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)
Versión AngelStyle: v4.0.0 (v5.0.0 en desarrollo)

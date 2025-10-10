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
}
Uso de Colores
ColorUso RecomendadoEjemplo--angel-green-darkBotones primarios, headersLogin button, nav bar--angel-green-mediumHover states, focusButton hover--angel-limeCTAs secundarios, badges"Ver más", status badges--angel-cyanIndicadores AI/tech, links"Procesando con AI..."--bg-primaryFondo de páginabody background--bg-secondaryCards, contenedores.angel-card

🔤 Tipografía
Fuente: Lexend
html<!-- Incluir en <head> -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
cssbody {
  font-family: 'Lexend', sans-serif;
  font-weight: 400;
  font-size: 16px;
  line-height: 1.6;
  color: var(--text-primary);
}
Jerarquía Tipográfica
cssh1 {
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
Por Qué Lexend

✅ Diseñada para legibilidad (ideal para dislexia)
✅ Espaciado generoso entre letras
✅ Formas de letras distintivas
✅ Variable font (weights 300-800)
✅ Moderna y profesional


🧩 Componentes v4.0.0 (Actual)
⚠️ Nota: Estos son los componentes actuales. Ver sección v5.0.0 para nuevos componentes.

Buttons
.btn (Base)
css.btn {
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
html<button class="btn">Base Button</button>

.btn-primary
css.btn-primary {
  background-color: #2e7d32;
  color: white;
}

.btn-primary:hover {
  background-color: #1b5e20;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
html<button class="btn btn-primary">Guardar</button>
Uso: Acción principal (guardar, enviar, confirmar)

.btn-secondary
css.btn-secondary {
  background-color: #757575;
  color: white;
}
html<button class="btn btn-secondary">Cancelar</button>
Uso: Acción secundaria (cancelar, volver)

.btn-success
css.btn-success {
  background-color: #4caf50;
  color: white;
}
html<button class="btn btn-success">Aprobar</button>
Uso: Confirmación positiva (aprobar, aceptar)

.btn-danger
css.btn-danger {
  background-color: #f44336;
  color: white;
}
html<button class="btn btn-danger">Rechazar</button>
Uso: Acción destructiva (eliminar, rechazar)

Loading
.spinner-circle
css.spinner-circle {
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
html<div class="spinner-circle"></div>
Uso: Indicador de carga durante procesamiento

Modals
.modal-bg
css.modal-bg {
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
html<div class="modal-bg hidden" id="myModal">
  <div style="background: white; padding: 20px; border-radius: 8px;">
    <h3>Título del Modal</h3>
    <p>Contenido...</p>
    <button class="btn btn-primary" onclick="closeModal()">Cerrar</button>
  </div>
</div>
JavaScript:
javascriptfunction showModal() {
  document.getElementById('myModal').classList.remove('hidden');
}

function closeModal() {
  document.getElementById('myModal').classList.add('hidden');
}

Utilities
.hidden
css.hidden {
  display: none !important;
}
html<div class="spinner-circle hidden" id="loading"></div>

<script>
  // Mostrar
  document.getElementById('loading').classList.remove('hidden');
  
  // Ocultar
  document.getElementById('loading').classList.add('hidden');
</script>

✨ Componentes v5.0.0 (AI-Modern Dark)
⚠️ Estado: En desarrollo - Templates listos para implementar

Layout
.angel-container
css.angel-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background: var(--bg-primary);
  min-height: 100vh;
}
html<div class="angel-container">
  <!-- Contenido de la app -->
</div>
Uso: Wrapper principal de cada página

.angel-header
css.angel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid var(--glass-border);
  margin-bottom: 30px;
}
html<header class="angel-header">
  <div class="angel-logo-container">
    <h1>Grupo Angel</h1>
  </div>
  <nav>
    <button class="angel-button angel-button-ghost" onclick="handleLogout()">
      Salir
    </button>
  </nav>
</header>

Forms
.angel-input
css.angel-input {
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
html<input type="text" 
       class="angel-input" 
       placeholder="Ingresa el monto...">

.angel-button
css.angel-button {
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
html<!-- Primario -->
<button class="angel-button angel-button-primary">
  Procesar Ticket
</button>

<!-- Secundario -->
<button class="angel-button angel-button-secondary">
  Ver Historial
</button>

<!-- Ghost -->
<button class="angel-button angel-button-ghost">
  Cancelar
</button>

.angel-upload-area
css.angel-upload-area {
  border: 2px dashed var(--glass-border);
  border-radius: 12px;
  padding: 40px;
  text-align: center;
  background: var(--glass-bg);
  cursor: pointer;
  transition: all 0.3s ease;
}

.angel-upload-area:hover {
  border-color: var(--angel-green-light);
  background: rgba(26, 95, 63, 0.2);
  box-shadow: var(--glow-green);
}

.angel-upload-area.dragging {
  border-color: var(--angel-cyan);
  box-shadow: var(--glow-cyan);
}
html<div class="angel-upload-area" 
     id="uploadArea"
     ondrop="handleDrop(event)"
     ondragover="handleDragOver(event)"
     ondragleave="handleDragLeave(event)">
  <p>📸 Arrastra foto de ticket aquí</p>
  <p class="small-text">o haz click para seleccionar</p>
  <input type="file" 
         id="fileInput" 
         accept="image/*" 
         style="display:none"
         onchange="handleFileSelect(event)">
</div>

<script>
function handleDragOver(e) {
  e.preventDefault();
  e.currentTarget.classList.add('dragging');
}

function handleDragLeave(e) {
  e.currentTarget.classList.remove('dragging');
}

function handleDrop(e) {
  e.preventDefault();
  e.currentTarget.classList.remove('dragging');
  const files = e.dataTransfer.files;
  processFile(files[0]);
}
</script>

Cards
.angel-card
css.angel-card {
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
html<div class="angel-card">
  <div class="angel-card-header">
    <h3 class="angel-card-title">Ticket #12345</h3>
    <span class="angel-badge angel-badge-success">Aprobado</span>
  </div>
  <div class="angel-card-body">
    <p><strong>Monto:</strong> $100.50</p>
    <p><strong>Fecha:</strong> 2025-10-08</p>
    <p><strong>Operador:</strong> Juan Pérez</p>
  </div>
</div>

.angel-glass
css.angel-glass {
  background: var(--glass-bg);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid var(--glass-border);
  border-radius: 16px;
  padding: 24px;
}
html<div class="angel-glass">
  <h3>Efecto Glassmorphism</h3>
  <p>Contenido con fondo semi-transparente y blur</p>
</div>
Uso: Overlays, modals modernos, paneles flotantes

Effects
.angel-glow-soft
css.angel-glow-soft {
  box-shadow: var(--glow-green);
  transition: box-shadow 0.3s ease;
}

.angel-glow-soft:hover {
  box-shadow: 0 0 30px rgba(26, 95, 63, 0.7);
}
html<button class="angel-button angel-button-primary angel-glow-soft">
  Con Glow Effect
</button>

.angel-spinner
css.angel-spinner {
  width: 48px;
  height: 48px;
  border: 4px solid var(--glass-border);
  border-top-color: var(--angel-green-light);
  border-radius: 50%;
  animation: angel-spin 1s linear infinite;
}

@keyframes angel-spin {
  to { transform: rotate(360deg); }
}

.angel-spinner-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  padding: 40px;
}

.angel-spinner-text {
  color: var(--text-secondary);
  font-size: 14px;
}
html<div class="angel-spinner-container">
  <div class="angel-spinner"></div>
  <p class="angel-spinner-text">🤖 Procesando con Gemini AI...</p>
</div>

.angel-badge
css.angel-badge {
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
html<span class="angel-badge angel-badge-success">Aprobado</span>
<span class="angel-badge angel-badge-warning">Pendiente</span>
<span class="angel-badge angel-badge-error">Rechazado</span>
<span class="angel-badge angel-badge-info">En Proceso</span>

🔧 JavaScript Helpers
Funciones JS incluidas en AngelStyle.

handleLogout()
javascriptfunction handleLogout() {
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
Uso en HTML:
html<button class="angel-button angel-button-ghost" onclick="handleLogout()">
  Salir
</button>

checkImpersonation()
javascriptfunction checkImpersonation() {
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
CSS del banner:
css.impersonation-banner {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: var(--warning);
  color: white;
  padding: 10px;
  text-align: center;
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

stopImpersonating()
javascriptfunction stopImpersonating() {
  google.script.run
    .withSuccessHandler(() => {
      location.reload();
    })
    .withFailureHandler((error) => {
      alert('Error: ' + error);
    })
    .clearImpersonatedRole();
}

📱 Responsive Breakpoints
css/* Mobile First Approach */

/* Base: Mobile (< 768px) */
.angel-container {
  padding: 16px;
}

/* Tablet (≥ 768px) */
@media (min-width: 768px) {
  .angel-container {
    padding: 24px;
  }
  
  .angel-card {
    padding: 24px;
  }
}

/* Desktop (≥ 1024px) */
@media (min-width: 1024px) {
  .angel-container {
    padding: 32px;
  }
  
  .angel-header {
    padding: 24px 0;
  }
}

/* Large Desktop (≥ 1440px) */
@media (min-width: 1440px) {
  .angel-container {
    max-width: 1400px;
  }
}
Grid Responsive
css.angel-grid {
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr;  /* Mobile: 1 columna */
}

@media (min-width: 768px) {
  .angel-grid {
    grid-template-columns: repeat(2, 1fr);  /* Tablet: 2 columnas */
  }
}

@media (min-width: 1024px) {
  .angel-grid {
    grid-template-columns: repeat(3, 1fr);  /* Desktop: 3 columnas */
  }
}
html<div class="angel-grid">
  <div class="angel-card">Card 1</div>
  <div class="angel-card">Card 2</div>
  <div class="angel-card">Card 3</div>
</div>

🛠️ Cómo Usar AngelStyle
Método 1: En Apps Script (Recomendado)
1. Agregar librería al proyecto
json// appsscript.json
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
2. Incluir en HTML
html<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi App</title>
    
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
3. Servir desde Code.gs
javascriptfunction doGet(e) {
  return HtmlService.createHtmlOutputFromFile('Index')
    .setTitle('Mi App')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}

Método 2: Desarrollo Local (con clasp)
1. Clonar AngelStyle localmente
bashcd "/path/to/grupo-angel-apps/libraries/AngelStyle"
clasp pull
2. Editar archivos CSS
bash# Editar en VS Code
code main.css.html
3. Subir cambios
bashclasp push
4. Incrementar versión
En Apps Script online:

Publish → Manage versions
New version: v4.1.0 - Mejoras en botones
Save

5. Actualizar apps que lo usan
json// En cada app, actualizar appsscript.json
{
  "dependencies": {
    "libraries": [
      {
        "userSymbol": "AngelStyle",
        "version": "5",  // Nueva versión
        "libraryId": "..."
      }
    ]
  }
}

🎯 Ejemplos Completos
Ejemplo 1: Formulario de Upload
html<!DOCTYPE html>
<html>
<head>
  <?!= AngelStyle.getStyles() ?>
</head>
<body>
  <div class="angel-container">
    <header class="angel-header">
      <h1>Procesar Ticket</h1>
      <button class="angel-button angel-button-ghost" onclick="handleLogout()">
        Salir
      </button>
    </header>
    
    <div class="angel-upload-area" onclick="document.getElementById('fileInput').click()">
      <p>📸 Haz click para seleccionar foto</p>
      <p class="small-text">Formatos: JPG, PNG (max 5MB)</p>
      <input type="file" id="fileInput" accept="image/*" style="display:none" onchange="handleFile(event)">
    </div>
    
    <div id="loading" class="angel-spinner-container hidden">
      <div class="angel-spinner"></div>
      <p class="angel-spinner-text">🤖 Analizando con Gemini AI...</p>
    </div>
    
    <div id="preview" class="angel-card hidden">
      <div class="angel-card-header">
        <h3 class="angel-card-title">Datos Extraídos</h3>
        <span class="angel-badge angel-badge-info">Verificar</span>
      </div>
      <div class="angel-card-body">
        <label>Monto:</label>
        <input type="number" class="angel-input" id="amount" placeholder="100.50">
        
        <label>Fecha:</label>
        <input type="date" class="angel-input" id="date">
        
        <label>Número de Ticket:</label>
        <input type="text" class="angel-input" id="ticketNumber" placeholder="ABC123">
        
        <div style="margin-top: 20px; display: flex; gap: 10px;">
          <button class="angel-button angel-button-primary" onclick="submitTicket()">
            Confirmar
          </button>
          <button class="angel-button angel-button-ghost" onclick="cancel()">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
  
  <script>
    function handleFile(e) {
      const file = e.target.files[0];
      if (!file) return;
      
      // Mostrar loading
      document.getElementById('loading').classList.remove('hidden');
      
      // Leer como base64
      const reader = new FileReader();
      reader.onload = function(event) {
        const base64 = event.target.result.split(',')[1];
        
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
      
      google.scriptRetryClaude does not have the ability to run the code it generates yet.CLContinue.run
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
function cancel() {
  if (confirm('¿Descartar estos datos?')) {
    location.reload();
  }
}
  </script>
</body>
</html>
```

Ejemplo 2: Dashboard de Supervisor
html<!DOCTYPE html>
<html>
<head>
  <?!= AngelStyle.getStyles() ?>
</head>
<body>
  <div class="angel-container">
    <header class="angel-header">
      <div>
        <h1>Dashboard Supervisor</h1>
        <p class="small-text" id="userEmail"></p>
      </div>
      <button class="angel-button angel-button-ghost" onclick="handleLogout()">
        Salir
      </button>
    </header>
    
    <!-- Stats Cards -->
    <div class="angel-grid">
      <div class="angel-card">
        <div class="angel-card-header">
          <h4 class="angel-card-title">Pendientes</h4>
          <span class="angel-badge angel-badge-warning" id="pendingCount">0</span>
        </div>
        <div class="angel-card-body">
          <p class="small-text">Tickets esperando aprobación</p>
        </div>
      </div>
      
      <div class="angel-card">
        <div class="angel-card-header">
          <h4 class="angel-card-title">Aprobados Hoy</h4>
          <span class="angel-badge angel-badge-success" id="approvedCount">0</span>
        </div>
        <div class="angel-card-body">
          <p class="small-text">Total: <strong id="approvedTotal">$0.00</strong></p>
        </div>
      </div>
      
      <div class="angel-card">
        <div class="angel-card-header">
          <h4 class="angel-card-title">Rechazados</h4>
          <span class="angel-badge angel-badge-error" id="rejectedCount">0</span>
        </div>
        <div class="angel-card-body">
          <p class="small-text">Últimas 24 horas</p>
        </div>
      </div>
    </div>
    
    <!-- Pending Tickets -->
    <div style="margin-top: 40px;">
      <h2>Tickets Pendientes</h2>
      <div id="ticketsList"></div>
    </div>
    
    <div id="loading" class="angel-spinner-container">
      <div class="angel-spinner"></div>
      <p class="angel-spinner-text">Cargando datos...</p>
    </div>
  </div>
  
  <script>
    // Load data on page load
    window.onload = function() {
      checkImpersonation();
      loadDashboard();
    };
    
    function loadDashboard() {
      google.script.run
        .withSuccessHandler(renderDashboard)
        .withFailureHandler((error) => {
          alert('Error cargando dashboard: ' + error);
        })
        .getDashboardData();
    }
    
    function renderDashboard(result) {
      document.getElementById('loading').style.display = 'none';
      
      if (!result.success) {
        alert('Error: ' + result.error);
        return;
      }
      
      const data = result.data;
      
      // Update stats
      document.getElementById('pendingCount').textContent = data.stats.pending;
      document.getElementById('approvedCount').textContent = data.stats.approved;
      document.getElementById('rejectedCount').textContent = data.stats.rejected;
      document.getElementById('approvedTotal').textContent = '$' + data.stats.approvedTotal.toFixed(2);
      document.getElementById('userEmail').textContent = data.userEmail;
      
      // Render tickets
      const ticketsList = document.getElementById('ticketsList');
      if (data.tickets.length === 0) {
        ticketsList.innerHTML = '<p class="small-text">No hay tickets pendientes</p>';
      } else {
        ticketsList.innerHTML = data.tickets.map(ticket => renderTicketCard(ticket)).join('');
      }
    }
    
    function renderTicketCard(ticket) {
      return `
        <div class="angel-card" style="margin-bottom: 20px;">
          <div class="angel-card-header">
            <h4 class="angel-card-title">Ticket #${ticket.ticketNumber}</h4>
            <span class="angel-badge angel-badge-info">Pendiente</span>
          </div>
          <div class="angel-card-body">
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 16px;">
              <div>
                <p><strong>Monto:</strong> $${ticket.amount}</p>
                <p><strong>Fecha:</strong> ${ticket.date}</p>
              </div>
              <div>
                <p><strong>Operador:</strong> ${ticket.operatorName}</p>
                <p><strong>Enviado:</strong> ${ticket.createdAt}</p>
              </div>
            </div>
            
            ${ticket.imageUrl ? `
              <div style="margin: 16px 0;">
                <img src="${ticket.imageUrl}" 
                     alt="Ticket" 
                     style="max-width: 100%; border-radius: 8px; cursor: pointer;"
                     onclick="window.open('${ticket.imageUrl}', '_blank')">
              </div>
            ` : ''}
            
            <div style="display: flex; gap: 12px; margin-top: 16px;">
              <button class="angel-button angel-button-primary" 
                      onclick="approveTicket('${ticket.id}')">
                ✓ Aprobar
              </button>
              <button class="angel-button angel-button-secondary" 
                      onclick="rejectTicket('${ticket.id}')">
                ✗ Rechazar
              </button>
            </div>
          </div>
        </div>
      `;
    }
    
    function approveTicket(ticketId) {
      if (!confirm('¿Aprobar este ticket?')) return;
      
      google.script.run
        .withSuccessHandler(() => {
          alert('✅ Ticket aprobado');
          loadDashboard();
        })
        .withFailureHandler((error) => {
          alert('Error: ' + error);
        })
        .approveTicket(ticketId);
    }
    
    function rejectTicket(ticketId) {
      const reason = prompt('Razón del rechazo:');
      if (!reason) return;
      
      google.script.run
        .withSuccessHandler(() => {
          alert('✅ Ticket rechazado');
          loadDashboard();
        })
        .withFailureHandler((error) => {
          alert('Error: ' + error);
        })
        .rejectTicket(ticketId, reason);
    }
  </script>
</body>
</html>

🔧 Mantenimiento de Esta Documentación
Cuándo Actualizar
✅ Agregar Nuevo Componente

Implementa el componente en AngelStyle

css   /* En main.css.html */
   .angel-nuevo-componente {
     /* estilos */
   }

Actualiza este documento

Agrega al índice
Crea sección completa con:

CSS code
HTML example
JavaScript (si aplica)
Casos de uso
Screenshot (si es complejo)




Commit

bash   git add human/reference/angelstyle-api.md
   git commit -m "docs(angelstyle): agregar componente .angel-nuevo"
✅ Modificar Componente Existente

Actualiza código en AngelStyle
Actualiza sección en este doc
Agrega nota de versión:

markdown   > **Actualizado en v4.1.0**: Ahora soporta tamaño `large`
✅ Deprecar Componente

Marca en código:

css   /* @deprecated Usar .angel-button en su lugar */
   .btn {
     /* ... */
   }

Agrega advertencia en doc:

markdown   ### `.btn` ⚠️ DEPRECATED
   
   **Razón**: Reemplazado por `.angel-button`
   
   **Migración**:
   \```html
   <!-- ❌ Viejo -->
   <button class="btn btn-primary">Click</button>
   
   <!-- ✅ Nuevo -->
   <button class="angel-button angel-button-primary">Click</button>
   \```
Template para Nuevo Componente
markdown### `.angel-nuevo-componente`

Descripción breve del componente.

#### CSS
\```css
.angel-nuevo-componente {
  /* estilos base */
}

.angel-nuevo-componente-variant {
  /* variante */
}
\```

#### HTML
\```html
<div class="angel-nuevo-componente">
  Contenido
</div>
\```

#### Uso Recomendado
- ✅ Usar para X
- ⚠️ No usar para Y

#### Variantes Disponibles
- `.angel-nuevo-componente-primary`
- `.angel-nuevo-componente-secondary`

#### Ejemplo Completo
\```html
<!-- Código funcional completo -->
\```

#### Notas
- ✅ Compatible con mobile
- ⚠️ Requiere JavaScript para Z
Checklist Pre-Commit
Antes de hacer commit de cambios:

 CSS probado en Chrome, Firefox, Safari
 Responsive verificado (mobile, tablet, desktop)
 Ejemplos funcionan (copy-paste y probar)
 Screenshots actualizados (si aplica)
 Índice actualizado
 Links internos funcionan
 Versión actualizada en header

Testing Visual Checklist
markdown## Testing de Componente Nuevo

- [ ] **Desktop**
  - [ ] Chrome (latest)
  - [ ] Firefox (latest)
  - [ ] Safari (latest)

- [ ] **Tablet**
  - [ ] iPad (Safari)
  - [ ] Android tablet (Chrome)

- [ ] **Mobile**
  - [ ] iPhone SE (pequeño)
  - [ ] iPhone 14 (estándar)
  - [ ] Android (varios tamaños)

- [ ] **Estados**
  - [ ] Normal
  - [ ] Hover
  - [ ] Focus
  - [ ] Active
  - [ ] Disabled

- [ ] **Temas**
  - [ ] Dark mode (principal)
  - [ ] Light mode (si aplica)

- [ ] **Accesibilidad**
  - [ ] Contraste suficiente
  - [ ] Tamaño mínimo táctil (44px)
  - [ ] Keyboard navigation

🔗 Links Relacionados

📖 System Overview
📖 CLib API Reference
📖 Creating New App (próximamente)
💻 AngelStyle Source Code
🎨 Lexend Font


Última actualización: Octubre 2025
Mantenido por: Christian Luciani (@ChristianLuciani)
Versión AngelStyle: v4.0.0 (v5.0.0 en desarrollo)

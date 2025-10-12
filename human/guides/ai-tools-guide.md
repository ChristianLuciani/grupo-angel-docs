# 🤖 AI Tools Guide

## Cuándo Usar Cada Herramienta

### Gemini Chat (gemini.google.com)
**✅ Mejor para:**
- Actualizar documentación con GEMs
- Conversaciones exploratorias
- Iterar sobre ideas
- Tasks ocasionales y ad-hoc

**Ejemplo**: Limpiar formato de docs, actualizar referencias

---

### Google AI Studio (aistudio.google.com)
**✅ Mejor para:**
- Experimentar con prompts antes de implementar
- Comparar modelos (Flash vs Pro)
- Ajustar parámetros técnicos
- Generar código para integración

**Ejemplo**: Testing prompt de OCR para optimizar antes de producción

---

### Gemini API (en código)
**✅ Mejor para:**
- OCR en producción (TicketProcessor)
- Clasificación de documentos (CasinoETL)
- Automatización de procesos
- Integración en apps

**Ejemplo**: CLib.callGenerativeApi() en apps

---

### VS Code + Copilot/Gemini Extension
**✅ Mejor para:**
- Autocompletado de código
- Generación de funciones
- Refactoring
- Documentación inline (JSDoc)

**Ejemplo**: Escribir nueva función en CLib con asistencia AI

---

### clasp + Terminal
**✅ Mejor para:**
- Push/pull código Apps Script
- Versionado local
- Git operations
- Deploy automatizado

**Ejemplo**: Workflow híbrido local→online

---

## Workflow Recomendado por Tipo de Tarea

### Task: Crear Nueva App
1. **Planear**: Gemini Chat (conversación)
2. **Codificar**: VS Code + Copilot
3. **Testear OCR**: AI Studio (experimentar)
4. **Implementar**: clasp push
5. **Deploy**: Apps Script UI

### Task: Actualizar Docs
1. **Actualizar**: Gemini Chat con GEM
2. **Verificar**: GitHub preview
3. **Commit**: Terminal + git

### Task: Debugging Complejo
1. **Analizar logs**: Apps Script Executions
2. **Consultar**: Claude Sonnet (este chat)
3. **Fix**: VS Code local
4. **Deploy**: clasp push

### Task: Optimizar Prompt
1. **Experimentar**: AI Studio
2. **Comparar**: Flash vs Pro
3. **Implementar**: Actualizar CLib
4. **Documentar**: Agregar a docs

# 🔧 Solución de Problemas - IsivoltPro

## ⚠️ Problema Resuelto: App se Queda en Pantalla de Carga

### 🐛 Causa del Problema
El código JavaScript tenía **funciones faltantes** que se llamaban durante la inicialización:
- `loadData()` - Para cargar datos del LocalStorage
- `renderTabs()` - Para actualizar badges en las pestañas
- `saveData()` - Para guardar datos (también faltaba)

Cuando la app intentaba ejecutar `DOMContentLoaded`, llamaba a funciones que no existían, causando un error de JavaScript que bloqueaba la ejecución y dejaba la pantalla de splash visible.

### ✅ Solución Aplicada
Se agregaron las funciones faltantes:

```javascript
function loadData(){
  try {
    inventory = JSON.parse(localStorage.getItem('iv_inventory') || '[]');
    workers = JSON.parse(localStorage.getItem('iv_workers') || '[]');
    history = JSON.parse(localStorage.getItem('iv_history') || '[]');
    soundEnabled = localStorage.getItem('iv_sound') !== 'false';
  } catch(e) {
    console.error('Error loading data:', e);
    inventory = [];
    workers = [];
    history = [];
  }
}

function saveData(){
  try {
    localStorage.setItem('iv_inventory', JSON.stringify(inventory));
    localStorage.setItem('iv_workers', JSON.stringify(workers));
    localStorage.setItem('iv_history', JSON.stringify(history));
  } catch(e) {
    console.error('Error saving data:', e);
    showNotif('Error al guardar datos', 'error');
  }
}

function renderTabs(){
  // Actualiza los badges con contadores
}
```

Además, se añadió manejo de errores en la inicialización:

```javascript
window.addEventListener('DOMContentLoaded', ()=>{
  try {
    initSplash();
    loadData();
    renderDashboard();
    renderTabs();
  } catch(e) {
    console.error('Initialization error:', e);
    alert('Error al inicializar. Recarga la página.');
  }
});
```

---

## 🛠️ Otros Problemas Comunes y Soluciones

### 1. ❌ La cámara no funciona en móvil

**Síntomas:**
- Al intentar escanear QR, aparece error
- No se solicita permiso de cámara
- La cámara no se activa

**Causa:**
Los navegadores modernos requieren HTTPS para acceder a la cámara (excepto en localhost).

**Solución:**
- ✅ Publica la app en GitHub Pages (HTTPS automático)
- ✅ Usa Netlify o Vercel (HTTPS gratis)
- ✅ En desarrollo local, usa `localhost` o `127.0.0.1`
- ❌ NO uses IP local tipo `192.168.x.x` (no funciona la cámara)

**Alternativa temporal:**
Si no puedes usar HTTPS, puedes:
1. Usar el modo manual (sin QR)
2. En **📦 Materiales** → Botones **Sacar/Regresar**

---

### 2. 💾 Los datos desaparecen al cerrar el navegador

**Síntomas:**
- Agregas materiales/personal
- Al cerrar y reabrir, todo está vacío
- Los datos no persisten

**Causa:**
- Estás usando modo incógnito/privado
- Las cookies/LocalStorage están bloqueadas
- El navegador borra datos al cerrar

**Solución:**
```javascript
// Verifica si LocalStorage funciona:
// Abre la consola (F12) y escribe:
localStorage.setItem('test', '123');
console.log(localStorage.getItem('test'));
// Debe mostrar '123'
```

**Acciones:**
- ✅ NO usar modo incógnito/privado
- ✅ Permitir cookies y almacenamiento local
- ✅ Agregar el sitio a favoritos/marcadores
- ✅ Exportar a Excel regularmente como respaldo

---

### 3. 🔇 No se escucha ningún sonido

**Síntomas:**
- Los sonidos no se reproducen
- El botón 🔊 está activo pero no suena

**Causa:**
- Los navegadores bloquean audio hasta que el usuario interactúe
- El volumen del dispositivo está bajo/silenciado
- El audio está desactivado en la app

**Solución:**
1. **Verifica el botón de sonido:**
   - Debe mostrar 🔊 (activo)
   - Si muestra 🔇, haz clic para activar

2. **En móvil:**
   - Toca la pantalla primero (el navegador requiere interacción)
   - Verifica que el volumen del dispositivo esté alto
   - Desactiva modo silencioso

3. **Prueba manual:**
```javascript
// En la consola (F12):
playSound('welcome');
// Debe sonar una melodía
```

---

### 4. 📷 El QR no se lee / lectura lenta

**Síntomas:**
- El escáner está activo pero no lee el código
- Tarda mucho en detectar
- Da error "QR no válido"

**Solución:**
1. **Iluminación:**
   - Usa buena luz (evita sombras sobre el QR)
   - No uses contraluz

2. **Distancia y enfoque:**
   - Mantén la cámara a 15-30 cm del QR
   - El QR debe ocupar ~50% del cuadro
   - Mantén la cámara estable

3. **Calidad del QR:**
   - Imprime en alta resolución
   - No uses QR arrugados o dañados
   - Tamaño mínimo recomendado: 3x3 cm

4. **Alternativa:**
   - Usa el modo manual en **📦 Materiales**

---

### 5. 📊 El Excel no se descarga

**Síntomas:**
- Haces clic en "Exportar Excel"
- No se descarga nada
- Aparece error en consola

**Causa:**
- El navegador bloqueó la descarga
- No hay datos para exportar
- Error en la librería SheetJS

**Solución:**
1. **Verifica permisos:**
   - Permite descargas en el navegador
   - Revisa el bloqueador de pop-ups

2. **Verifica que haya datos:**
```javascript
// En consola (F12):
console.log(history.length);
// Debe ser > 0
```

3. **Prueba manual:**
   - Botón derecho en la página
   - "Guardar como..." → HTML completo
   - Abre ese archivo y prueba exportar

---

### 6. 🔄 La app se ve rara / estilos rotos

**Síntomas:**
- Los colores no se ven
- Los elementos están desalineados
- Falta el diseño

**Causa:**
- El archivo HTML está incompleto
- Error en el CSS
- Navegador muy antiguo

**Solución:**
1. **Usa la versión corregida:**
   - Descarga `index_CORREGIDO_FUNCIONAL.html`

2. **Actualiza el navegador:**
   - Chrome 90+
   - Firefox 88+
   - Safari 14+
   - Edge 90+

3. **Borra caché:**
   - Ctrl + Shift + R (Windows/Linux)
   - Cmd + Shift + R (Mac)

---

### 7. 📱 No funciona en mi navegador/dispositivo

**Navegadores Soportados:**
- ✅ Chrome/Edge 90+ (recomendado)
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Opera 76+
- ❌ Internet Explorer (no soportado)

**Dispositivos:**
- ✅ Android 8+ (Chrome)
- ✅ iOS 14+ (Safari)
- ✅ Windows 10/11
- ✅ macOS 10.15+
- ✅ Linux (Ubuntu, Fedora, etc)

---

### 8. 🚨 Error: "Cannot read property 'xxx' of undefined"

**Causa:**
Alguna función intenta acceder a un elemento HTML que no existe.

**Solución:**
1. **Verifica el HTML está completo:**
```bash
# Las últimas líneas deben ser:
</script>
</body>
</html>
```

2. **Usa la versión corregida:**
   - `index_CORREGIDO_FUNCIONAL.html`

3. **Revisa la consola:**
   - F12 → Console
   - Copia el error completo
   - Busca en GitHub Issues

---

## 🔍 Herramientas de Debug

### Ver errores en consola:
```
1. Presiona F12 (o Cmd+Option+I en Mac)
2. Ve a la pestaña "Console"
3. Busca mensajes en rojo (errores)
4. Copia el mensaje completo
```

### Ver datos guardados:
```javascript
// En consola:
console.log(localStorage.getItem('iv_inventory'));
console.log(localStorage.getItem('iv_workers'));
console.log(localStorage.getItem('iv_history'));
```

### Resetear la app:
```javascript
// ⚠️ CUIDADO: Borra todos los datos
localStorage.clear();
location.reload();
```

### Exportar datos manualmente:
```javascript
// Copia y pega en un archivo .json:
console.log(JSON.stringify({
  inventory: JSON.parse(localStorage.getItem('iv_inventory') || '[]'),
  workers: JSON.parse(localStorage.getItem('iv_workers') || '[]'),
  history: JSON.parse(localStorage.getItem('iv_history') || '[]')
}, null, 2));
```

---

## 📞 ¿Aún tienes problemas?

1. **Verifica:**
   - ✅ Estás usando `index_CORREGIDO_FUNCIONAL.html`
   - ✅ Navegador actualizado
   - ✅ No estás en modo incógnito
   - ✅ LocalStorage habilitado
   - ✅ JavaScript habilitado

2. **Revisa la consola:**
   - Abre F12 → Console
   - Copia todos los errores en rojo

3. **Reporta el problema:**
   - Ve a GitHub Issues
   - Incluye:
     - Navegador y versión
     - Sistema operativo
     - Pasos para reproducir
     - Captura del error en consola
     - Captura de pantalla del problema

---

## 💡 Tips para Evitar Problemas

### ✅ Buenas Prácticas:
- Exporta tus datos a Excel semanalmente
- No uses modo incógnito para la app
- Mantén el navegador actualizado
- Usa HTTPS en producción
- Haz backup del archivo HTML

### ❌ Evita:
- Editar el HTML sin saber JavaScript
- Minificar el código (dificulta debug)
- Usar IPs locales en móvil (la cámara no funciona)
- Bloquear LocalStorage
- Usar navegadores obsoletos

---

**⚡ IsivoltPro** - Ahora funcional y listo para usar.

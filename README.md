# 📊 INCASUR Dashboard — Web Estática

Dashboard de control de incidencias INCASUR. Réplica exacta de la app Streamlit, pero como página web estática que funciona desde cualquier navegador, sin necesidad de Python.

## 🚀 Despliegue en GitHub Pages (gratis, 5 minutos)

### Paso 1 — Subir el repositorio

```bash
git init
git add .
git commit -m "Initial commit - INCASUR Dashboard"
git remote add origin https://github.com/TU_USUARIO/incasur-dashboard.git
git push -u origin main
```

### Paso 2 — Activar GitHub Pages

1. Ve a tu repositorio en GitHub → **Settings** → **Pages**
2. En *Source*, selecciona **Deploy from a branch**
3. Rama: `main`, carpeta: `/ (root)`
4. Clic en **Save**

En ~1 minuto tendrás la URL: `https://TU_USUARIO.github.io/incasur-dashboard/`

---

## ⚙️ Configuración en el navegador

Al abrir la web por primera vez aparece un modal de configuración. Rellena:

| Campo | Valor |
|---|---|
| URL Base de Jira | `https://proyectos.gco.global` |
| Token Bearer | Tu token de API de Jira (solo lectura) |
| JQL Query | `project=INCASUR AND resolution = Unresolved` |
| Campo Reclamaciones | `customfield_15400` |

Los datos se guardan en **localStorage** del navegador (nunca salen de tu dispositivo).

---

## ⚠️ Requisito CORS de Jira

La web hace peticiones directas a la API de Jira desde el navegador. Para que funcione, **Jira debe permitir CORS** desde el dominio de GitHub Pages.

### Opciones si hay problemas de CORS:

**Opción A — Jira Server con proxy CORS habilitado** (preguntar al admin de Jira)

**Opción B — Netlify Functions como proxy** (más complejo):
```
// netlify/functions/jira-proxy.js
exports.handler = async (event) => { ... }
```

**Opción C — Cloudflare Workers** (gratuito, recomendado si hay CORS):
```javascript
// worker.js
export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    const jiraUrl = url.searchParams.get('url');
    const token = request.headers.get('X-Jira-Token');
    // redirige la petición a Jira y añade cabeceras CORS
  }
}
```

---

## 🔒 Seguridad

- El token **nunca se envía** a ningún servidor excepto directamente a tu instancia de Jira.
- Se guarda en `localStorage` del navegador del usuario.
- **Usa siempre un token de solo lectura** (Read-only API token de Jira).
- Si compartes la URL, cada usuario deberá configurar su propio token.

---

## ✅ Funcionalidades

- ✅ Misma lógica de extracción de datos que Streamlit (changelog, departamentos, rebotes)
- ✅ Filtros dinámicos: estado, departamento, días mínimos, reclamaciones, rebotes
- ✅ KPIs: tickets abiertos, máx días, promedio atascado, total reclamaciones
- ✅ Tabla ordenable por columnas
- ✅ Búsqueda por clave/resumen
- ✅ Paginación (50 por página)
- ✅ Exportar a Excel con los filtros activos
- ✅ Caché de 24h en localStorage para no sobrecargar la API
- ✅ Botón "Forzar Actualización" para limpiar caché
- ✅ Accesible desde cualquier dispositivo con la URL de GitHub Pages

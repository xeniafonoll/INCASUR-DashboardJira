# 📊 INCASUR Dashboard

Dashboard accesible desde cualquier navegador, sin VPN, sin servidores.  
Los datos se actualizan automáticamente cada día a las 8:00h.

## Cómo funciona

```
GitHub Action (cada día 8:00h)  →  llama a Jira  →  guarda data.json en el repo
Navegador  →  lee data.json desde GitHub Pages  →  muestra el dashboard al instante
```

- Sin proxy. Sin puertos. Sin CORS. Sin VPN.
- El token de Jira vive como secreto cifrado en GitHub.
- La carga es instantánea (lee un archivo estático).

---

## Puesta en marcha

### 1. Subir el repositorio a GitHub

```bash
git init
git add .
git commit -m "INCASUR Dashboard"
git remote add origin https://github.com/TU_USUARIO/incasur-dashboard.git
git push -u origin main
```

### 2. Añadir el token de Jira como secreto

En GitHub → tu repositorio → **Settings → Secrets and variables → Actions → New repository secret**

- Nombre: `JIRA_TOKEN`
- Valor: tu token Bearer de Jira (solo lectura)

### 3. Activar GitHub Pages

**Settings → Pages → Source: Deploy from a branch → main / root → Save**

URL resultante: `https://TU_USUARIO.github.io/incasur-dashboard/`

### 4. Ejecutar la Action por primera vez

**Actions → Actualizar datos Jira → Run workflow**

Esto genera el `data.json` inicial. A partir de ahí se ejecuta sola cada día a las 8:00h.

---

## Archivos

| Archivo | Qué hace |
|---|---|
| `index.html` | El dashboard web |
| `data.json` | Datos procesados de Jira (generado automáticamente) |
| `fetch_jira.py` | Script que descarga y procesa los datos de Jira |
| `.github/workflows/actualizar_datos.yml` | GitHub Action que ejecuta el script cada día |

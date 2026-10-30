# MSO API Proxy

Proxy HTTP ligero para consumir la API MSO (`kpi.red.com.sv`) desde el Dashboard Intelfon. Mantiene `MSO_API_KEY` en el servidor, reenvía solicitudes `GET` y aplica una caché en memoria.

## Inicio rápido

Requisitos: Node.js 18 o posterior. No hay dependencias externas.

1. Copia `.env.example` a `.env` y define `MSO_API_KEY` con una clave vigente. No publiques ni compartas ese archivo.
2. Inicia el servicio:

   ```bash
   npm start
   ```

3. Verifica que esté disponible:

   ```text
   http://localhost:3000/api/health
   ```

En PowerShell, puedes definir la clave para una sola sesión en vez de usar `.env`:

```powershell
$env:MSO_API_KEY = "REEMPLAZAR_CON_TU_CLAVE"
npm start
```

## Rutas del proxy

| Ruta local | Prefijo upstream |
|---|---|
| `/api/mso/simcards` | `/dashboard-simcards` |
| `/api/mso/reclamos` | `/dashboard-reclamos-m2m` |

El sufijo de la ruta y los parámetros de consulta se reenvían a MSO. Usa `?refresh=1` para omitir una respuesta almacenada en caché; `refresh` se elimina antes de consultar MSO. El detalle de rutas, ejemplos, respuestas, configuración y despliegue está en la [guía de API](docs/API.md). También puedes descargar la [guía en PDF](docs/API.pdf).

## Configuración

`MSO_API_KEY` es obligatoria. Las demás variables son opcionales:

| Variable | Predeterminado | Uso |
|---|---:|---|
| `MSO_API_BASE_URL` | `https://kpi.red.com.sv/api` | URL base de MSO |
| `PORT` | `3000` | Puerto HTTP |
| `HOST` | `0.0.0.0` | Interfaz de escucha |
| `CACHE_TTL_MS` | `60000` | Duración de caché en milisegundos |
| `MSO_API_TIMEOUT_MS` | `30000` | Tiempo límite upstream en milisegundos |
| `ALLOW_ORIGIN` | `*` | Orígenes CORS permitidos, separados por coma |

## Seguridad de credenciales

La clave solo se envía al upstream en el encabezado `x-api-key`. No la incluyas en JavaScript del navegador, URLs, capturas, tickets ni documentos compartidos. `.env` está excluido de Git; para despliegues, configura la clave en el gestor de secretos o variables protegidas del proveedor.

## Estado del proyecto

Las rutas de negocio concretas y los esquemas de datos dependen de la API MSO. Este repositorio define los prefijos de reenvío, pero no contiene un catálogo upstream; consulta la documentación del proveedor para conocer los endpoints y parámetros soportados.

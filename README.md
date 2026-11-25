# Escáner de Archivos y Credenciales Expuestas

[![Escaneo Archivos Expuestos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Herramienta automatizada de análisis de superficie de ataque para la detección de archivos de entorno expuestos, copias de seguridad de bases de datos, repositorios git y credenciales de nube.

El motor inspecciona perímetros web para localizar configuraciones erróneas en servidores y fugas involuntarias de activos críticos antes de que actores maliciosos puedan explotarlos. Diseñado sin dependencias externas, opera de forma nativa en consola Python y flujos de automatización de seguridad.

***

## Alcance de Detección de Superficie y Arquitectura de Amenazas

La exposición accidental de recursos confidenciales en la raíz pública de servidores web representa uno de los vectores de Acceso Inicial más críticos en la infraestructura moderna. Servidores Nginx, Apache, Caddy o buckets S3 mal configurados a menudo sirven archivos ocultos y respaldos SQL a usuarios no autenticados.

* **Variables de Entorno y Secretos**: Inspección directa de archivos `.env`, `.env.local` y `.env.production` que contienen contraseñas de bases de datos, claves de cifrado y credenciales de API.
* **Metadatos de Control de Versiones**: Localización de rutas `/.git/HEAD` y `/.git/config`, evitando que atacantes descarguen y reconstruyan el código fuente completo.
* **Respaldos de Bases de Datos**: Detección de volcados SQL críticos como `backup.sql`, `dump.sql` y `database.sql`, que exponen esquemas y datos confidenciales.
* **Infraestructura y Orquestación**: Revisión de manifiestos en `docker-compose.yml` y diagnósticos del servidor mediante `phpinfo.php` y `config.json`.
* **Credenciales de Acceso Cloud**: Verificación de rutas `/.aws/credentials`, previniendo la toma de control de cuentas y recursos de infraestructura en la nube.
* **Alineación Normativa**: Estructurado en estricta conformidad con las directrices de Medidas de Seguridad Digital del INAI y LFPD-PPP.

***

## Matriz de Inspección de Superficie de Ataque

<table>
  <thead>
    <tr>
      <th align="left">Ruta Auditada</th>
      <th align="left">Severidad</th>
      <th align="left">Vector de Amenaza y Tipo de Activo</th>
      <th align="left">Medida Defensiva Recomendada</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>/.env</code></td>
      <td>CRÍTICA</td>
      <td>Variables de entorno, contraseñas de base de datos y claves privadas de API.</td>
      <td>Bloquear acceso a archivos que inicien con punto o moverlos fuera de la raíz web.</td>
    </tr>
    <tr>
      <td><code>/.env.local</code></td>
      <td>CRÍTICA</td>
      <td>Ajustes de desarrollo y secretos de prueba subidos accidentalmente a producción.</td>
      <td>Configurar reglas de .gitignore rigurosas y rechazar dotfiles en el proxy inverso.</td>
    </tr>
    <tr>
      <td><code>/.env.production</code></td>
      <td>CRÍTICA</td>
      <td>Credenciales del entorno de producción y tokens de servicios integrados.</td>
      <td>Sustituir archivos estáticos por gestores de secretos basados en bóvedas seguras.</td>
    </tr>
    <tr>
      <td><code>/.git/HEAD</code></td>
      <td>CRÍTICA</td>
      <td>Puntero de rama de git que revela la arquitectura del repositorio de código.</td>
      <td>Bloquear el directorio /.git en el servidor y eliminar carpetas git en el despliegue.</td>
    </tr>
    <tr>
      <td><code>/.git/config</code></td>
      <td>CRÍTICA</td>
      <td>Direcciones de repositorios remotos y posibles tokens de autenticación asociados.</td>
      <td>Denegar peticiones a metadatos de control de versiones en todos los servidores virtuales.</td>
    </tr>
    <tr>
      <td><code>/backup.sql</code></td>
      <td>CRÍTICA</td>
      <td>Copia de seguridad completa con tablas, contraseñas con hash y datos de usuarios.</td>
      <td>Almacenar respaldos en almacenamiento en la nube privado con cifrado en reposo.</td>
    </tr>
    <tr>
      <td><code>/dump.sql</code></td>
      <td>CRÍTICA</td>
      <td>Exportación directa de datos generada tras labores manuales de mantenimiento.</td>
      <td>Ejecutar rutinas automáticas de limpieza tras cada ciclo de despliegue de software.</td>
    </tr>
    <tr>
      <td><code>/database.sql</code></td>
      <td>CRÍTICA</td>
      <td>Esquema relacional de la base de datos y datos iniciales de inicialización.</td>
      <td>Prohibir la persistencia de scripts de base de datos en carpetas con salida web directa.</td>
    </tr>
    <tr>
      <td><code>/docker-compose.yml</code></td>
      <td>ALTA</td>
      <td>Estructura de contenedores, nombres de red interna y variables de entorno.</td>
      <td>Excluir manifiestos de orquestación de los paquetes de despliegue en entornos web.</td>
    </tr>
    <tr>
      <td><code>/phpinfo.php</code></td>
      <td>MEDIA</td>
      <td>Versión del motor PHP, extensiones instaladas y rutas absolutas del servidor.</td>
      <td>Inhabilitar la función phpinfo() en el php.ini productivo y suprimir archivos de prueba.</td>
    </tr>
    <tr>
      <td><code>/config.json</code></td>
      <td>ALTA</td>
      <td>Configuración interna del aplicativo, rutas de red y parámetros de componentes.</td>
      <td>Limitar el acceso a archivos JSON de configuración o integrarlos en código binario.</td>
    </tr>
    <tr>
      <td><code>/.aws/credentials</code></td>
      <td>CRÍTICA</td>
      <td>Identificadores de clave de acceso y secretos de administración de AWS.</td>
      <td>Utilizar roles IAM y perfiles de instancia en lugar de guardar credenciales en disco.</td>
    </tr>
  </tbody>
</table>

***

## Instalación y Guía Rápida de Uso

Requiere Python 3.8 o superior utilizando únicamente librerías estándar (`sys`, `json`, `ssl`, `argparse`, `urllib.request`). No requiere descargas adicionales vía pip.

Clone el repositorio y ejecute el escaneo sobre un dominio objetivo:

```bash
git clone https://github.com/TAVARDT-Mexico/exposed-files-scanner-mx.git
cd exposed-files-scanner-mx
python scanner.py ejemplo.mx
```

Para generar los resultados estructurados en formato JSON:

```bash
python scanner.py ejemplo.mx -j
```

Inspeccione dominios indicando el protocolo de conexión:

```bash
python scanner.py https://ejemplo.mx -j
```

Ejemplo de payload JSON generado:

```json
{
  "target": "https://ejemplo.mx",
  "status": "Critical (Severe secret or data exposure detected)",
  "inspected_routes": 12,
  "exposed_count": 1,
  "exposed_files": [
    {
      "path": "/.env",
      "url": "https://ejemplo.mx/.env",
      "status_code": 200,
      "severity": "CRITICAL",
      "description": "Environment variables and API secrets"
    }
  ],
  "recommendation": "Immediately decommission exposed files and enforce web server hardening controls."
}
```

***

## Preguntas Frecuentes (FAQ Técnico)

### ¿Cómo distingue el escáner la exposición real de archivos de respuestas 404 personalizadas?
El escáner detecta la presencia de un código de estado HTTP 200 OK en las rutas auditadas. Si el servidor devuelve código 200 para páginas inexistentes en vez de emitir HTTP 404 o 403, los administradores deben normalizar las cabeceras de error para evitar detecciones no deseadas.

### ¿Cuáles son los riesgos reales derivados del acceso público a la carpeta `/.git/`?
La exposición de rutas como `/.git/HEAD` y `/.git/config` permite a los atacantes descargar los objetos del repositorio utilizando herramientas automáticas. Con ello pueden reconstruir el código fuente íntegro, analizar el historial de cambios y localizar contraseñas olvidadas.

### ¿Por qué el motor de auditoría prescinde de la validación estricta de certificados SSL?
El script establece un contexto SSL unificado (`ssl.CERT_NONE`) con comprobación de nombres desactivada para permitir el análisis de dominios en preproducción, certificados autofirmados o entornos con cambios de claves sin que se interrumpa la ejecución.

### ¿Cómo puede incorporarse la salida en formato JSON en cadenas de DevSecOps?
Mediante el parámetro `-j`, el script emite datos JSON limpios hacia la salida estándar. Las plataformas de integración continua pueden consultar el valor de `exposed_count` con utilidades como jq, cancelando el despliegue si algún activo crítico devuelve un código HTTP 200.

### ¿Qué medidas de mitigación inmediata deben adoptarse ante estas vulnerabilidades?
Se recomienda retirar inmediatamente los archivos confidenciales de la raíz accesible y definir directivas de bloqueo en Nginx (`location ~ /\.(?!well-known).* { deny all; }`) o en Apache (`RedirectMatch 404 /\.(git|env)`). Los respaldos de bases de datos nunca deben ubicarse en directorios web.

***

## Consultoría en Seguridad Ofensiva

Para pruebas de penetración, remediación de vulnerabilidades y defensa perimetral zero trust en México, contacte con los especialistas de **TAVARDT México**.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnósticos:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT.

***

© 2026 TAVARDT. Todos los derechos reservados.

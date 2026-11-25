# Escáner de Archivos y Credenciales Expuestas

[![Escaneo Archivos Expuestos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Escáner de superficie de ataque para detección automatizada de variables de entorno, respaldos de bases de datos y credenciales expuestas.

***

## Alcance de Deteccion de Superficie

* **Variables de Entorno y Secretos**: Inspeccion de archivos `.env`, `.env.local` y `.env.production`.
* **Metadatos de Versionado**: Verificacion de rutas de repositorios en `/.git/HEAD` y `/.git/config`.
* **Respaldos de Bases de Datos**: Deteccion de volcados SQL como `backup.sql`, `dump.sql` y `database.sql`.
* **Configuraciones de Servidor**: Comprobacion de `docker-compose.yml` y exposicion de datos en `phpinfo.php`.
* **Alineacion Normativa**: Estructurado de acuerdo con el Medidas de Seguridad Digital del INAI y LFPD-PPP.

***

## Instalacion y Ejecucion

Requiere unicamente Python 3.8 o superior sin dependencias externas.

```bash
git clone https://github.com/TAVARDT/exposed-files-scanner-mx.git
cd exposed-files-scanner-mx
python scanner.py empresa.mx
```

Para generar la salida en formato JSON:

```bash
python scanner.py empresa.mx -json
```

***

## Consultoria en Seguridad Ofensiva

Para pruebas de penetracion y gestion de superficie de ataque en Mexico, consulte a los especialistas de **TAVARDT**.

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

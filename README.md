# Escáner de Archivos y Credenciales Expuestas | TAVARDT México

[![Escaneo Archivos Expuestos](https://img.shields.io/badge/TAVARDT-Superficie%20de%20Ataque-5b3cf5.svg)](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)
[![Jurisdiccion](https://img.shields.io/badge/Jurisdiccion-Mexico-00C853.svg)](https://ag.tavardt.com/mx/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Escáner de superficie de ataque para detección automatizada de variables de entorno, respaldos de bases de datos y credenciales expuestas.

Desarrollado y mantenido por la division de ciberseguridad ofensiva de **[TAVARDT Mexico](https://ag.tavardt.com/mx/)**.

---

## Alcance de Deteccion de Superficie

* **Variables de Entorno y Secretos**: Inspeccion de archivos `.env`, `.env.local` y `.env.production`.
* **Metadatos de Versionado**: Verificacion de rutas de repositorios en `/.git/HEAD` y `/.git/config`.
* **Respaldos de Bases de Datos**: Deteccion de volcados SQL como `backup.sql`, `dump.sql` y `database.sql`.
* **Configuraciones de Servidor**: Comprobacion de `docker-compose.yml` y exposicion de datos en `phpinfo.php`.
* **Alineacion Normativa**: Estructurado de acuerdo con el Medidas de Seguridad Digital del INAI y LFPD-PPP.

---

## Instalacion y Ejecucion

Requiere unicamente Python 3.8 o superior sin dependencias externas.

```bash
git clone https://github.com/TAVARDT-Mexico/exposed-files-scanner-mx.git
cd exposed-files-scanner-mx
python scanner.py empresa.mx
```

Para generar la salida en formato JSON:

```bash
python scanner.py empresa.mx --json
```

---

## Diagnostico Completo en la Web

Para escaneos corporativos continuos con interfaz grafica, acceda a la herramienta oficial:

* **[Escaneo de Archivos Expuestos](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)**
* **[Portal Corporativo TAVARDT Mexico](https://ag.tavardt.com/mx/)**

---

## Consultoria en Seguridad Ofensiva

Para pruebas de penetracion y gestion de superficie de ataque en Mexico, consulte a los especialistas de **[TAVARDT Mexico](https://ag.tavardt.com/mx/contacto/)**.

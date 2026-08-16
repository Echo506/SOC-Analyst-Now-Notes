# Lab 11: Investigación de dispositivo USB sospechoso

## Objetivo

Documentar la investigación de una alerta relacionada con la conexión de un dispositivo USB no autorizado y la posible ejecución de archivos desde un medio removible.

## Escenario

El EDR detecta la conexión de una memoria USB en una estación de trabajo corporativa. Minutos después, se registra la ejecución de un archivo desde la unidad removible.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Dispositivo USB y ejecución sospechosa |
| Equipo afectado | `WS-SALES-014` |
| Usuario asociado | `usuario.ejemplo` |
| Dispositivo detectado | USB Mass Storage Device |
| Unidad asignada | `E:\` |
| Archivo ejecutado | `actualizacion.exe` |
| Ruta | `E:\actualizacion.exe` |
| Proceso padre | `explorer.exe` |
| Severidad inicial | Alta |

> Los datos de este laboratorio son ficticios y se utilizan únicamente con fines educativos.

## Hipótesis inicial

La actividad puede corresponder al uso legítimo de una memoria USB autorizada. Sin embargo, la ejecución de un archivo desde una unidad removible puede indicar introducción de malware, ejecución manual de un archivo malicioso o incumplimiento de la política de dispositivos removibles.

## Indicadores observados

- Conexión de un USB no reconocido.
- Ejecución de archivos `.exe`, `.bat`, `.ps1` o accesos directos desde la unidad removible.
- Creación de archivos en carpetas temporales o `AppData`.
- Alertas del antivirus o EDR posteriores a la conexión.
- Conexiones de red sospechosas desde el endpoint.
- Actividad fuera del horario habitual.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el equipo, usuario, fecha, hora, fabricante o identificador del dispositivo, letra de unidad y archivo ejecutado.

### 2. Revisar el dispositivo

- Validar si el USB pertenece a un empleado o proveedor autorizado.
- Revisar número de serie, fabricante y política interna de dispositivos removibles.
- Determinar si el mismo USB fue conectado a otros equipos.
- Consultar si existe una solicitud o aprobación de uso.

### 3. Analizar el archivo ejecutado

- Obtener el hash del archivo.
- Revisar la reputación del hash en fuentes autorizadas.
- Validar firma digital, tamaño, extensión y fecha de creación.
- Revisar procesos hijos y línea de comandos.
- Identificar archivos creados, modificados o descargados después de la ejecución.

### 4. Revisar posible impacto

- Buscar actividad de red posterior.
- Revisar persistencia mediante tareas programadas, servicios o claves de inicio.
- Ejecutar análisis completo con EDR o antivirus.
- Buscar el hash o nombre de archivo en otros endpoints.
- Validar si se copiaron archivos sensibles al dispositivo USB.

## Clasificación

El caso se clasifica como **uso de dispositivo removible sospechoso**. Debe escalarse como posible compromiso de endpoint si se confirma ejecución de malware, conexiones maliciosas, persistencia o extracción no autorizada de información.

## Acciones recomendadas

- Aislar el equipo si existe evidencia de actividad maliciosa.
- Bloquear o restringir dispositivos removibles según la política.
- Mantener el archivo en cuarentena si fue detectado como malicioso.
- Buscar el mismo dispositivo o archivo en otros activos.
- Revisar controles DLP si existe posibilidad de extracción de datos.
- Informar al equipo de seguridad y documentar el caso.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Replication Through Removable Media | T1091 | Uso de medios removibles para propagar archivos maliciosos |
| User Execution: Malicious File | T1204.002 | Ejecución de un archivo por parte del usuario |
| Ingress Tool Transfer | T1105 | Posible transferencia de herramientas al endpoint |
| Exfiltration Over Physical Medium | T1052 | Posible extracción de datos mediante medios físicos |

## Conclusión

La conexión de un USB no autorizado no confirma un incidente, pero la ejecución de un archivo desde esa unidad incrementa el riesgo. La investigación debe determinar si el dispositivo y el archivo eran legítimos, además de revisar actividad posterior en el endpoint.

## Lecciones aprendidas

- Los dispositivos removibles pueden introducir malware o facilitar la extracción de datos.
- Los controles de USB y DLP reducen la superficie de riesgo.
- El análisis debe correlacionar dispositivo, archivo, usuario y actividad de endpoint.
- Documentar el identificador del USB ayuda a buscar actividad en otros equipos.

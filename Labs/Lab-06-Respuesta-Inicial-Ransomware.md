# Lab 06: Respuesta inicial ante posible ransomware

## Objetivo

Documentar la respuesta inicial ante una alerta que indica posible cifrado masivo de archivos en un endpoint corporativo.

## Escenario

El EDR genera una alerta de severidad crítica porque detecta un proceso que modifica una gran cantidad de archivos y cambia sus extensiones en una estación de trabajo.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Posible comportamiento de ransomware |
| Equipo afectado | `WS-ACCT-007` |
| Usuario asociado | `usuario.ejemplo` |
| Sistema operativo | Windows 11 |
| Proceso detectado | `update_service.exe` |
| Ruta del proceso | `C:\Users\usuario.ejemplo\AppData\Local\Temp\` |
| Archivos modificados | 1,850 en 8 minutos |
| Extensión observada | `.locked` |
| Severidad inicial | Crítica |

> Los equipos, usuarios, rutas, procesos y métricas son ficticios y se utilizan únicamente con fines educativos.

## Hipótesis inicial

La actividad puede corresponder a ransomware o a un proceso malicioso que intenta cifrar archivos locales y recursos compartidos. También se debe descartar una actividad legítima, como una herramienta autorizada de respaldo o migración.

## Indicadores observados

- Cambio masivo de extensiones de archivos.
- Alta velocidad de modificación de documentos.
- Ejecución de un proceso desde una ruta temporal.
- Posible creación de una nota de rescate.
- Intentos de acceso a recursos compartidos.
- Posible eliminación de copias de seguridad locales.

## Acciones inmediatas de contención

1. Aislar el endpoint de la red mediante la herramienta EDR, si existe autorización.
2. No apagar ni reiniciar el equipo, salvo que el procedimiento interno lo indique.
3. Notificar al equipo de respuesta a incidentes y al responsable SOC.
4. Preservar información de la alerta, procesos, hash, usuario, red y hora.
5. Verificar si hay otros equipos con el mismo proceso, extensión o indicadores.
6. Proteger los recursos compartidos potencialmente afectados.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el equipo afectado, usuario, proceso, ruta, hash, hora de inicio y acción aplicada por el EDR.

### 2. Revisar el alcance

- Identificar archivos, carpetas y recursos compartidos afectados.
- Buscar la extensión `.locked` en otros endpoints.
- Revisar si existen notas de rescate.
- Validar si hubo conexiones a servidores o dominios sospechosos.
- Determinar si el usuario tuvo actividad de phishing o descargas recientes.

### 3. Analizar el proceso

- Obtener el hash del archivo detectado.
- Consultar reputación en fuentes autorizadas.
- Revisar proceso padre, línea de comandos y archivos creados.
- Buscar persistencia mediante servicios, tareas programadas o claves de inicio.
- Correlacionar con logs de endpoint, red, correo y autenticación.

### 4. Escalar y documentar

Registrar evidencias, decisiones y acciones en el ticket. Escalar el caso como incidente crítico según el playbook de ransomware de la organización.

## Acciones de recuperación

- Identificar respaldos disponibles y verificar su integridad.
- Erradicar el malware antes de restaurar sistemas.
- Restablecer credenciales si existe riesgo de compromiso.
- Aplicar parches y controles que reduzcan la posibilidad de reinfección.
- Comunicar el estado a los equipos técnicos y de gestión definidos.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Data Encrypted for Impact | T1486 | Cifrado de datos para afectar la disponibilidad |
| Inhibit System Recovery | T1490 | Posible eliminación o alteración de mecanismos de recuperación |
| File and Directory Discovery | T1083 | Búsqueda de archivos y carpetas antes del cifrado |
| Windows Command Shell | T1059.003 | Posible uso de comandos para ejecutar acciones en Windows |

## Conclusión

La modificación masiva de archivos junto con extensiones inusuales y un proceso ejecutado desde una ruta temporal justifica tratar el evento como posible ransomware de severidad crítica. La prioridad es contener, preservar evidencias, determinar el alcance y evitar una propagación.

## Lecciones aprendidas

- La velocidad de contención puede reducir significativamente el impacto.
- Aislar un equipo debe seguir los procedimientos y autorizaciones de la organización.
- Los respaldos probados son esenciales para la recuperación.
- La investigación debe correlacionar evidencias de endpoint, red, correo e identidad.

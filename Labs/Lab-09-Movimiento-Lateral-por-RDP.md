# Lab 09: Investigación de posible movimiento lateral por RDP

## Objetivo

Documentar la investigación de una alerta por conexiones remotas RDP inusuales entre equipos internos de una organización.

## Escenario

El SIEM genera una alerta porque una cuenta administrativa inicia sesión mediante RDP en varios equipos internos en un periodo corto, incluyendo servidores a los que normalmente no accede.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Conexiones RDP internas inusuales |
| Cuenta utilizada | `admin.ejemplo` |
| Equipo origen | `WS-FIN-021` |
| Equipo destino inicial | `SRV-FILES-01` |
| Destinos adicionales | `SRV-APP-02`, `SRV-DB-01` |
| Protocolo | RDP / TCP 3389 |
| Periodo observado | 25 minutos |
| Cantidad de destinos | 3 |
| Severidad inicial | Alta |

> Los usuarios, equipos, direcciones y métricas de este laboratorio son ficticios y se usan exclusivamente con fines educativos.

## Hipótesis inicial

La actividad puede ser una tarea administrativa legítima, pero también puede indicar que un atacante utiliza una cuenta comprometida para acceder a otros sistemas internos y ampliar su alcance.

## Indicadores observados

- Inicio de sesión RDP desde una estación de trabajo no administrativa.
- Uso de una cuenta con privilegios elevados.
- Conexiones a varios servidores en poco tiempo.
- Acceso a sistemas fuera del patrón habitual del usuario.
- Actividad fuera del horario normal.
- Intentos de autenticación fallidos antes o después de accesos exitosos.

## Proceso de investigación

### 1. Validar la alerta

Confirmar la cuenta, equipos origen y destino, fecha, hora, protocolo, eventos de autenticación y la regla que generó la alerta.

### 2. Revisar autenticaciones

- Buscar eventos exitosos y fallidos de la cuenta.
- Revisar inicios de sesión RDP en los equipos destino.
- Identificar la dirección IP interna de origen.
- Comparar con el comportamiento habitual del administrador.
- Validar si existen tickets de mantenimiento o cambios aprobados.

### 3. Analizar el equipo origen

- Revisar si `WS-FIN-021` pertenece realmente al administrador.
- Buscar ejecución de PowerShell, herramientas de administración remota o procesos sospechosos.
- Revisar malware, actividad de red y cambios recientes.
- Confirmar si el endpoint tiene privilegios administrativos autorizados.

### 4. Determinar el alcance

- Identificar todos los sistemas contactados por la cuenta.
- Revisar accesos a recursos compartidos, bases de datos o archivos sensibles.
- Buscar creación de cuentas, tareas programadas o cambios de permisos.
- Identificar otras cuentas utilizadas desde el mismo equipo origen.

## Clasificación

El caso se clasifica inicialmente como **actividad administrativa sospechosa**. Debe escalarse como posible movimiento lateral si no existe justificación, si el origen está comprometido o si se observan acciones no autorizadas en los sistemas destino.

## Acciones recomendadas

- Validar la actividad con el propietario de la cuenta mediante un canal confiable.
- Aislar el equipo origen si existe evidencia de compromiso.
- Revocar sesiones activas y restablecer credenciales si corresponde.
- Restringir RDP según grupos, segmentación de red y listas de acceso.
- Revisar privilegios y accesos administrativos.
- Buscar actividad similar en otros equipos.
- Documentar evidencias y escalar al equipo de respuesta a incidentes.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Remote Services: Remote Desktop Protocol | T1021.001 | Uso de RDP para acceder a sistemas remotos |
| Valid Accounts | T1078 | Uso de cuentas legítimas comprometidas o abusadas |
| Lateral Tool Transfer | T1570 | Posible transferencia de herramientas entre sistemas |
| Remote System Discovery | T1018 | Posible identificación de sistemas internos |

## Conclusión

El uso de RDP desde una estación de trabajo común hacia varios servidores mediante una cuenta privilegiada requiere validación inmediata. Si no existe un cambio aprobado o el origen presenta indicadores de compromiso, se debe tratar como un posible caso de movimiento lateral.

## Lecciones aprendidas

- Las cuentas privilegiadas deben tener patrones de acceso definidos y monitoreados.
- RDP es útil para administración, pero puede ser abusado por atacantes.
- La correlación entre identidad, endpoint y logs de red permite determinar contexto.
- La segmentación de red y el mínimo privilegio reducen el riesgo de movimiento lateral.

# Lab 03: Investigación de inicio de sesión sospechoso en cloud

## Objetivo

Documentar la investigación de una alerta por inicio de sesión inusual en una cuenta corporativa alojada en un servicio cloud.

## Escenario

El SIEM genera una alerta porque un usuario inicia sesión desde una ubicación geográfica inusual y, pocos minutos después, realiza varios intentos fallidos de autenticación.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Inicio de sesión sospechoso |
| Usuario afectado | `analista.ejemplo@empresa.local` |
| Servicio | Microsoft 365 / Entra ID |
| IP de origen | `198.51.100.78` |
| Ubicación detectada | País no habitual |
| Método de autenticación | Contraseña |
| MFA | No completado |
| Hora del evento | 02:15 UTC |
| Severidad inicial | Alta |

> Los datos, direcciones IP, usuarios y ubicaciones de este laboratorio son ficticios y se usan exclusivamente con fines educativos.

## Hipótesis inicial

La actividad puede indicar un intento de acceso no autorizado mediante credenciales comprometidas. También puede tratarse de una conexión legítima desde una VPN, viaje, proveedor autorizado o falso positivo de geolocalización.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el usuario, la dirección IP, la fecha, la hora, el servicio afectado y la regla que generó la alerta.

### 2. Revisar el historial de autenticación

- Buscar inicios de sesión exitosos y fallidos del usuario.
- Comparar IP, ubicación, dispositivo y agente de usuario con eventos anteriores.
- Revisar si el usuario utiliza VPN o viaja con frecuencia.
- Validar si existe autenticación multifactor configurada.
- Identificar intentos desde otras IP relacionadas.

### 3. Analizar actividad posterior

- Revisar accesos a correo, archivos y aplicaciones corporativas.
- Buscar cambios de contraseña o registro de nuevos métodos MFA.
- Verificar reglas de reenvío creadas en el correo.
- Identificar creación de aplicaciones, tokens o permisos inusuales.
- Revisar descargas masivas o acceso a información sensible.

### 4. Consultar al usuario

Contactar al usuario mediante un canal confiable para confirmar si reconoce el inicio de sesión. No utilizar enlaces ni datos suministrados en un correo sospechoso para verificar su identidad.

## Clasificación

El caso se clasifica inicialmente como **posible compromiso de cuenta**. La severidad aumenta si se confirma acceso exitoso, modificación de MFA, creación de reglas de correo, descargas inusuales o actividad desde múltiples ubicaciones.

## Acciones recomendadas

- Revocar sesiones activas del usuario.
- Restablecer la contraseña si existe evidencia de compromiso.
- Exigir registro o verificación de MFA.
- Bloquear la IP si es maliciosa y la política lo permite.
- Eliminar reglas de reenvío o configuraciones no autorizadas.
- Revisar permisos, tokens y aplicaciones asociadas a la cuenta.
- Documentar evidencias, acciones y escalamiento en el ticket.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Valid Accounts | T1078 | Posible uso de credenciales legítimas comprometidas |
| External Remote Services | T1133 | Acceso a servicios corporativos desde Internet |
| Email Collection | T1114 | Posible acceso al correo de la cuenta comprometida |
| Account Manipulation | T1098 | Posible modificación de métodos MFA, permisos o reglas |

## Conclusión

Un inicio de sesión desde una ubicación inusual no confirma por sí solo un compromiso. Sin embargo, la ausencia de MFA completado, múltiples fallos de autenticación y actividad posterior anómala justifican investigar el caso con prioridad alta.

## Lecciones aprendidas

- La geolocalización es un indicador, no una prueba definitiva.
- Los logs de identidad permiten reconstruir la actividad de una cuenta.
- MFA, contraseñas seguras y revisión de sesiones reducen el impacto de credenciales comprometidas.
- La actividad posterior al inicio de sesión es clave para determinar el alcance de un incidente.

# Lab 10: Investigación de fuerza bruta contra VPN

## Objetivo

Documentar la investigación de múltiples intentos fallidos de autenticación contra un portal VPN corporativo.

## Escenario

El SIEM genera una alerta porque una misma dirección IP externa realiza numerosos intentos fallidos de inicio de sesión contra varias cuentas de usuarios en el portal VPN.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Múltiples fallos de autenticación VPN |
| Servicio afectado | VPN corporativa |
| IP de origen | `198.51.100.145` |
| Periodo observado | 20 minutos |
| Intentos fallidos | 180 |
| Cuentas objetivo | 12 |
| Autenticaciones exitosas | 0 |
| MFA | Habilitado |
| Severidad inicial | Media |

> Las direcciones IP, usuarios, equipos y métricas de este laboratorio son ficticios y se utilizan únicamente con fines educativos.

## Hipótesis inicial

La actividad puede ser un ataque de password spraying o fuerza bruta contra cuentas corporativas. También debe descartarse un cliente VPN mal configurado, credenciales guardadas incorrectamente o actividad legítima desde un proveedor autorizado.

## Indicadores observados

- Alto número de fallos desde una misma IP externa.
- Múltiples cuentas objetivo en un periodo corto.
- Uso repetitivo de credenciales incorrectas.
- Actividad fuera del horario habitual.
- Intentos contra cuentas privilegiadas o de uso frecuente.
- Posibles inicios de sesión exitosos posteriores.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el origen, número de intentos, cuentas afectadas, periodo observado, método de autenticación y regla que generó el evento.

### 2. Analizar el patrón

- Determinar si una sola cuenta recibe muchos intentos o si se atacan varias cuentas.
- Revisar si la IP cambia durante el ataque.
- Identificar si los intentos siguen intervalos regulares.
- Comparar la actividad con intentos de VPN legítimos previos.
- Verificar si el origen pertenece a una ubicación aprobada o VPN conocida.

### 3. Buscar impacto

- Revisar autenticaciones exitosas posteriores desde la misma IP.
- Validar si las cuentas afectadas recibieron solicitudes MFA.
- Buscar cambios de contraseña, registro de nuevos dispositivos o métodos MFA.
- Revisar actividad posterior en correo, archivos y aplicaciones corporativas.
- Determinar si otras fuentes externas atacan las mismas cuentas.

### 4. Clasificar el caso

Si no hay autenticaciones exitosas, clasificar como **ataque bloqueado o intento de fuerza bruta**. Si existe un acceso exitoso sospechoso, escalar como **posible compromiso de cuenta**.

## Acciones recomendadas

- Bloquear temporalmente la IP de origen según la política de la organización.
- Aplicar límites de intentos y mecanismos de bloqueo de cuentas.
- Confirmar que MFA está habilitado y funcionando.
- Notificar a los usuarios afectados si existe riesgo de compromiso.
- Restablecer contraseñas si se detecta acceso exitoso no autorizado.
- Buscar patrones similares en otros servicios expuestos a Internet.
- Documentar evidencias, impacto y acciones tomadas.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Password Guessing | T1110.001 | Intentos repetidos contra cuentas específicas |
| Password Spraying | T1110.003 | Intentos contra múltiples cuentas con pocas contraseñas |
| External Remote Services | T1133 | Uso de servicios expuestos como VPN |
| Valid Accounts | T1078 | Posible uso de credenciales si el ataque tiene éxito |

## Conclusión

Un alto volumen de fallos de autenticación desde una IP externa contra varias cuentas es consistente con password spraying o fuerza bruta. La ausencia de accesos exitosos reduce el impacto inmediato, pero debe revisarse la actividad posterior y reforzarse la protección de cuentas.

## Lecciones aprendidas

- Los patrones de autenticación ayudan a diferenciar fuerza bruta y password spraying.
- MFA reduce el riesgo, pero no elimina la necesidad de monitoreo.
- Los servicios remotos expuestos a Internet requieren controles de acceso y alertas.
- La revisión de accesos exitosos posteriores es esencial para medir el impacto.

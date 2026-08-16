# Lab 01: Análisis de fuerza bruta contra SSH

## Objetivo

Simular y documentar el proceso de investigación de múltiples intentos fallidos de autenticación SSH para determinar si existe un posible ataque de fuerza bruta.

## Escenario

Un sistema de monitoreo genera una alerta por varios intentos fallidos de inicio de sesión hacia un servidor Linux mediante el puerto 22/TCP.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Múltiples fallos de autenticación SSH |
| Activo afectado | `linux-server-01` |
| Servicio | SSH |
| Puerto | 22/TCP |
| Usuario objetivo | `admin` |
| IP de origen | `203.0.113.25` |
| Periodo observado | 10 minutos |
| Intentos fallidos | 45 |
| Severidad inicial | Media |

> Las direcciones IP, nombres de equipos y usuarios de este laboratorio son datos ficticios para fines educativos.

## Hipótesis inicial

La actividad puede corresponder a un ataque de fuerza bruta, donde un actor intenta adivinar credenciales mediante múltiples intentos de autenticación.

## Proceso de investigación

### 1. Validar la alerta

Confirmar que los eventos provienen de los logs de autenticación del servidor y que no se trata de una prueba autorizada, un sistema de monitoreo o una cuenta de servicio configurada incorrectamente.

### 2. Revisar evidencia disponible

Ejemplo de registros a buscar:

```text
Failed password for admin from 203.0.113.25 port 49822 ssh2
Failed password for admin from 203.0.113.25 port 49824 ssh2
Failed password for invalid user test from 203.0.113.25 port 49830 ssh2
```

Comandos de consulta en Linux:

```bash
sudo grep "Failed password" /var/log/auth.log
sudo grep "203.0.113.25" /var/log/auth.log
sudo lastb
```

### 3. Obtener contexto

- Verificar si la IP de origen pertenece a la organización o a un proveedor conocido.
- Revisar si hubo autenticaciones exitosas desde la misma IP.
- Validar si el usuario `admin` existe y si debería tener acceso remoto.
- Determinar si existen otros servidores objetivo.
- Consultar si hay actividad relacionada en firewall, IDS/IPS o SIEM.

### 4. Clasificación

La alerta se clasifica inicialmente como **actividad sospechosa**. Si se confirma un inicio de sesión exitoso posterior a los fallos, debe escalarse como posible compromiso de cuenta.

## Acciones recomendadas

- Bloquear temporalmente la IP si no es legítima y existe autorización.
- Aplicar autenticación mediante llaves SSH.
- Deshabilitar el acceso remoto directo de cuentas administrativas.
- Habilitar autenticación multifactor cuando sea posible.
- Configurar límites de intentos y herramientas como Fail2ban.
- Revisar contraseñas y accesos autorizados.
- Documentar el análisis en un ticket.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Password Guessing | T1110.001 | Intentos repetidos para adivinar contraseñas |
| Valid Accounts | T1078 | Posible uso de una cuenta válida si un intento tiene éxito |
| Remote Services: SSH | T1021.004 | Acceso remoto al servidor mediante SSH |

## Conclusión

Los 45 intentos fallidos desde una IP externa contra una cuenta administrativa en un periodo corto son consistentes con un posible intento de fuerza bruta. No se observó autenticación exitosa en este escenario, por lo que el caso puede cerrarse como actividad bloqueada o escalarse para aplicar controles preventivos, según el procedimiento de la organización.

## Lecciones aprendidas

- Los logs de autenticación son esenciales para investigar accesos remotos.
- La cantidad, frecuencia y origen de los intentos ayudan a priorizar una alerta.
- La revisión de inicios de sesión exitosos es crítica para determinar impacto.
- Los controles de acceso y la autenticación fuerte reducen el riesgo de compromiso.

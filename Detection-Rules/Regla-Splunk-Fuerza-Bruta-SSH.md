# Regla de detección: posible fuerza bruta contra SSH

## Objetivo

Detectar múltiples intentos fallidos de autenticación SSH desde una misma dirección IP contra uno o varios usuarios en un intervalo corto.

## Fuente de logs

- Servidor Linux
- Archivo: `/var/log/auth.log`
- Eventos relevantes: `Failed password`, `Invalid user`
- Plataforma SIEM: Splunk

## Lógica de detección

Generar una alerta cuando una dirección IP de origen produzca 10 o más intentos fallidos de autenticación SSH en un periodo de 5 minutos.

## Consulta SPL

```spl
index=linux sourcetype=linux_secure ("Failed password" OR "Invalid user")
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| stats count values(user) as targeted_users values(host) as targeted_hosts by src_ip
| where count >= 10
| sort - count
```

> Ajusta `index=linux` y `sourcetype=linux_secure` según la configuración real de tu entorno.

## Campos relevantes

| Campo | Descripción |
|---|---|
| `src_ip` | Dirección IP desde la cual se originan los intentos |
| `host` | Servidor Linux afectado |
| `user` | Cuenta objetivo del intento de autenticación |
| `count` | Cantidad de eventos fallidos detectados |
| `targeted_users` | Usuarios atacados desde la misma IP |
| `targeted_hosts` | Sistemas objetivo asociados |

## Severidad sugerida

| Condición | Severidad |
|---|---|
| 10 a 24 intentos en 5 minutos | Media |
| 25 a 49 intentos en 5 minutos | Alta |
| 50 o más intentos en 5 minutos | Crítica |
| Intento exitoso posterior desde la misma IP | Crítica |

## Proceso de triage

1. Validar la IP de origen y confirmar si pertenece a una fuente autorizada.
2. Revisar los usuarios y servidores afectados.
3. Buscar inicios de sesión exitosos desde la misma IP.
4. Revisar eventos de firewall, IDS/IPS y EDR relacionados.
5. Clasificar el caso como falso positivo, actividad sospechosa o intento de fuerza bruta.
6. Escalar o bloquear la IP según el procedimiento autorizado.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Password Guessing | T1110.001 | Intentos repetidos de adivinar contraseñas |
| Remote Services: SSH | T1021.004 | Acceso remoto mediante SSH |
| Valid Accounts | T1078 | Posible uso de credenciales válidas si el ataque tiene éxito |

## Validación en home lab

Para validar esta regla en un entorno autorizado:

- Configurar una VM Ubuntu o Linux con SSH habilitado.
- Instalar y configurar un forwarder o agente para enviar `auth.log` al SIEM.
- Generar intentos fallidos controlados desde una VM Kali o un segundo host de laboratorio.
- Confirmar que los eventos se visualizan en Splunk.
- Ejecutar la consulta SPL y comprobar que se activa la alerta.
- Guardar una captura sanitizada del resultado en la carpeta `Evidence`.

## Resultado esperado

La consulta debe identificar la IP de origen, el número de fallos, los usuarios objetivo y los servidores afectados, facilitando el triage inicial de un posible ataque de fuerza bruta.

## Nota ética

Las pruebas deben realizarse únicamente contra máquinas virtuales, cuentas y redes propias o expresamente autorizadas.

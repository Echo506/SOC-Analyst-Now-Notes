# Lab 12: Detección de posible beaconing C2

## Objetivo

Documentar la investigación de conexiones periódicas desde un endpoint hacia un dominio externo potencialmente asociado a comando y control.

## Escenario

El SIEM detecta que una estación de trabajo realiza conexiones HTTPS hacia el mismo dominio externo cada 60 segundos durante varias horas.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Patrón de comunicación periódica |
| Equipo afectado | `WS-MKT-009` |
| Usuario asociado | `usuario.ejemplo` |
| Dominio destino | `update-check.example` |
| Protocolo | HTTPS / TCP 443 |
| Frecuencia | Una conexión cada 60 segundos |
| Periodo observado | 4 horas |
| Volumen total | 18 MB |
| Severidad inicial | Alta |

> Los nombres, dominios, direcciones y métricas son ficticios y se utilizan únicamente con fines educativos.

## Hipótesis inicial

La comunicación periódica puede ser legítima, por ejemplo una actualización de software o telemetría autorizada. Sin embargo, también puede indicar beaconing: un patrón utilizado por malware para recibir instrucciones de un servidor de comando y control.

## Indicadores observados

- Intervalos regulares entre conexiones.
- Dominio no identificado como servicio corporativo aprobado.
- Comunicación desde un único endpoint.
- Persistencia del patrón durante un periodo prolongado.
- Agente de usuario, proceso o certificado inusual.
- Cambios en la frecuencia o volumen de tráfico.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el endpoint, usuario, dominio, IP destino, frecuencia, periodo de observación y regla que generó la alerta.

### 2. Analizar la comunicación

- Revisar logs de proxy, firewall, DNS y EDR.
- Identificar el proceso que abrió la conexión.
- Validar si el dominio pertenece a un proveedor legítimo.
- Revisar reputación del dominio, IP y certificado.
- Comparar frecuencia, tamaño de paquetes y horarios de conexión.

### 3. Investigar el endpoint

- Revisar procesos activos y árbol de procesos.
- Buscar archivos recientes en rutas temporales, descargas o `AppData`.
- Revisar tareas programadas, servicios y claves de inicio.
- Identificar conexiones adicionales al mismo dominio.
- Buscar el mismo proceso o dominio en otros endpoints.

### 4. Determinar alcance

- Identificar todos los equipos que se comunican con el dominio.
- Revisar si existen descargas, comandos o transferencia de archivos.
- Correlacionar con eventos de phishing, PowerShell, malware o credenciales.
- Determinar si la comunicación continuó después de bloquear el dominio.

## Clasificación

El evento se clasifica como **posible beaconing de comando y control**. Debe escalarse como incidente de alta prioridad si se confirma que un proceso malicioso mantiene comunicación con infraestructura no autorizada.

## Acciones recomendadas

- Bloquear el dominio e IP asociados si existe autorización.
- Aislar el endpoint si se confirman indicadores de malware.
- Preservar logs de red, DNS, proxy y endpoint.
- Buscar indicadores equivalentes en otros equipos.
- Ejecutar análisis EDR o antivirus en el activo afectado.
- Revisar persistencia y credenciales potencialmente comprometidas.
- Documentar el caso y escalar al equipo de respuesta a incidentes.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Application Layer Protocol: Web Protocols | T1071.001 | Uso de HTTP o HTTPS para comunicación de comando y control |
| Encrypted Channel | T1573 | Uso de canales cifrados para ocultar comunicaciones |
| Proxy | T1090 | Posible uso de infraestructura intermedia para ocultar el destino |
| Scheduled Task/Job | T1053 | Posible mecanismo de persistencia para mantener la comunicación |

## Conclusión

Las conexiones HTTPS con una periodicidad exacta hacia un dominio no reconocido pueden ser una señal de beaconing. El análisis del proceso origen, la reputación del destino y la presencia de persistencia permite diferenciar telemetría legítima de una posible actividad de comando y control.

## Lecciones aprendidas

- La frecuencia de red es un indicador relevante en detección de amenazas.
- HTTPS puede ocultar contenido, pero los metadatos de conexión siguen siendo útiles.
- Correlacionar red, DNS, proxy y endpoint permite obtener mayor contexto.
- El bloqueo del dominio debe ir acompañado de investigación en el host afectado.

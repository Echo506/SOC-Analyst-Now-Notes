# Lab 05: Detección de posible exfiltración de datos por DNS

## Objetivo

Documentar la investigación de actividad DNS anómala que podría indicar tunneling DNS o una posible exfiltración de datos.

## Escenario

El SIEM genera una alerta por un volumen elevado de consultas DNS desde una estación de trabajo hacia un dominio externo desconocido. Las consultas contienen subdominios largos, aleatorios y con alta frecuencia.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Consultas DNS anómalas |
| Equipo afectado | `WS-OPS-018` |
| Usuario asociado | `usuario.ejemplo` |
| Servidor DNS | `dns-interno-01` |
| Dominio consultado | `x7a92kq1m.data-sync.example` |
| Protocolo | DNS / UDP 53 |
| Periodo observado | 15 minutos |
| Cantidad de consultas | 1,250 |
| Severidad inicial | Alta |

> Los equipos, usuarios, dominios y métricas son ficticios y se utilizan únicamente con fines educativos.

## Hipótesis inicial

La actividad puede corresponder a DNS tunneling, una técnica que utiliza consultas DNS para ocultar comandos, datos codificados o comunicaciones con infraestructura controlada por un atacante.

## Indicadores observados

- Alto volumen de consultas DNS desde un único equipo.
- Subdominios largos o generados de forma aparentemente aleatoria.
- Dominio poco frecuente o recién observado en el entorno.
- Consultas repetitivas a intervalos regulares.
- Tráfico DNS fuera del patrón habitual de la organización.
- Posibles cadenas codificadas en los subdominios.

## Proceso de investigación

### 1. Validar la alerta

Confirmar el equipo, usuario, dominio consultado, número de eventos, fechas, horas y la regla que generó la alerta.

### 2. Revisar logs DNS

Buscar patrones de consultas, longitud de subdominios, frecuencia y respuestas recibidas.

Ejemplo de consulta:

```text
a8d72f1k3m9q.data-sync.example
b9e83g2l4n0r.data-sync.example
c0f94h3m5p1s.data-sync.example
```

### 3. Obtener contexto

- Verificar si el dominio está aprobado o pertenece a un proveedor legítimo.
- Consultar reputación del dominio e IPs asociadas.
- Revisar si otros equipos realizan consultas al mismo dominio.
- Identificar el proceso que generó las consultas DNS.
- Validar instalaciones recientes, tareas programadas o scripts ejecutados.
- Revisar conexiones de red adicionales desde el endpoint.

### 4. Analizar posible codificación

Las cadenas largas en subdominios pueden contener datos codificados. Se debe preservar la evidencia para análisis posterior y evitar decodificar información sensible fuera de procedimientos autorizados.

### 5. Determinar alcance

- Identificar cuántos equipos contactaron el dominio.
- Revisar cuándo inició la actividad.
- Buscar archivos, procesos o tareas comunes entre los endpoints afectados.
- Verificar si existen otros indicadores relacionados con malware o compromiso de cuenta.

## Clasificación

El caso se clasifica como **actividad sospechosa de DNS tunneling**. Debe escalarse a incidente de alta prioridad si se confirma que existe transferencia de información, malware activo o comunicación con infraestructura maliciosa.

## Acciones recomendadas

- Bloquear el dominio mediante DNS firewall o proxy, si existe autorización.
- Aislar el endpoint si se confirma actividad maliciosa.
- Preservar logs DNS, red y endpoint para análisis.
- Buscar indicadores similares en otros activos.
- Ejecutar análisis EDR o antivirus en el equipo afectado.
- Revisar procesos, persistencia y tareas programadas.
- Documentar la investigación y escalar al equipo de respuesta a incidentes.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Application Layer Protocol: DNS | T1071.004 | Uso del protocolo DNS para comunicaciones de comando y control |
| Exfiltration Over C2 Channel | T1041 | Posible transferencia de información por un canal de comando y control |
| Dynamic Resolution | T1568 | Posible uso de infraestructura dinámica para resolver dominios controlados por un atacante |

## Conclusión

El alto volumen de consultas con subdominios largos y aleatorios hacia un dominio no reconocido es consistente con una posible actividad de DNS tunneling. La investigación debe correlacionar logs DNS, red y endpoint antes de confirmar exfiltración.

## Lecciones aprendidas

- DNS es esencial para la operación, pero también puede ser abusado por atacantes.
- La frecuencia, longitud y patrón de los subdominios ayudan a detectar anomalías.
- El análisis debe incluir contexto de red, reputación e información del endpoint.
- Bloquear un dominio puede contener el riesgo, pero se debe investigar la causa en el host afectado.

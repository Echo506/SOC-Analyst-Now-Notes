# Herramientas de un SOC

## Introducción

Un Security Operations Center utiliza diferentes herramientas para recolectar eventos, detectar comportamientos sospechosos, investigar alertas y responder ante incidentes.

## SIEM

Un Security Information and Event Management (SIEM) centraliza logs de distintos dispositivos y sistemas. Permite correlacionar eventos, generar alertas y apoyar investigaciones.

### Ejemplos
- Microsoft Sentinel
- Splunk
- IBM QRadar
- Elastic Security
- Wazuh

## EDR

Un Endpoint Detection and Response (EDR) monitorea equipos como laptops, servidores y estaciones de trabajo. Ayuda a detectar procesos sospechosos, malware, cambios en archivos y actividad anómala.

### Ejemplos
- Microsoft Defender for Endpoint
- CrowdStrike Falcon
- SentinelOne
- Sophos Intercept X

## IDS e IPS

Un Intrusion Detection System (IDS) detecta actividad sospechosa en la red. Un Intrusion Prevention System (IPS) puede bloquear o prevenir tráfico malicioso según sus reglas y configuración.

### Ejemplos
- Suricata
- Snort
- Zeek

## Threat Intelligence

Las plataformas de inteligencia de amenazas ayudan a investigar indicadores de compromiso, como direcciones IP, dominios, URLs y hashes.

### Fuentes y herramientas
- VirusTotal
- AbuseIPDB
- AlienVault OTX
- Talos Intelligence
- MISP

## Gestión de vulnerabilidades

Estas herramientas identifican vulnerabilidades, configuraciones inseguras y software desactualizado en los activos de una organización.

### Ejemplos
- Nessus
- OpenVAS
- Qualys
- Rapid7 InsightVM

## Criterio de uso

Las herramientas generan información, pero no sustituyen el análisis humano. Un SOC Analyst debe validar el contexto, determinar la severidad y documentar las conclusiones antes de escalar o cerrar una alerta.

## Reflexión personal

El conocimiento de herramientas SOC debe acompañarse de fundamentos de redes, sistemas operativos, logs y respuesta a incidentes. La herramienta muestra señales; el analista investiga y toma decisiones basadas en evidencia.

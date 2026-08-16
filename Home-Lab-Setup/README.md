# Personal SOC Home Lab

## Objetivo

Construir un entorno controlado para practicar monitoreo, detección, análisis de logs, threat hunting y respuesta a incidentes.

## Componentes

| Componente | Herramienta | Propósito |
|---|---|---|
| SIEM | Wazuh, Splunk Free o Elastic Security | Ingesta de logs, alertas y dashboards |
| Virtualización | VirtualBox o VMware | Ejecución aislada de máquinas virtuales |
| Endpoint Windows | Windows 10/11 con Sysmon | Generación de eventos y telemetría |
| Endpoint Linux | Ubuntu Server | Logs SSH, syslog y pruebas de red |
| Máquina de pruebas | Kali Linux | Simulaciones controladas y autorizadas |
| Análisis de red | Wireshark y tcpdump | Captura y análisis de tráfico |
| IDS | Suricata o Zeek | Detección y visibilidad de red |

## Alcance y ética

Todas las simulaciones se realizan únicamente en un entorno personal, aislado y autorizado. No se realizan pruebas contra sistemas, redes o cuentas de terceros.

## Evidencia documentada

- Capturas sanitizadas de dashboards y alertas.
- Consultas de detección utilizadas.
- Logs relevantes de Windows, Linux y red.
- Reportes de incidentes simulados.
- Lecciones aprendidas y mejoras aplicadas.

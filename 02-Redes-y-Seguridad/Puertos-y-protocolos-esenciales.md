# Puertos y protocolos esenciales para un SOC Analyst

## Introducción

El análisis de red permite identificar comunicaciones normales, configuraciones inseguras e indicadores de actividad maliciosa. Un analista SOC debe reconocer los protocolos y puertos más comunes para interpretar alertas y logs.

## Puertos frecuentes

| Puerto | Protocolo | Uso habitual | Consideración de seguridad |
|---:|---|---|---|
| 20/21 | FTP | Transferencia de archivos | Puede transmitir credenciales sin cifrado |
| 22 | SSH | Administración remota segura | Vigilar fuerza bruta e inicios de sesión anómalos |
| 23 | Telnet | Administración remota | Inseguro; transmite datos sin cifrar |
| 25 | SMTP | Envío de correo electrónico | Revisar phishing, spam y relay no autorizado |
| 53 | DNS | Resolución de nombres | Detectar dominios sospechosos y túneles DNS |
| 80 | HTTP | Tráfico web | No cifra la comunicación |
| 443 | HTTPS | Tráfico web cifrado | Revisar dominios, certificados y patrones anómalos |
| 3389 | RDP | Escritorio remoto | Objetivo frecuente de ataques de fuerza bruta |

## TCP y UDP

TCP establece una conexión y busca entregar los datos de forma confiable. UDP es más rápido y no establece una conexión previa, por lo que se utiliza en servicios como DNS, streaming y algunas aplicaciones en tiempo real.

## Qué validar ante una alerta de red

- Dirección IP de origen y destino.
- Puerto, protocolo y servicio asociado.
- Fecha y hora del evento.
- Usuario, equipo o servidor involucrado.
- Volumen y frecuencia de conexiones.
- Dominio consultado, si existe tráfico DNS o web.
- Reputación de la IP o dominio.
- Actividad relacionada en otros logs.

## Ejemplo de investigación

Una gran cantidad de intentos de conexión al puerto 22 desde una misma IP externa puede indicar un posible ataque de fuerza bruta contra SSH. El analista debe confirmar si hubo autenticaciones exitosas, revisar la cuenta afectada, validar si la IP es conocida y escalar el caso según la severidad.

## Reflexión personal

Conocer puertos y protocolos permite priorizar alertas y comprender mejor el comportamiento de una red. Esta base es esencial para investigar posibles incidentes de forma ordenada.

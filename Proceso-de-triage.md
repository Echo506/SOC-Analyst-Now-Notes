# Proceso de triage en un SOC

## Definición

El triage es el proceso inicial de analizar una alerta para determinar si representa una amenaza real, su impacto potencial, prioridad y necesidad de escalamiento.

## Objetivos

- Distinguir falsos positivos de eventos relevantes.
- Establecer la severidad del evento.
- Recolectar evidencias iniciales.
- Determinar si se requiere contención o escalamiento.
- Documentar las acciones y conclusiones.

## Flujo de triage

1. **Recibir la alerta:** identificar la fuente, regla activada, fecha, hora y severidad inicial.
2. **Validar el evento:** confirmar que la alerta corresponde a actividad real y revisar si es esperada.
3. **Obtener contexto:** identificar usuario, host, IP de origen y destino, dominio, proceso o archivo relacionado.
4. **Analizar indicadores:** revisar reputación de IP, dominio, hash o URL; buscar eventos relacionados.
5. **Clasificar la alerta:** falso positivo, actividad benigna, evento sospechoso o incidente confirmado.
6. **Asignar prioridad:** considerar impacto, criticidad del activo, alcance, evidencia y urgencia.
7. **Escalar o contener:** seguir el playbook o procedimiento de respuesta definido por la organización.
8. **Documentar y cerrar:** registrar evidencias, decisiones, acciones, recomendaciones y estado final.

## Información que se debe documentar

- Identificador de alerta o ticket.
- Fecha y hora del evento.
- Usuario y activo involucrados.
- Direcciones IP, dominios, URLs, hashes o archivos asociados.
- Herramientas y fuentes de logs consultadas.
- Evidencias encontradas.
- Clasificación y severidad final.
- Acciones tomadas y persona o equipo al que se escaló.

## Ejemplo: alerta de phishing

Ante una alerta de correo sospechoso, se debe revisar el remitente, asunto, destinatarios, enlaces, adjuntos, reputación del dominio y cualquier actividad posterior del usuario. Si el usuario hizo clic o descargó un archivo, se deben revisar los logs del endpoint, conexiones de red y autenticaciones relacionadas.

## Reflexión personal

El triage requiere una metodología ordenada para evitar decisiones apresuradas. Una buena documentación permite que otros analistas comprendan el caso y facilita la respuesta ante incidentes.

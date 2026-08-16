# Automatización en un SOC

## Definición

La automatización en un Security Operations Center consiste en usar scripts, reglas, integraciones y plataformas para ejecutar tareas repetitivas de manera consistente y rápida.

## Objetivos

- Reducir el tiempo de respuesta ante alertas.
- Disminuir tareas manuales repetitivas.
- Enriquecer automáticamente indicadores de compromiso.
- Estandarizar pasos de investigación.
- Permitir que los analistas se concentren en casos complejos.

## Ejemplos de automatización

- Consultar la reputación de una IP, dominio o hash.
- Crear tickets cuando una alerta supera un nivel de severidad.
- Enriquecer una alerta con datos de threat intelligence.
- Bloquear un dominio o IP maliciosa aprobada.
- Deshabilitar temporalmente una cuenta comprometida.
- Enviar notificaciones al equipo de respuesta.
- Generar reportes diarios de eventos críticos.

## SOAR

Security Orchestration, Automation and Response (SOAR) integra herramientas de seguridad y ejecuta flujos de trabajo conocidos como playbooks. Un playbook define pasos claros para investigar y responder a un tipo de alerta.

## Precauciones

La automatización debe revisarse y probarse antes de aplicarse en producción. Una acción incorrecta, como bloquear un servicio legítimo o deshabilitar una cuenta crítica, puede afectar la operación de la organización.

## Ejemplo de playbook de phishing

1. Recibir alerta de correo sospechoso.
2. Extraer remitente, URL, adjuntos y destinatarios.
3. Consultar reputación de los indicadores.
4. Buscar mensajes similares en el correo corporativo.
5. Identificar usuarios que hicieron clic.
6. Crear ticket y notificar al analista.
7. Bloquear indicadores solo con aprobación o reglas definidas.

## Reflexión personal

La automatización no elimina la necesidad de analistas SOC. Su valor consiste en acelerar tareas repetitivas y permitir que el equipo dedique más tiempo al análisis, la validación y la toma de decisiones.

# Seguridad en la nube para un SOC

## Introducción

Los entornos cloud permiten utilizar recursos tecnológicos bajo demanda, pero también requieren monitoreo, configuración segura y controles de acceso adecuados. Un analista SOC debe comprender cómo investigar eventos en servicios cloud.

## Modelos de servicio

| Modelo | Responsabilidad del proveedor | Responsabilidad del cliente |
|---|---|---|
| IaaS | Infraestructura física y virtualización | Sistemas operativos, red, aplicaciones y datos |
| PaaS | Infraestructura, sistemas base y plataforma | Aplicaciones, configuraciones y datos |
| SaaS | Aplicación, infraestructura y plataforma | Usuarios, permisos, datos y configuración de uso |

## Riesgos frecuentes

- Credenciales expuestas o comprometidas.
- Configuraciones incorrectas de almacenamiento.
- Permisos excesivos en cuentas y roles.
- Falta de autenticación multifactor.
- Llaves de acceso expuestas en código o repositorios.
- Registros de auditoría deshabilitados.
- Recursos expuestos directamente a Internet.

## Controles recomendados

- Aplicar autenticación multifactor.
- Usar el principio de mínimo privilegio.
- Habilitar logs de auditoría y monitoreo.
- Revisar periódicamente permisos, roles y cuentas inactivas.
- Cifrar datos almacenados y en tránsito.
- Gestionar secretos mediante herramientas especializadas.
- Detectar configuraciones inseguras de forma continua.
- Mantener respaldos y procedimientos de respuesta a incidentes.

## Investigación de una alerta cloud

Ante un inicio de sesión sospechoso, se debe validar la cuenta utilizada, dirección IP, ubicación aproximada, método de autenticación, permisos de la cuenta, acciones realizadas y cambios posteriores en recursos cloud.

## Reflexión personal

En la nube, la seguridad es una responsabilidad compartida. El proveedor protege la infraestructura subyacente, mientras que la organización debe proteger sus identidades, configuraciones, aplicaciones y datos.

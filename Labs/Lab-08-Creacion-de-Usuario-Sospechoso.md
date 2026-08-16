# Lab 08: Investigación de creación de usuario sospechoso

## Objetivo

Documentar la investigación de una alerta relacionada con la creación no autorizada de una cuenta de usuario con posibles privilegios elevados.

## Escenario

El SIEM genera una alerta al detectar la creación de una nueva cuenta local o de dominio fuera del horario habitual y desde un equipo administrativo.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Creación de cuenta sospechosa |
| Equipo origen | `ADMIN-WS-02` |
| Usuario que ejecutó la acción | `admin.ejemplo` |
| Cuenta creada | `support_backup` |
| Grupo asociado | `Administrators` |
| Hora del evento | 03:12 AM |
| Severidad inicial | Alta |

> Los nombres de usuarios, equipos y grupos son ficticios y se usan únicamente con fines educativos.

## Hipótesis inicial

La actividad puede corresponder a una acción administrativa legítima, pero también puede indicar compromiso de una cuenta privilegiada, persistencia maliciosa o creación de una cuenta para acceso no autorizado posterior.

## Proceso de investigación

### 1. Validar la alerta

Confirmar quién creó la cuenta, en qué sistema, a qué hora y qué privilegios recibió.

### 2. Revisar contexto

- Verificar si existe ticket o cambio aprobado.
- Confirmar si el usuario administrador reconoce la acción.
- Revisar si el horario coincide con actividad habitual.
- Validar si el equipo origen es de uso administrativo autorizado.

### 3. Revisar eventos relacionados

- Buscar eventos de agregado a grupos privilegiados.
- Revisar inicios de sesión previos del usuario administrador.
- Buscar cambios de contraseña, desactivación de logs o creación de otras cuentas.
- Revisar si hubo autenticaciones remotas o RDP antes del evento.

### 4. Evaluar impacto

- Determinar si la nueva cuenta ya inició sesión.
- Validar si accedió a recursos sensibles.
- Revisar si hay actividad en otros equipos usando la misma cuenta.
- Confirmar si la cuenta fue usada para persistencia o movimiento lateral.

## Clasificación

El caso se clasifica como **cambio administrativo sospechoso** y debe escalarse como posible compromiso de cuenta privilegiada si no existe justificación válida o si la nueva cuenta presenta actividad posterior.

## Acciones recomendadas

- Deshabilitar temporalmente la cuenta creada si no está autorizada.
- Restablecer credenciales de la cuenta administrativa involucrada.
- Revisar MFA y accesos recientes del administrador.
- Buscar actividad asociada en otros activos.
- Documentar hallazgos y escalar al equipo correspondiente.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| Create Account | T1136 | Creación de cuentas para persistencia o acceso |
| Account Manipulation | T1098 | Modificación de cuentas o privilegios |
| Valid Accounts | T1078 | Uso de cuentas legítimas o creadas para acceso posterior |

## Conclusión

La creación de una cuenta privilegiada fuera de horario y sin evidencia inmediata de aprobación es una señal de riesgo. La investigación debe confirmar legitimidad, revisar actividad posterior y determinar si existe compromiso de una cuenta administrativa.

## Lecciones aprendidas

- Los cambios administrativos deben validarse con contexto operativo.
- Las cuentas privilegiadas requieren monitoreo reforzado.
- La correlación entre creación de usuario, grupos y autenticaciones posteriores es clave.
- La documentación clara facilita escalamiento y respuesta.

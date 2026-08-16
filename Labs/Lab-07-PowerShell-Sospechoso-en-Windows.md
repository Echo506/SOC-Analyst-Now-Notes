# Lab 07: Investigación de PowerShell sospechoso en Windows

## Objetivo

Documentar la investigación inicial de una alerta relacionada con la ejecución sospechosa de PowerShell en un endpoint Windows.

## Escenario

El EDR genera una alerta porque detecta la ejecución de `powershell.exe` con parámetros inusuales, incluyendo una cadena codificada y una ventana oculta.

## Datos iniciales de la alerta

| Campo | Valor |
|---|---|
| Tipo de alerta | Ejecución sospechosa de PowerShell |
| Equipo afectado | `WS-HR-012` |
| Usuario asociado | `usuario.ejemplo` |
| Proceso detectado | `powershell.exe` |
| Proceso padre | `WINWORD.EXE` |
| Línea de comandos | `powershell.exe -ExecutionPolicy Bypass -WindowStyle Hidden -EncodedCommand ...` |
| Severidad inicial | Alta |
| Acción del EDR | Alerta generada, ejecución observada |

> Los nombres, rutas y comandos de este laboratorio son ficticios y se utilizan solo con fines educativos.

## Hipótesis inicial

La actividad puede indicar la ejecución de un script malicioso lanzado desde un documento de Office, posiblemente asociado a phishing, descarga de payloads o ejecución de comandos de reconocimiento.

## Indicadores observados

- Uso de `-EncodedCommand`
- Uso de `-ExecutionPolicy Bypass`
- Ventana oculta con `-WindowStyle Hidden`
- Proceso padre inusual: `WINWORD.EXE`
- Posible intento de evasión o ejecución no interactiva

## Proceso de investigación

### 1. Validar la alerta

Confirmar el equipo, usuario, fecha, hora, línea de comandos completa, hash del proceso relacionado y acción aplicada por el EDR.

### 2. Revisar el árbol de procesos

- Identificar el proceso padre y procesos hijos.
- Confirmar si `WINWORD.EXE`, `EXCEL.EXE` o `OUTLOOK.EXE` iniciaron PowerShell.
- Revisar si hubo ejecución posterior de `cmd.exe`, `mshta.exe`, `wscript.exe` o binarios descargados.

### 3. Analizar la línea de comandos

- Preservar la línea completa para análisis.
- Verificar si contiene URLs, rutas temporales, cadenas codificadas o comandos de descarga.
- Identificar parámetros asociados a bypass, ocultamiento o ejecución remota.

### 4. Obtener contexto adicional

- Revisar si el usuario abrió un documento reciente o un correo sospechoso.
- Buscar conexiones de red iniciadas por PowerShell.
- Revisar si se crearon archivos en `Downloads`, `Temp` o `AppData`.
- Validar persistencia mediante tareas programadas, claves de inicio o servicios.
- Buscar eventos similares en otros equipos.

## Clasificación

El caso se clasifica como **actividad sospechosa con posible ejecución maliciosa**. Debe escalarse si se confirma descarga de payload, persistencia, conexión a infraestructura maliciosa o ejecución derivada de phishing.

## Acciones recomendadas

- Aislar el endpoint si existe evidencia de actividad maliciosa.
- Preservar la línea de comandos y el árbol de procesos.
- Revisar el correo o documento que originó la ejecución.
- Bloquear indicadores relacionados, como URLs, hashes o dominios.
- Buscar el mismo patrón en otros equipos.
- Restablecer credenciales si existe posibilidad de robo de información.
- Documentar hallazgos y decisiones en el ticket.

## Mapeo MITRE ATT&CK

| Técnica | Identificador | Relación |
|---|---|---|
| PowerShell | T1059.001 | Uso de PowerShell para ejecutar comandos o scripts |
| User Execution | T1204 | Posible ejecución por interacción del usuario |
| Spearphishing Attachment | T1566.001 | Posible origen mediante documento adjunto |
| Command and Scripting Interpreter | T1059 | Uso de intérpretes para ejecutar acciones en el sistema |

## Conclusión

La combinación de `EncodedCommand`, `ExecutionPolicy Bypass`, ejecución oculta y origen desde `WINWORD.EXE` es altamente sospechosa. Aunque no confirma por sí sola un compromiso, sí justifica una investigación prioritaria del endpoint, del correo asociado y de cualquier actividad posterior.

## Lecciones aprendidas

- El árbol de procesos es esencial para entender el origen de una alerta.
- PowerShell legítimo y PowerShell malicioso pueden parecer similares sin contexto.
- La línea de comandos proporciona evidencia valiosa para clasificación y escalamiento.
- Correlacionar Office, PowerShell, red y archivos temporales mejora la investigación.

# LD-009 - Intento de Explotación de Utilman.exe mediante Winlogon para Escalada de Privilegios

## Descripción del Escenario

Esta investigación se centró en la detección de un intento de explotación de **Utilman.exe** para obtener acceso privilegiado al sistema mediante la sustitución del ejecutable legítimo por **cmd.exe**. El ataque permitió la ejecución de comandos desde la pantalla de inicio de sesión de Windows y la creación de una cuenta con privilegios administrativos.

El objetivo de la investigación fue validar la alerta, analizar la cadena de ejecución, identificar las acciones realizadas por el atacante, evaluar el nivel de compromiso del equipo y aplicar las medidas de contención correspondientes.

---

## Habilidades Demostradas

- Respuesta a Incidentes
- Análisis de Endpoints
- Análisis de Procesos
- Análisis de Persistencia
- Escalada de Privilegios
- Threat Hunting
- Threat Intelligence
- Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

| Campo | Valor |
|---------|---------|
| Event ID | 161 |
| Regla | SOC211 - Utilman.exe Winlogon Exploit Attempt |
| Equipo | Henry |
| Dirección IP | 172.16.17.149 |
| Archivo Detectado | utilman.exe (cmd.exe) |
| SHA1 | ded8fd7f36417f66eb6ada10e0c0d7c0022986e9 |
| Acción del Endpoint | Allowed |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La alerta fue generada tras detectar la ejecución de **Utilman.exe** como proceso hijo de **Winlogon.exe**, comportamiento comúnmente asociado con técnicas de escalada de privilegios y persistencia.

Hallazgos principales:

- Equipo afectado: Henry.
- Dirección IP: 172.16.17.149.
- El endpoint permitió la ejecución del proceso (**Allowed**).
- Se detectó la creación de una nueva cuenta de usuario desde un proceso iniciado por **Winlogon.exe**.

La evidencia permitió confirmar que la alerta correspondía a un **Verdadero Positivo**.

---

### 2. Análisis del Endpoint

El análisis de los procesos del equipo mostró múltiples ejecuciones de **utilman.exe** coincidentes con la hora de generación de la alerta.

Al inspeccionar el árbol de procesos se observó que:

- Proceso padre:

```text
C:\Windows\System32\winlogon.exe
```

- Proceso ejecutado:

```text
C:\Windows\System32\utilman.exe
```

Durante la ejecución se creó una nueva cuenta de usuario denominada:

```text
Superman
```

con la contraseña:

```text
onepunch123
```

Lo anterior confirmó la manipulación del proceso de autenticación de Windows para obtener acceso privilegiado.

---

### 3. Análisis de la Actividad del Sistema

El historial de comandos permitió reconstruir la secuencia utilizada por el atacante.

Las acciones observadas fueron:

- Acceso al directorio **System32**.
- Renombrado del archivo original **utilman.exe** a **utilman.old**.
- Copia de **cmd.exe** con el nombre **utilman.exe**.
- Reinicio del sistema.
- Ejecución de la consola de comandos desde la pantalla de inicio de sesión.
- Creación de un nuevo usuario local.
- Incorporación del usuario al grupo **Administrators**.

Esta técnica permite obtener una consola con privilegios **SYSTEM** antes de iniciar sesión en Windows.

---

### 4. Análisis de Persistencia

La modificación del ejecutable **Utilman.exe** constituye un mecanismo de persistencia ampliamente conocido.

El atacante reemplazó una utilidad legítima del sistema por **cmd.exe**, permitiendo abrir una consola privilegiada cada vez que se selecciona la opción **Ease of Access** en la pantalla de inicio de sesión.

Asimismo, la creación de una cuenta administrativa proporciona un mecanismo adicional de acceso persistente al sistema.

---

### 5. Evaluación del Compromiso

La investigación confirmó que el atacante logró:

- Modificar archivos críticos del sistema.
- Escalar privilegios.
- Crear una cuenta administrativa no autorizada.
- Establecer persistencia mediante la modificación de **Utilman.exe**.

Debido a estas evidencias, el equipo fue considerado comprometido.

---

## Indicadores de Compromiso (IOC)

| Tipo | Valor |
|---------|---------|
| Archivo | utilman.exe (cmd.exe) |
| SHA1 | ded8fd7f36417f66eb6ada10e0c0d7c0022986e9 |
| Archivo Modificado | C:\Windows\System32\utilman.old |
| Usuario Creado | Superman |
| Contraseña | onepunch123 |

---

## Evaluación del Ataque

### Hallazgos

- Se modificó un archivo crítico del sistema.
- Se reemplazó **Utilman.exe** por **cmd.exe**.
- Se obtuvo ejecución con privilegios **SYSTEM**.
- Se creó una cuenta administrativa no autorizada.
- El atacante estableció mecanismos de persistencia.

### Dirección del Ataque

```text
Usuario → Sistema Operativo → Escalada de Privilegios
```

### Estado del Ataque

```text
Exitoso
```

---

## Mapeo MITRE ATT&CK

| Táctica | Técnica |
|---------|---------|
| Persistencia | T1136 - Create Account |
| Persistencia | T1546 - Event Triggered Execution |
| Escalada de Privilegios | T1546 - Event Triggered Execution |
| Evasión de Defensas | T1036 - Masquerading |

---

## Acciones de Contención

- Aislar el equipo comprometido.
- Eliminar la cuenta de usuario no autorizada.
- Restaurar el archivo original **Utilman.exe**.
- Eliminar cualquier archivo modificado por el atacante.
- Revisar el resto de los equipos en busca de indicadores similares.
- Restringir los permisos de escritura sobre el directorio **System32**.
- Realizar un análisis completo del endpoint para detectar mecanismos adicionales de persistencia.

---

## Lecciones Aprendidas

- La supervisión del árbol de procesos permite detectar técnicas avanzadas de escalada de privilegios.
- La modificación de binarios legítimos de Windows continúa siendo una técnica utilizada para mantener persistencia.
- La creación inesperada de cuentas administrativas debe investigarse inmediatamente.
- El antivirus por sí solo no es suficiente para detectar todas las técnicas de post-explotación.
- La correlación entre procesos, historial de comandos y eventos del sistema permite reconstruir con precisión la actividad del atacante.

---

## Conclusión

La investigación confirmó un compromiso exitoso del equipo mediante la explotación de **Utilman.exe**, técnica utilizada para obtener privilegios elevados desde la pantalla de inicio de sesión de Windows.

El análisis del árbol de procesos, el historial de comandos y los eventos del endpoint permitió verificar la sustitución de **Utilman.exe** por **cmd.exe**, la creación de una cuenta administrativa denominada **Superman** y el establecimiento de mecanismos de persistencia. Como medidas inmediatas se recomendó aislar el equipo, eliminar la cuenta creada por el atacante, restaurar los archivos del sistema afectados y realizar una revisión exhaustiva del endpoint para garantizar la erradicación completa de la amenaza.

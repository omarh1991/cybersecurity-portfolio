# LD-006 - Intento de Acceso desde una Región Cloud No Autorizada

## Resumen Ejecutivo

Durante esta investigación se analizó una alerta de seguridad relacionada con múltiples intentos de acceso provenientes de una región geográfica no autorizada hacia servicios alojados en infraestructura AWS.

El objetivo fue validar la legitimidad de la actividad, identificar los indicadores de compromiso (IOC), determinar el impacto potencial y recomendar medidas de contención para prevenir accesos no autorizados.

---

## Habilidades Demostradas

* Respuesta a Incidentes (Incident Response)
* Análisis de Logs
* Threat Hunting
* Threat Intelligence
* Investigación de IOC
* Análisis de Accesos Remotos
* Monitoreo de Seguridad
* Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

Se detectaron múltiples solicitudes provenientes de una dirección IP externa ubicada en una región geográfica restringida para la organización. El tráfico estaba dirigido hacia servicios expuestos en infraestructura AWS.

Las políticas de seguridad implementadas en el firewall y el proxy bloquearon todas las conexiones, impidiendo que el atacante lograra acceso al entorno.

### Activo Afectado

| Campo | Valor |
|-------|-------|
| Plataforma | Amazon Web Services (AWS) |
| Host/IP | 52.15.206.21 |
| Usuario Objetivo | test@letsdefend.io |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La investigación comenzó revisando los registros de seguridad para identificar el origen de las conexiones, el sistema objetivo y la naturaleza del tráfico detectado.

Se confirmó que todas las solicitudes provenían de una región no autorizada según las políticas de acceso de la organización.

---

### 2. Análisis de Inteligencia de Amenazas

La dirección IP de origen fue consultada mediante plataformas de inteligencia de amenazas.

Los resultados mostraron que la dirección IP estaba previamente asociada con actividades maliciosas relacionadas con:

- Ataques Web
- Fuerza Bruta (Brute Force)
- Ataques SSH
- Actividad Maliciosa General

---

### 3. Análisis del Tráfico

El análisis de los registros permitió confirmar que todas las solicitudes fueron bloqueadas por los dispositivos perimetrales antes de alcanzar el servicio objetivo.

No se identificó evidencia de autenticación exitosa ni ejecución de acciones sobre el sistema.

---

### 4. Evaluación del Impacto

La investigación confirmó que:

- No existió acceso exitoso.
- No hubo compromiso del sistema.
- No se identificó movimiento lateral.
- No se observaron modificaciones sobre la infraestructura.

Las medidas de seguridad implementadas funcionaron correctamente evitando el acceso no autorizado.

---

## Indicadores de Compromiso (IOC)

| Tipo | Valor |
|------|-------|
| IPv4 | 134.209.145.73 |
| Host Objetivo | AWS_Services / 52.15.206.21 |
| Usuario Objetivo | test@letsdefend.io |
| Origen | Región Cloud No Autorizada |

---

## Evaluación del Ataque

La investigación permitió determinar:

- Actividad sospechosa proveniente de Internet.
- Intento de acceso desde una ubicación geográfica restringida.
- Solicitudes bloqueadas por controles perimetrales.
- Sin evidencia de compromiso.

### Dirección del Tráfico

```text
Internet → Infraestructura AWS
```

### Estado del Ataque

```text
Bloqueado
```

---

## Mapeo MITRE ATT&CK

| Táctica | Técnica |
|---------|----------|
| Initial Access | T1078 - Valid Accounts (Intento de uso de credenciales) |
| Reconnaissance | T1595 - Active Scanning |

---

## Acciones de Contención

Se recomendaron las siguientes medidas:

1. Mantener las restricciones geográficas configuradas en firewall y proxy.
2. Continuar bloqueando direcciones IP maliciosas mediante listas de reputación.
3. Habilitar MFA para todos los servicios expuestos.
4. Revisar periódicamente las listas blancas (Whitelist) de acceso remoto.
5. Aplicar actualizaciones de seguridad sobre los sistemas públicos.
6. Realizar escaneos periódicos de vulnerabilidades.

---

## Lecciones Aprendidas

- Las restricciones geográficas reducen significativamente la superficie de ataque.
- El uso de inteligencia de amenazas permite validar rápidamente la reputación de una dirección IP.
- El monitoreo continuo facilita la detección temprana de intentos de acceso no autorizados.
- La autenticación multifactor fortalece la protección frente a intentos de compromiso.

---

## Conclusión

La investigación confirmó múltiples intentos de acceso provenientes de una región cloud no autorizada hacia servicios alojados en AWS.

Gracias a las políticas de seguridad implementadas en los dispositivos perimetrales, todas las solicitudes fueron bloqueadas exitosamente, evitando cualquier acceso no autorizado o compromiso de la infraestructura.

La revisión de los registros, junto con el análisis de inteligencia de amenazas, permitió validar que la actividad correspondía a un intento malicioso sin impacto sobre los activos protegidos.

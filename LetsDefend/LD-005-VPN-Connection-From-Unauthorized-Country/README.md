# LD-005 - Conexión VPN Detectada desde un País No Autorizado

## Resumen Ejecutivo

Durante esta investigación se analizó una alerta relacionada con un intento de acceso VPN desde una ubicación geográfica no autorizada utilizando las credenciales de una usuaria legítima.

El objetivo fue validar la alerta, determinar si el acceso fue exitoso, identificar indicadores de compromiso (IOC) y evaluar el impacto potencial sobre la organización.

---

## Habilidades Demostradas

- Respuesta a Incidentes (Incident Response)
- Análisis de Logs
- Investigación de Accesos VPN
- Threat Intelligence
- Análisis de Reputación IP
- Detección de Cuentas Comprometidas
- Análisis de MFA
- Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

| Campo | Valor |
|---------|---------|
| Event ID | 225 |
| Regla | SOC257 - VPN Connection Detected from Unauthorized Country |
| Usuario | monica@letsdefend.io |
| IP Atacante | 113.161.158.12 |
| Ubicación | Vietnam |
| Servicio Objetivo | VPN Corporativa |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La alerta fue generada debido a un intento de conexión VPN desde una ubicación geográfica no habitual para la usuaria Monica.

Durante la revisión inicial se identificó:

- Usuario: monica@letsdefend.io
- IP origen: 113.161.158.12
- Ubicación: Vietnam
- Servicio objetivo: VPN corporativa

La actividad fue considerada sospechosa debido a la ubicación del origen y al uso de credenciales válidas. :contentReference[oaicite:1]{index=1}

---

### 2. Verificación de la Actividad VPN

Los registros Proxy mostraron una solicitud hacia:

```text
https://vpn-letsdefend.io
```

La conexión recibió:

```text
HTTP 200
```

Lo que confirmó que el atacante logró acceder al portal VPN. :contentReference[oaicite:2]{index=2}

---

### 3. Inteligencia de Amenazas

La dirección IP fue analizada utilizando distintas fuentes de reputación.

Resultados obtenidos:

- IP perteneciente a Vietnam Posts and Telecommunications Group.
- Reportes previos relacionados con:
  - Brute Force
  - Web Attacks
  - Actividad maliciosa

La reputación de la dirección IP fue considerada sospechosa. :contentReference[oaicite:3]{index=3}

---

### 4. Análisis de Acceso Inicial

La investigación confirmó que el atacante conocía la contraseña correcta de la usuaria Monica.

Sin embargo, la infraestructura VPN requería autenticación multifactor (MFA), lo que impidió completar el acceso.

Los registros mostraron:

```text
Incorrect OTP Code
```

Esto indicó que el atacante no pudo proporcionar el código OTP válido enviado a la víctima. :contentReference[oaicite:4]{index=4}

---

### 5. Verificación del MFA

La revisión de Email Security confirmó la recepción de múltiples correos con códigos OTP.

Se identificaron envíos a las:

- 02:01 AM
- 02:02 AM
- 02:03 AM

Lo que confirmó que el mecanismo MFA funcionó correctamente y evitó el acceso no autorizado. :contentReference[oaicite:5]{index=5}

---

### 6. Actividad Adicional del Atacante

La revisión de los registros reveló actividad adicional proveniente de la misma dirección IP.

Se observaron intentos de reconocimiento mediante escaneo de puertos contra:

```text
33.33.33.33
```

Esta actividad sugiere acciones de reconocimiento posteriores al intento de acceso VPN. :contentReference[oaicite:6]{index=6}

---

## Indicadores de Compromiso (IOC)

| Tipo | Valor |
|---------|---------|
| IPv4 | 113.161.158.12 |
| Usuario Objetivo | monica@letsdefend.io |
| Servicio | https://vpn-letsdefend.io |
| Acción Detectada | Incorrect OTP Code |
| Técnica | Acceso Remoto Externo |

---

## Evaluación del Ataque

### Hallazgos

- Se utilizaron credenciales válidas.
- El atacante conocía la contraseña de la víctima.
- La autenticación MFA impidió el acceso.
- Se identificó actividad de reconocimiento adicional.
- La cuenta debe considerarse comprometida.

### Dirección del Tráfico

```text
Internet → VPN Corporativa
```

### Estado del Ataque

```text
Parcialmente Exitoso
```

El atacante logró autenticarse con la contraseña correcta, pero no pudo completar el proceso MFA.

---

## Mapeo MITRE ATT&CK

| Táctica | Técnica |
|---------|---------|
| Initial Access | T1133 - External Remote Services |
| Credential Access | T1110 - Brute Force / Credential Abuse |
| Reconnaissance | T1046 - Network Service Discovery |

---

## Acciones de Contención

1. Restablecer inmediatamente la contraseña de la cuenta afectada.
2. Revisar posibles filtraciones de credenciales.
3. Mantener MFA obligatorio para accesos remotos.
4. Bloquear la dirección IP identificada.
5. Revisar accesos VPN recientes asociados al usuario.
6. Implementar restricciones geográficas de acceso cuando sea posible.

---

## Lecciones Aprendidas

- MFA continúa siendo una de las mejores defensas contra el robo de credenciales.
- Las conexiones desde ubicaciones inusuales deben investigarse inmediatamente.
- Las cuentas comprometidas deben tratarse como incidentes de seguridad incluso cuando el acceso no se complete.
- Las restricciones geográficas pueden reducir significativamente la superficie de ataque.

---

## Conclusión

La investigación confirmó un intento de acceso VPN utilizando credenciales válidas de una usuaria legítima desde una ubicación geográfica no autorizada.

Aunque el atacante logró autenticarse con la contraseña correcta, el mecanismo MFA impidió completar el acceso. La actividad observada permitió concluir que la cuenta debía considerarse comprometida, requiriendo el restablecimiento inmediato de credenciales y la revisión de accesos asociados.

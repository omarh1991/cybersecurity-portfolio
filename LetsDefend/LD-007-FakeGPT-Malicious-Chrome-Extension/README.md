# LD-007 - Extensión Maliciosa FakeGPT para Google Chrome

## Descripción del Escenario

Esta investigación se centró en la detección de una extensión maliciosa de Google Chrome que se hacía pasar por una extensión legítima de ChatGPT.

El objetivo fue validar la alerta, determinar si la extensión se instaló correctamente en el equipo, identificar posibles comunicaciones con infraestructura de Comando y Control (C2), evaluar el nivel de compromiso del sistema y aplicar las medidas de contención correspondientes.

---

## Habilidades Demostradas

- Respuesta a Incidentes
- Análisis de Malware
- Análisis de Extensiones de Navegador
- Threat Intelligence
- Análisis de Indicadores de Compromiso (IOC)
- Investigación de Endpoints
- Análisis de Tráfico de Red
- Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

| Campo | Valor |
|---------|---------|
| Event ID | 153 |
| Regla | SOC202 - FakeGPT Malicious Chrome Extension |
| Equipo | Samuel |
| Dirección IP | 172.16.17.173 |
| Extensión Detectada | hacfaophiklaeolhnmckojjjjbnappen.crx |
| SHA256 | 7421f9abe5e618a0d517861f4709df53292a5f137053a227bfb4eb8e152a4669 |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La alerta indicó la instalación de una extensión sospechosa en el navegador Google Chrome del equipo afectado.

Hallazgos principales:

- Equipo: Samuel
- Dirección IP: 172.16.17.173
- ID de la extensión:

```text
hacfaophiklaeolhnmckojjjjbnappen
```

- Acción del Endpoint:

```text
Allowed
```

La solución de seguridad permitió la ejecución de la extensión sin aplicar ninguna acción preventiva, por lo que fue necesario continuar con la investigación.

---

### 2. Análisis del Endpoint

Los registros del endpoint confirmaron que Google Chrome ejecutó la instalación de la extensión mediante el siguiente comando:

```text
"C:\Program Files\Google\Chrome\Application\chrome.exe"
--single-argument
C:\Users\LetsDefend\Desktop\hacfaophiklaeolhnmckojjjjbnappen.crx
```

Esto confirmó que la extensión fue instalada correctamente en el sistema comprometido.

---

### 3. Análisis de la Actividad del Navegador

El historial del navegador mostró diversas interacciones relacionadas con la extensión detectada.

Entre las actividades observadas se encontraron:

- Acceso a la página de la extensión en Chrome Web Store.
- Acceso al administrador de extensiones de Chrome.
- Apertura de la página de configuración de la extensión.
- Acceso al sitio legítimo de OpenAI Chat.
- Acceso a la página de inicio de sesión de OpenAI.

Estos eventos indican que el usuario interactuó con la extensión e intentó utilizarla junto con su cuenta de ChatGPT.

---

### 4. Análisis de Threat Intelligence

Se investigó el identificador de la extensión utilizando fuentes externas de inteligencia de amenazas.

Los resultados mostraron que:

- La extensión correspondía a **"ChatGPT for Google"**.
- Había sido eliminada previamente de Chrome Web Store.
- Diversas fuentes de Threat Intelligence la clasificaban como malware.
- Existían reportes que la relacionaban con el robo de credenciales y el compromiso de cuentas de usuarios.

La evidencia confirmó que se trataba de una extensión maliciosa de alto riesgo.

---

### 5. Verificación del Estado del Malware

Los registros del endpoint mostraron la siguiente acción:

```text
Device Action: Allowed
```

No se observó ningún proceso de cuarentena o limpieza posterior a la detección.

La actividad de procesos y las conexiones de red posteriores indicaron que la extensión permanecía activa en el sistema.

---

### 6. Identificación de Comunicación con C2

El análisis de los registros de red permitió identificar conexiones salientes hacia dominios maliciosos asociados con la extensión.

Dominios detectados:

```text
www.chatgptforgoogle.pro
www.chatgptgoogle.org
version.chatgpt4google.workers.dev
```

La verificación mediante plataformas de Threat Intelligence confirmó que estos dominios formaban parte de la infraestructura de Comando y Control (C2) utilizada por la amenaza.

Esto confirmó que el equipo comprometido estableció comunicaciones no autorizadas con infraestructura maliciosa.

---

## Indicadores de Compromiso (IOC)

| Tipo | Valor |
|---------|---------|
| Extensión | hacfaophiklaeolhnmckojjjjbnappen.crx |
| SHA256 | 7421f9abe5e618a0d517861f4709df53292a5f137053a227bfb4eb8e152a4669 |
| Dominio | www.chatgptforgoogle.pro |
| Dominio | www.chatgptgoogle.org |
| Dominio | version.chatgpt4google.workers.dev |
| IP | 172.217.17.142 |
| IP | 18.140.6.45 |
| IP | 52.76.101.124 |

---

## Evaluación del Ataque

### Hallazgos

- La extensión maliciosa fue instalada correctamente.
- La solución de seguridad permitió su ejecución.
- El usuario interactuó con la extensión.
- Se detectaron comunicaciones hacia infraestructura C2.
- Existe una alta probabilidad de compromiso de credenciales y del navegador.

### Dirección del Tráfico

```text
Endpoint → Infraestructura C2
```

### Estado del Ataque

```text
Exitoso
```

---

## Mapeo MITRE ATT&CK

| Táctica | Técnica |
|---------|---------|
| Acceso Inicial | T1189 - Drive-by Compromise |
| Ejecución | T1204 - User Execution |
| Persistencia | T1176 - Browser Extensions |

---

## Acciones de Contención

- Aislar el equipo afectado de la red.
- Eliminar la extensión maliciosa del navegador.
- Bloquear los dominios e IP identificados como maliciosos.
- Restablecer las credenciales potencialmente comprometidas.
- Habilitar autenticación multifactor (MFA).
- Ejecutar un análisis completo del endpoint para identificar posibles mecanismos de persistencia.

---

## Lecciones Aprendidas

- Las extensiones del navegador deben instalarse únicamente desde desarrolladores confiables.
- La inteligencia de amenazas permite validar rápidamente artefactos sospechosos.
- Las extensiones maliciosas pueden convertirse en mecanismos de persistencia para los atacantes.
- El monitoreo de conexiones salientes facilita la detección temprana de comunicaciones con servidores C2.
- La concienciación de los usuarios continúa siendo una de las principales medidas de prevención.

---

## Conclusión

La investigación confirmó la instalación exitosa de una extensión maliciosa que suplantaba una integración legítima de ChatGPT para Google Chrome.

El análisis de los registros del endpoint, el historial del navegador, las conexiones de red y las fuentes de Threat Intelligence permitió confirmar que la extensión estableció comunicaciones con infraestructura de Comando y Control (C2), representando un alto riesgo para la confidencialidad de la información del usuario. Como medidas inmediatas se recomendó aislar el equipo, eliminar la extensión, bloquear los indicadores de compromiso y restablecer las credenciales potencialmente afectadas.

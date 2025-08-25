---
layout: post
title: "Introducción práctica al Threat Hunting: primeros pasos"
date: 2025-08-24
tags: [ciberseguridad, threat-hunting, blue-team, mitre-attack, soc]
description: "Una guía clara y accionable para empezar con Threat Hunting: hipótesis, datos, anomalías, herramientas y documentación."
image: /assets/img/threat-hunting/cover.jpg
---

![Threat Hunting en acción: analista revisando telemetría de endpoints y red](/assets/img/threat-hunting/cover.JPG "Threat Hunting en acción")

La ciberseguridad ya no se trata solo de **levantar muros y esperar a que resistan**. Las organizaciones más maduras saben que tarde o temprano algún atacante conseguirá atravesar las defensas. Ahí es donde entra en juego el **Threat Hunting**, o “caza de amenazas”: una disciplina proactiva cuyo objetivo es **buscar indicios de actividad maliciosa dentro de los sistemas antes de que causen un incidente mayor**.

---

## ¿Qué es exactamente el Threat Hunting?

El Threat Hunting es una práctica de investigación que combina **inteligencia de amenazas, hipótesis basadas en comportamiento adversario y análisis de datos** para descubrir ataques que pasan inadvertidos por las soluciones de seguridad tradicionales (AV, IDS, SIEM).

> En pocas palabras: es salir a **cazar** en lugar de esperar a que salten las alarmas.


---

## ¿Por qué es importante?

- **Los atacantes se vuelven más sigilosos.** Malware sin archivos, movimientos laterales y técnicas de evasión.
- **Reduce el tiempo de permanencia.** Acorta semanas o meses de dwell time.
- **Fortalece las defensas.** Cada hallazgo alimenta reglas, alertas y procedimientos de respuesta.

---

## Primeros pasos para empezar con Threat Hunting

No necesitas un SOC enorme ni un arsenal de herramientas sofisticadas para comenzar. Lo esencial es **un enfoque metódico y disciplina analítica**.

### 1) Define una hipótesis

Ejemplos que puedes convertir en “misiones” de hunting:

- *¿Qué pasaría si un atacante ya tuviera credenciales de administrador?*
- *¿Vemos intentos de phishing dirigidos que evadan el gateway?*
- *¿Hay uso anómalo de herramientas nativas (LOLBins) en servidores críticos?*

> **Consejo:** formula la hipótesis con una táctica/técnica de MITRE ATT&CK y el(los) dataset(s) necesarios para validarla.

### 2) Reúne tus fuentes de datos

Los **logs** son tu materia prima:

- **Autenticación:** Windows Event Logs, Syslog, AzureAD/Okta.
- **Red:** NetFlow/Zeek, DNS, proxy/firewall.
- **Endpoints:** EDR/XDR, creación de procesos, módulos cargados.
- **Infra:** AD, GPO, registros de nube (CloudTrail, Activity Logs).

### 3) Establece una línea base

Antes de cazar anomalías, entiende qué es “normal”:

- ¿Quién se conecta fuera de horario y desde dónde?
- ¿Qué procesos generan más tráfico saliente?
- ¿Qué admins usan PowerShell a diario?

### 4) Busca comportamientos anómalos

Patrones para un primer ciclo de hunting:

- **Logons desde geos inusuales** o ASN desconocidos.
- **Uso de PowerShell/PsExec** en horarios extraños o no habituales para el host.
- **Accesos fallidos repetidos** a sistemas críticos.
- **DNS hacia dominios recién registrados** o con puntaje de riesgo alto.
- **Persistencia** sospechosa (tareas programadas, servicios nuevos).

### 5) Documenta y comparte hallazgos

Registra **hipótesis, datos, consultas, evidencia y resultados**:

- Crea *playbooks* reutilizables.
- Convierte resultados en **detecciones** (Sigma/EDR/UEBA).
- Alimenta **IR** (procedimientos de contención y erradicación).

---

## Ejemplo rápido: hipótesis → consulta → resultado

**Hipótesis:** “Posible exfiltración a través de DNS (T1048.003).”  
**Datos:** logs de DNS + EDR (procesos que generan consultas).  
**Búsqueda inicial:** dominios con alto volumen de subdominios únicos por host en 24h.  
**Evidencia:** host `SRV-FILE-02` realiza >5 000 consultas TXT a `*.exfil-temp[.]com`.  
**Acción:** contener host, bloquear dominio, volcar memoria/procesos, revisar credenciales.


---

## Herramientas recomendadas para principiantes

- **ELK Stack (Elasticsearch, Logstash, Kibana):** ingesta y visualización de logs.
- **Sigma Rules:** detecciones portables que puedes adaptar a tu SIEM.
- **Velociraptor o Wazuh:** telemetría y respuesta en endpoints (open source).
- **MITRE ATT&CK:** taxonomía de tácticas y técnicas para mapear hipótesis y hallazgos.

> **Tip:** cada vez que cierres un hunting, crea al menos **una** detección nueva o mejora una existente.

---

## Checklist para tu primer hunting

- [ ] Hipótesis definida y mapeada a ATT&CK  
- [ ] Datasets disponibles (autenticación, DNS, EDR, red)  
- [ ] Línea base (qué es normal) establecida  
- [ ] Consultas guardadas con parámetros (host, usuario, tiempo)  
- [ ] Evidencias y decisiones documentadas  
- [ ] Reglas/alertas actualizadas y validadas  
- [ ] Lecciones aprendidas compartidas con SecOps/IR

---

## Conclusión

El Threat Hunting no es una actividad de una sola vez, sino un **proceso continuo de aprendizaje y mejora**. Empieza con hipótesis simples, mide tus resultados y documenta todo. Al final, lo importante no es cazar mucho, sino **cazar mejor cada vez**.

---

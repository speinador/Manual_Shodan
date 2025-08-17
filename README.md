# 📖 Manual de Shodan para Ethical Hacking y Bug Bounty

---

## 📑 Tabla de contenidos

- 📘 [Introducción y ética](#-introducción-y-ética)
- 🔍 [Qué es Shodan](#-qué-es-shodan)
- 🛠️ [Casos de uso (pentesting & bug bounty)](#-casos-de-uso-pentesting--bug-bounty)
- 👤 [Cuenta, planes y límites](#-cuenta-planes-y-límites)
- 🌐 [Interfaz web: recorrido rápido](#-interfaz-web-recorrido-rápido)
- ⚡ [Búsqueda básica y operadores](#-búsqueda-básica-y-operadores)
- 🧰 [Dorks prácticos por categorías](#-dorks-prácticos-por-categorías)
- 🔄 [Workflows paso a paso](#-workflows-paso-a-paso)
- 🐍 [Uso de la API (Python y CLI)](#-uso-de-la-api-python-y-cli)
- 🤖 [Automatización y enriquecimiento](#-automatización-y-enriquecimiento)
- 📝 [Validación segura y reporte de hallazgos](#-validación-segura-y-reporte-de-hallazgos)
- 🕵️ [OPSEC: seguridad operacional](#-opsec-seguridad-operacional)
- ⚠️ [Limitaciones y trampas comunes](#-limitaciones-y-trampas-comunes)
- ✅ [Checklist de revisión rápida](#-checklist-de-revisión-rápida)
- 📂 [Plantillas para GitHub](#-plantillas-para-github)
- 📎 [Anexos: más dorks, hashes de favicon y referencias](#-anexos-más-dorks-hashes-de-favicon-y-referencias)

---

## 📘 Introducción y ética
Shodan es un motor de búsqueda de dispositivos y servicios expuestos a Internet. Es valiosísimo para reconocimiento (recon) en pentesting y bug bounty, pero su uso debe ser **ético y legal**.

**Reglas de oro:**
- ✅ Trabaja solo con permiso: laboratorio propio, clientes con contrato o programas de bug bounty con scope explícito.  
- 🚫 No interactúes con sistemas de terceros fuera del scope.  
- 📢 Practica Responsible Disclosure.  
- 🗂️ Mantén registro (evidencias, fechas, consultas) y protege datos sensibles.  

---

## 🔍 Qué es Shodan
- 🔎 Indexa banners y metadatos de servicios (HTTP, SSH, RDP, MQTT, etc.) expuestos.  
- 📊 Permite filtrar por IP/host, puerto, producto/versión, geografía, ASN/ORG, fecha, SSL, favicon hash, etc.  
- 🖥️ Ofrece panel web, API, y herramientas complementarias.  

**¿Para qué sirve en seguridad?**
- 🕵️ Descubrir activos olvidados (shadow IT).  
- ⚠️ Detectar servicios mal configurados o versiones vulnerables.  
- 🎯 Priorizar superficies de ataque antes de un test más profundo.  

---

## 🛠️ Casos de uso (pentesting & bug bounty)
- 🌍 **Asset discovery:** enumerar subdominios y hosts relacionados a una organización.  
- 🔓 **Configuraciones débiles:** bases de datos sin auth, paneles de admin expuestos, listados de directorios.  
- 🐞 **Vulnerabilidades por versión:** encontrar tecnologías específicas con versiones afectadas por CVE conocidas.  
- 🔐 **Higiene de certificados:** SSL expirado, CN/SAN incorrectos.  

💡 Muchos programas de bug bounty aceptan como hallazgo válido la exposición de un servicio sensible (ej: Kibana/Grafana sin auth) si está dentro del scope.  

---

## 👤 Cuenta, planes y límites
- 🔑 Crea cuenta con MFA.  
- 💳 Considera plan de pago si necesitas más resultados, descargas, API y histórico.  
- ⏱️ Respeta rate limits: evita automatizaciones agresivas.  

---

## 🌐 Interfaz web: recorrido rápido
- 🔎 **Barra de búsqueda:** queries y dorks.  
- 🗂️ **Filtros:** país, ciudad, organización, puerto, tiempo.  
- 📋 **Resultados:** banner, puertos, headers, certificado, tags.  
- 🗺️ **Mapas:** heatmap geográfico.  
- 📈 **Explorar:** servicios populares, categorías, informes.  

💡 Usa **Saved Searches** para consultas frecuentes.  

---

## ⚡ Búsqueda básica y operadores
La sintaxis clave de Shodan es `clave:valor`. Se pueden combinar operadores lógicos (AND, OR) y términos libres.

**Filtros comunes:**
- `ip:1.2.3.4` → IP específica.  
- `net:1.2.3.0/24` → Rango CIDR.  
- `hostname:"sub.ejemplo.com"` → Hostname exacto.  
- `org:"ACME Corp"` → Organización/ISP.  
- `asn:AS13335` → Nro. de ASN.  
- `port:443` → Puerto.  
- `country:AR` / `city:"Buenos Aires"` → Geografía.  
- `before:01/01/2024` / `after:01/01/2024` → Fecha indexación.  
- `product:Apache` / `version:2.4.49` → Tecnología y versión.  
- `os:"Windows Server"` → Sistema operativo.  
- `ssl:"acme.com"` / `ssl.cert.subject.CN:"*.acme.com"` → Certificados.  
- `http.title:"index of /"` → Título de página.  
- `http.favicon.hash:2055322029` → Huella por favicon.  

💡 Tip: empieza amplio (org/asn) y ve refinando (puertos, títulos, versiones).  

---

## 🧰 Dorks prácticos por categorías
Incluye ejemplos de dorks organizados por **Web y paneles**, **Autenticación remota**, **Bases de datos**, **Certificados y dominios**, **Favicon hashes**, **IoT**, **ICS/SCADA**, **Por organización/ASN**, **Versiones vulnerables**.

Cada dork está acompañado de:
- 📌 Qué detecta.  
- ⚠️ Qué riesgo implica.  
- 🔒 Recomendación de validación segura.  

---

## 🔄 Workflows paso a paso
Guías detalladas para:
1. 🕵️ Recon en bug bounty.  
2. 🔐 Pentesting interno/externo.  
3. 📜 Higiene de certificados.  
4. 🕸️ Shadow IT.  

---

## 🐍 Uso de la API (Python y CLI)
Incluye ejemplos listos de:
- 📦 Instalación (`pip install shodan`).  
- 🐍 Script Python para búsqueda y exportación CSV.  
- 💻 Uso de la CLI con ejemplos (`shodan search`, `shodan count`).  

---

## 🤖 Automatización y enriquecimiento
Flujo típico:
```
Shodan → Export → Limpieza → Validación (nmap/httpx) → Priorización → Reporte
```

Herramientas sugeridas:
- 🔎 Nmap/Naabu/HTTPx → Validación de puertos.  
- 🌐 Wappalyzer/WhatWeb → Fingerprinting web.  
- 📑 Nuclei → Templates no intrusivos.  
- 🗂️ CVE/CPE mapping → Cruce con vulnerabilidades.  

---

## 📝 Validación segura y reporte de hallazgos
**Validación (no intrusiva):**
- Revisar banners, headers, mensajes de error.  
- Confirmar exposición sin fuerza bruta ni intrusión.  

**Plantilla de reporte:**
```
# [Título del hallazgo]

**Severidad:** Alta/Media/Baja  
**Activo:** host/subdominio/IP  
**Descripción:** Qué está expuesto y por qué es un riesgo.  
**Evidencia:** Capturas, banners, fecha observación.  
**Impacto:** Qué podría lograr un atacante.  
**Reproducción:** Pasos para verificar (no intrusivos).  
**Remediación:** Acciones sugeridas.  
**Referencias:** CVE/CWE/Docs.  
```

---

## 🕵️ OPSEC: seguridad operacional
- 🔐 Usa cuentas separadas y MFA.  
- 🌍 Evita exponer tu IP real (VPN/lab).  
- 📂 Guarda evidencias en repos privados hasta disclosure.  

---

## ⚠️ Limitaciones y trampas comunes
- ❌ Falsos positivos/negativos: indexación no es en tiempo real.  
- ❌ Asumir vulnerabilidad por banner: puede haber parches/backports.  
- ❌ Confundir ISP con organización objetivo en bug bounty.  

---

## ✅ Checklist de revisión rápida
- 🔎 Scope verificado.  
- 📜 Búsqueda por certificados/org/asn.  
- ⚡ Filtros aplicados (puertos/producto).  
- 🛠️ Validación con nmap/httpx/wappalyzer.  
- 🎯 Priorización de hallazgos.  
- 📝 Reporte con evidencia y remediación.  

---

## 📂 Plantillas para GitHub
Ejemplo de **README.md**:
```markdown
# Shodan Playbook (Pentest & Bug Bounty)

Colección de consultas, workflows y ejemplos de API para usar Shodan de forma ética y efectiva.

## Contenido
- Manual (este documento)
- Scripts de API (Python/CLI)
- Dorks por categorías
- Plantillas de reporte

## Ética
Usar únicamente en entornos con autorización explícita.
```

Ejemplo de **.gitignore**:
```
# Claves y entornos
*.key
*.pem
.env
*.csv
venv/
__pycache__/
*.log
```

---

## 📎 Anexos: más dorks, hashes de favicon y referencias
Incluye:
- 🔎 Más dorks listos para consulta rápida.  
- 🖼️ Huellas de favicon adicionales (Jira, Nexus, Harbor, etc.).  
- 📜 Snippet Python para calcular favicon hash.  
- 📚 Referencias: Documentación oficial, HackerOne, Bugcrowd, CWE/CVE.  

________________________________________
## 🧑‍🏫 Autor

Explicación elaborada por [Sebastian Peinador](https://www.linkedin.com/in/sebastian-j-peinador/) para propósitos didácticos y de investigación en ciberseguridad ofensiva.

________________________________________

## 📄 Licencia

Este material se distribuye bajo la licencia [MIT](LICENSE).

________________________________________

> Si te resulta útil, ¡no olvides darle ⭐ al repo o compartirlo!


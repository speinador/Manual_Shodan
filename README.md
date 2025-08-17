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
- 🌍 WEB de Shodan [https://www.shodan.io/](https://www.shodan.io/)
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
- 🔑 Crea cuenta con MFA (Autenticación Multifactor).  
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

## 🔎 1. Filtros básicos

ip:1.2.3.4 → Buscar por IP específica.

hostname:"ejemplo.com" → Buscar por nombre de host.

org:"Nombre Empresa" → Buscar por organización/ISP.

asn:AS12345 → Buscar activos de un ASN.

country:"AR" → Filtrar por país (ej: Argentina).

city:"Buenos Aires" → Filtrar por ciudad.

before:01/01/2023 → Resultados indexados antes de una fecha.

after:01/01/2023 → Resultados después de una fecha.

## ⚙️ 2. Servicios comunes expuestos
### 🖥️ Web

http.title:"index of /" → Directorios listados.

http.title:"phpMyAdmin" → phpMyAdmin expuesto.

http.title:"Dashboard" → Paneles genéricos.

http.html:"Welcome to nginx" → Servidores nginx por defecto.

http.html:"It works!" → Apache por defecto.

### 🔑 Autenticación remota

port:22 → SSH.

port:21 Anonymous user logged in → FTP sin auth.

port:3389 → RDP expuesto.

port:5900 → VNC expuesto.

port:23 → Telnet.

### 🛢️ Bases de datos

product:MongoDB port:27017 → Mongo sin auth.

product:ElasticSearch port:9200 → Elastic expuesto.

product:Redis port:6379 → Redis abierto.

product:MySQL port:3306 → MySQL accesible.

product:PostgreSQL port:5432 → PostgreSQL.

### 📡 APIs y servicios

port:5601 title:"Kibana" → Kibana expuesto.

title:"Grafana" → Panel de Grafana.

title:"Docker API" → Docker sin seguridad.

title:"Kubernetes Dashboard" → K8s dashboard abierto.

## 🕵️ 3. Identificación de tecnología

product:Apache

product:Nginx

product:Microsoft-IIS

product:OpenSSH

os:"Windows 7"

os:"Linux 3.x"

## ⚡ 4. Versiones vulnerables

product:OpenSSL version:1.0.2 → Heartbleed.

product:Apache httpd version:2.4.49 → Path traversal (CVE-2021-41773).

product:Exim version:4.87 → Exim RCE.

product:ProFTPD version:1.3.5 → ProFTPD vulnerable.

(Útil para cruzar con CVEs en bug bounty).

## 🔐 5. Certificados y dominios

ssl.cert.subject.CN:"*.ejemplo.com" → Subdominios en certificados.

ssl.cert.issuer.CN:"Let's Encrypt" → Certificados de Let's Encrypt.

ssl:"example.com" → Buscar por string en certificados.

ssl.cert.expired:true → Certificados vencidos.

## 🖼️ 6. Huellas por favicon

(Favicons tienen hashes únicos que identifican productos).

http.favicon.hash:-247388890 → Jenkins.

http.favicon.hash:81586312 → GitLab.

http.favicon.hash:1601194732 → Kibana.

http.favicon.hash:2055322029 → Grafana.

(Muy usado en Bug Bounty para encontrar servicios ocultos).

## 🛰️ 7. IoT y dispositivos

title:"webcamXP" → Cámaras abiertas.

port:554 has_screenshot:true → Cámaras RTSP.

title:"Printer" → Impresoras expuestas.

port:10000 title:"Webmin" → Webmin expuesto.

title:"RouterOS login" → Mikrotik routers.

## 📊 8. SCADA/ICS (mucho cuidado ⚠️)

port:502 product:Modbus

port:44818 product:"EtherNet/IP"

product:Siemens

product:Schneider

(⚠️ Solo para investigación controlada, no tocar en Bug Bounty salvo que sea parte del scope autorizado).

## 🧰 9. Combos útiles para Bug Bounty

hostname:"*.target.com" → Subdominios de un objetivo.

ssl.cert.subject.CN:"target.com" → Servicios con SSL del target.

org:"Target Corp" → Infraestructura de la organización.

product:nginx hostname:"target.com" → Buscar tecnología específica en el target.

📌 Consejo final:
Lo mejor es usar estos dorks como primera capa de reconocimiento, luego cruzar la data con Nmap, WhatWeb, Wappalyzer, nuclei o exploit-db. En Bug Bounty, muchas veces basta con documentar un panel sensible expuesto para que el hallazgo sea válido 💰.

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

________________________________________
## 🧑‍🏫 Autor

Explicación elaborada por [Sebastian Peinador](https://www.linkedin.com/in/sebastian-j-peinador/) para propósitos didácticos y de investigación en ciberseguridad ofensiva.

________________________________________

## 📄 Licencia

Este material se distribuye bajo la licencia [MIT](LICENSE).

________________________________________

> Si te resulta útil, ¡no olvides darle ⭐ al repo o compartirlo!


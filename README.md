# 🍯 T-Pot Honeypot Lab — Inteligencia de Amenazas y Análisis de Ataques

![T-Pot](https://img.shields.io/badge/T--Pot_CE-Honeypot-ff2d78?style=for-the-badge&logo=shield&logoColor=white)
![Platform](https://img.shields.io/badge/Plataforma-Linux%20%7C%20Docker-0a0a0a?style=for-the-badge&logo=docker&logoColor=white)
![Cloud](https://img.shields.io/badge/Cloud-DigitalOcean-0080FF?style=for-the-badge&logo=digitalocean&logoColor=white)
![Duration](https://img.shields.io/badge/Duración-80%20horas%20%7C%205%20días-brightgreen?style=for-the-badge)
![Status](https://img.shields.io/badge/Estado-Completado-lightgrey?style=for-the-badge)

> **Proyecto personal de ciberseguridad** — Despliegue completo y operación activa de un honeypot multi-servicio T-Pot CE sobre un Droplet de DigitalOcean, capturando, analizando y respondiendo a ataques reales del Internet público durante **80 horas continuas** (8 al 12 de marzo de 2026).

**Autores:** Edyn Dominguez · Enrique Jou

---

## 📌 Descripción General

Este repositorio documenta el ciclo completo de un laboratorio de inteligencia de amenazas (*Threat Intelligence*) real: desde el aprovisionamiento del servidor cloud hasta la aplicación de reglas de firewall activas en respuesta a ataques en curso. T-Pot Community Edition integra más de 20 honeypot daemons coordinados mediante Docker, complementados con Suricata como IDS de red, Elasticsearch + Kibana como SIEM y un Attack Map en tiempo real que geolocaliza cada conexión entrante sobre un mapa mundial.

Durante las **80 horas de operación**, el contador de 24 horas del Attack Map superó repetidamente los **350,000 eventos de ataque**, con un pico de **26,364 ataques en una sola hora** — todo generado de forma completamente automática por actores maliciosos que detectaron la IP pública del servidor mediante escaneos masivos del espacio de direcciones IPv4.

---

## 🖥️ Infraestructura del Proyecto

El laboratorio fue desplegado sobre un **Droplet de DigitalOcean**, aprovechando su modelo de facturación por hora para controlar el costo con precisión y destruir el servidor al finalizar sin costos residuales.

| Parámetro | Detalle |
|---|---|
| **Proveedor cloud** | DigitalOcean |
| **Tipo de Droplet** | `s-2vcpu-8gb-160gb-intel` |
| **CPU / RAM / Disco** | 2 vCPU · 8 GB RAM · 160 GB SSD Intel |
| **Sistema operativo** | Ubuntu 24 LTS (64-bit) |
| **Hostname** | `Tpot-Honeypot` |
| **Usuario del sistema** | `honeypot` |
| **Inicio del Droplet** | 08 de marzo de 2026 — 15:30 hrs |
| **Fin del Droplet** | 12 de marzo de 2026 — 00:00 hrs |
| **Horas facturadas** | 80 horas |
| **Costo total del proyecto** | **$5.75 USD** |

El plan `s-2vcpu-8gb` fue seleccionado porque Elasticsearch —el backend de almacenamiento de T-Pot— consume entre 1 y 2 GB de RAM por sí solo, y con más de 15 contenedores Docker activos simultáneamente, un Droplet de menor capacidad hubiera colapsado por falta de memoria. Los 8 GB de RAM proporcionaron el margen necesario para operar sin degradación de calidad de captura durante los cinco días completos.

---

## 🎯 Objetivos del Proyecto

El propósito fue construir experiencia práctica real que la teoría no puede reemplazar: observar cómo se comportan los atacantes al encontrar un objetivo expuesto, y practicar respuesta activa con las mismas herramientas que usa un analista SOC.

Los objetivos específicos fueron configurar y validar el despliegue completo de T-Pot CE con múltiples honeypots activos simultáneamente; observar en tiempo real campañas de ataque contra SSH, SIP, Elasticsearch y HTTPS mediante el Live Feed; capturar y analizar tráfico a nivel de paquetes con `tcpdump`; aplicar reglas de bloqueo activo con `iptables` y `ufw`; y correlacionar eventos de Suricata con CVEs públicos para evaluar el nivel de sofisticación de los actores observados.

---

## 🛠️ Tecnologías y Herramientas

| Componente | Función en el laboratorio |
|---|---|
| **T-Pot CE** | Plataforma central — orquesta todos los honeypots via Docker |
| **Cowrie** | Honeypot SSH/Telnet — captura brute force y sesiones de shell simuladas |
| **Sentrypeer** | Honeypot SIP/VoIP — captura intentos de fraude de telefonía |
| **ElasticPot** | Honeypot Elasticsearch — captura escaneos de bases de datos NoSQL |
| **H0neytr4p** | Honeypot HTTP/HTTPS — captura explotación de CVEs web |
| **Honeytrap** | Honeypot TCP/UDP genérico — captura puertos y protocolos varios |
| **Suricata** | IDS de red — correlaciona tráfico con firmas CVE y anomalías TCP |
| **tcpdump** | Captura de paquetes en eth0 para análisis forense manual |
| **iptables (DOCKER-USER)** | Bloqueo activo antes de que el tráfico llegue al contenedor Docker |
| **ufw** | Firewall de host — complementa iptables a nivel de sistema operativo |
| **Elasticsearch + Kibana** | Indexación y visualización de todos los eventos capturados |
| **T-Pot Attack Map** | Visualización geográfica en tiempo real de ataques entrantes |

---

## 📊 Estadísticas de Ataque (Ventana de 24 horas)

| Métrica | Valor |
|---|---|
| **Total de ataques (24h)** | ~361,576 |
| **Pico de ataques (1 hora)** | ~26,364 |
| **ASN atacante número uno** | OVH SAS — 244,644 eventos |
| **IP atacante número uno** | `51.38.144.6` — 241,724 eventos |
| **Protocolo más atacado** | **SIP (puerto 5060)** — ~67% del volumen total |
| **CVE más activo detectado** | **CVE-2024-38816** — 69 detecciones por Suricata |
| **Honeypots más activos** | Sentrypeer · Cowrie · ElasticPot · H0neytr4p |
| **Países de origen principales** | EE.UU. · Polonia/OVH · Países Bajos · Singapur · Alemania |

---

## 🌍 Attack Map y Live Feed

El Attack Map de T-Pot mostró en tiempo real la geolocalización de cada atacante, con puntos de color diferente según el tipo de servicio objetivo. Los tipos de servicio activos durante los cinco días incluyeron:

`SSH` · `SIP` · `HTTPS` · `Elasticsearch` · `FTP` · `FTP-DATA` · `DNS` · `DHCP` · `DICOM` · `RDP` · `MySQL` · `MongoDB` · `MQTT` · `NFS` · `NTP` · `Oracle` · `POP3` · `POP3S` · `PostgreSQL` · `PPTP` · `ADB` · `CHARGEN`

La presencia de **DICOM** —el protocolo estándar de imágenes médicas como radiografías y tomografías— entre los servicios escaneados es uno de los hallazgos más preocupantes del proyecto, ya que indica que actores maliciosos buscan activamente sistemas de salud mal configurados expuestos al Internet público.

> ![Attack Map - Live Feed](screenshots/attack_map_livefeed.png)
> ![Attack Map - Pico SIP](screenshots/attack_map_sip.png)

---

## 🔍 Campañas de Ataque Documentadas

### 1. 🔴 Brute Force SSH — Honeypot Cowrie

La IP `198.143.191.202` (Estados Unidos, *Reputation Unknown*) mantuvo una cadencia exactamente regular de **un intento de autenticación por segundo** contra el puerto 22 durante períodos sostenidos de más de 10 minutos continuos, observados en múltiples días del proyecto. Esta regularidad milimétrica —sin ninguna variación de timing— es la firma característica de herramientas como **Hydra** o **Medusa** configuradas con un hilo por objetivo y un delay de 1000 ms, diseñadas precisamente para evadir sistemas de detección basados en umbrales de tasa.

Cowrie respondió simulando un servidor SSH real, aceptando conexiones y registrando todas las combinaciones de usuario/contraseña intentadas. En algunos intentos el honeypot incluso permitió acceso simulado para capturar los comandos que el atacante hubiera ejecutado post-compromiso.

```
IP origen:    198.143.191.202  (EE.UU.)
Protocolo:    SSH  →  Puerto 22
Honeypot:     Cowrie
Cadencia:     ~1 intento/segundo (sostenido por minutos)
Reputación:   Reputation Unknown (escáner automatizado)
Período:      Observado en múltiples días — 08-03 al 12-03-2026
```

> ![SSH Brute Force](screenshots/ssh_bruteforce_cowrie.png)

---

### 2. 📞 Fraude SIP/VoIP — Honeypot Sentrypeer

La campaña de **mayor volumen absoluto** del proyecto fue ejecutada por `51.38.144.6`, asociada al ASN de **OVH SAS (AS16276)** y enrutada geográficamente a través de Polonia. Esta sola IP generó **241,724 eventos**, convirtiendo a OVH SAS en el ASN atacante número uno con 244,644 eventos totales.

El ataque consistió en un flood masivo de mensajes `SIP REGISTER` al puerto **5060**, el puerto estándar del protocolo SIP (Session Initiation Protocol). El objetivo es registrar extensiones SIP no autorizadas para realizar llamadas internacionales fraudulentas a cargo del propietario del sistema —un delito conocido como **VoIP Toll Fraud** que genera pérdidas de cientos de millones de dólares anuales globalmente.

La captura con `tcpdump` reveló un detalle técnico importante: el atacante enviaba paquetes desde **docenas de puertos efímeros distintos** del bloque `ip6.ip-51-38-144.eu.*`, todos del mismo ASN de OVH. Esta técnica de múltiples sockets simultáneos maximiza la tasa de intentos y puede evadir reglas de firewall basadas únicamente en el puerto de origen.

```
IP origen:    51.38.144.6  (OVH SAS / enrutamiento Polonia)
Protocolo:    SIP  →  Puerto 5060 (UDP y TCP)
Honeypot:     Sentrypeer
Patrón:       SIP REGISTER flood — múltiples puertos efímeros simultáneos
Tipo:         VoIP Toll Fraud — registro de extensiones no autorizadas
Eventos:      241,724 (solo esta IP durante el período observado)
Período:      Activo desde el primer día — persistió los 5 días completos
```

> ![SIP Flood - Sentrypeer](screenshots/sip_flood_sentrypeer.png)
> ![tcpdump SIP](screenshots/tcpdump_sip_capture.png)

---

### 3. 🗄️ Escaneo Coordinado de Elasticsearch — Honeypot ElasticPot

A las 14:42 del 10 de marzo, tres IPs distintas —`134.209.78.215`, `137.184.197.230` y `137.184.201.88`— todas del rango de DigitalOcean en Estados Unidos, realizaron conexiones al puerto **9200** en **exactamente el mismo segundo**. La precisión de esta sincronización es evidencia directa de un script automatizado ejecutándose en paralelo desde múltiples nodos, probablemente un framework de reconocimiento distribuido. Una cuarta IP de Singapur (`178.128.221.39`), marcada como **Known Attacker**, también participó en el escaneo coordinado.

El objetivo típico de estos ataques es encontrar instancias de Elasticsearch expuestas sin autenticación para robar o cifrar los datos y exigir rescate —una modalidad conocida como *database ransomware*.

```
IPs origen:   134.209.78.215 / 137.184.197.230 / 137.184.201.88  (EE.UU.)
              178.128.221.39  (Singapur — Known Attacker)
Protocolo:    Elasticsearch  →  Puerto 9200
Honeypot:     ElasticPot
Patrón:       Escaneo multi-fuente sincronizado en el mismo timestamp exacto
```

> ![Elasticsearch Scan](screenshots/elasticsearch_scan.png)

---

### 4. 🌐 Scanning HTTPS — Honeypot H0neytr4p

La IP `89.248.168.239` (Países Bajos), clasificada como **Known Attacker** en la base de reputación de T-Pot, mantuvo una campaña persistente contra el puerto **443 (HTTPS)** durante múltiples días. Las ráfagas de hasta 8 conexiones simultáneas por segundo son características de escáneres de vulnerabilidades web multi-hilo como **Nuclei** o **Nikto**, diseñados para identificar paneles de administración expuestos, APIs sin autenticación y vulnerabilidades conocidas en frameworks web populares.

```
IP origen:    89.248.168.239  (Países Bajos — Known Attacker)
Protocolo:    HTTPS  →  Puerto 443
Honeypot:     H0neytr4p
Patrón:       Ráfagas de hasta 8 conexiones HTTPS simultáneas por segundo
```

> ![HTTPS Scan](screenshots/https_scan_h0neytr4p.png)

---

## 🛡️ Respuesta Activa — Mitigación en Tiempo Real

Una parte central del proyecto fue no limitarse a la observación pasiva, sino aplicar mitigación activa durante la sesión —replicando el flujo de trabajo real de un analista SOC.

### Bloqueo con `iptables` — Cadena DOCKER-USER

La cadena `DOCKER-USER` es el punto de intercepción correcto en entornos Docker. Se evalúa en la cadena `FORWARD` del kernel, **antes** de que Docker entregue el paquete al contenedor destino, a diferencia de las reglas de `INPUT` donde opera `ufw`.

```bash
# Bloquear todo el tráfico del atacante principal a nivel Docker
sudo iptables -I DOCKER-USER -s 51.38.144.6 -j DROP

# Bloqueo específico del puerto SIP en UDP y TCP
sudo iptables -I DOCKER-USER -p udp --dport 5060 -s 51.38.144.6 -j DROP
sudo iptables -I DOCKER-USER -p tcp --dport 5060 -s 51.38.144.6 -j DROP

# Verificar reglas y contadores de tráfico bloqueado
sudo iptables -L DOCKER-USER -n -v
```

La verificación confirmó **38 paquetes / 22,987 bytes** descartados en el momento de la captura. El comando `sudo ss -ntu | grep 51.38.144.6` confirmó que no existían conexiones activas — el bloqueo era completo.

> ![iptables DOCKER-USER](screenshots/iptables_docker_user.png)

---

### Firewall de Host con `ufw`

`ufw` fue configurado para endurecer el sistema operativo host de forma independiente a Docker:

```bash
# Permitir SSH antes de activar el firewall (evitar quedar bloqueado)
sudo ufw allow OpenSSH

# Bloquear el atacante más agresivo globalmente
sudo ufw deny from 51.38.144.6

# Cerrar el puerto SIP completamente
sudo ufw deny 5060

# Habilitar el firewall
sudo ufw enable

# Verificar estado final
sudo ufw status
```

El estado final confirmó: `51.38.144.6 → DENY`, `Puerto 5060 → DENY` (IPv4 + IPv6), `OpenSSH → ALLOW`.

> ⚠️ **Lección técnica clave:** Al aplicar `sudo ufw deny 5060`, `tcpdump` seguía mostrando paquetes SIP activos llegando al honeypot. Docker inyecta reglas en la cadena `PREROUTING` de `iptables` con precedencia sobre las reglas de `INPUT` donde opera `ufw`. La solución definitiva fue `DOCKER-USER`. En entornos Docker, **`ufw` y `DOCKER-USER` son complementarios, no equivalentes** — esto es una de las brechas de configuración más comunes en entornos de producción.

> ![UFW Status](screenshots/ufw_firewall_rules.png)

---

### Captura y Verificación con `tcpdump`

`tcpdump` fue usado en tres momentos distintos para verificar el contenido real de los ataques a nivel de paquete:

```bash
# Capturar todo el tráfico del atacante en la interfaz eth0
sudo tcpdump -i eth0 host 51.38.144.6

# Monitorear específicamente el puerto SIP
sudo tcpdump -i eth0 port 5060
```

Las capturas confirmaron mensajes `SIP REGISTER` enviados desde múltiples puertos efímeros del bloque `ip6.ip-51-38-144.eu.*`, con el honeypot respondiendo `SIP/2.0 200 OK` —simulando un servidor SIP legítimo que acepta los registros, lo que mantenía al atacante activo y maximizaba la captura de datos de inteligencia.

> ![tcpdump SIP](screenshots/tcpdump_sip_capture.png)

---

## 🔬 Inteligencia de Amenazas — Dashboard Suricata y Kibana

El dashboard de T-Pot consolidó todos los datos y produjo las siguientes tablas de inteligencia durante los cinco días de operación.

### Top 10 ASNs Atacantes

| AS | Organización | Eventos |
|---|---|---|
| 16276 | OVH SAS | 244,644 |
| 71 | Hewlett-Packard Company | 75,041 |
| 11556 | Cable & Wireless Panama | 39,729 |
| 207083 | HostSlim B.V. | 18,909 |
| 52055 | Mayak Smart Services | 11,818 |
| 14061 | DigitalOcean, LLC | 7,367 |
| 20473 | Choopa, LLC (Vultr) | 3,995 |
| 28267 | LANTEC Comunicaciones | 3,691 |
| 8452 | TE-AS (Telecom Egypt) | 3,148 |
| 51852 | Private Layer INC | 2,574 |

### Top 10 IPs Atacantes

| IP origen | Eventos |
|---|---|
| 51.38.144.6 | 241,724 |
| 15.235.104.234 | 67,236 |
| 186.73.123.68 | 39,729 |
| 194.50.16.198 | 18,870 |
| 190.61.86.226 | 11,562 |
| 209.38.27.202 | 8,939 |
| 15.204.135.78 | 5,505 |
| 187.108.1.130 | 3,691 |
| 41.36.32.70 | 3,148 |

### CVEs Detectados por Suricata

| CVE ID | Detecciones | Significado |
|---|---|---|
| **CVE-2024-38816** | 69 | Vulnerabilidad crítica de 2024 — activamente explotada con herramientas automatizadas |
| CVE-2020-11900 | 2 | Stack TCP/IP Treck — explotación remota de dispositivos IoT |
| CVE-2020-11910 | 1 | Stack Treck — parsing incorrecto de paquetes ICMPv4 |

La presencia de **CVE-2024-38816** con 69 detecciones es el hallazgo más significativo del IDS: siendo una vulnerabilidad publicada en 2024, su incorporación activa en herramientas automatizadas de ataque demuestra que la ventana entre la divulgación pública de un CVE y su explotación masiva se ha reducido a semanas o días.

> ![Dashboard Suricata - Kibana](screenshots/suricata_dashboard.png)

---

## 📅 Cronología del Proyecto (5 días completos)

| Fecha / Hora | Evento |
|---|---|
| **08-03 ~ 15:30** | Droplet aprovisionado en DigitalOcean. T-Pot CE instalado y configurado. Inicio de captura. |
| **08-03 (noche)** | Primeras conexiones. Sentrypeer comienza a registrar flood SIP de `51.38.144.6`. |
| **10-03 ~ 11:19** | Sesión de monitoreo activo. Pico máximo: 224/min · 26,364/hora · 358,368/24h. |
| **10-03 ~ 12:01** | Segundo atacante `64.226.93.85` (EE.UU., Known Attacker) activo en Honeytrap puerto 11434. |
| **10-03 ~ 12:12** | Bloqueo `iptables DOCKER-USER` aplicado. Tráfico horario cae ~95% en la hora siguiente. |
| **10-03 ~ 14:42** | Escaneo Elasticsearch coordinado — 3 IPs EE.UU. + 1 Singapur en el mismo segundo exacto. |
| **10-03 ~ 16:26** | UFW configurado: `deny from 51.38.144.6`, `deny 5060`, `allow OpenSSH`. Estado: active. |
| **10-03 ~ 17:02** | `tcpdump port 5060` confirma flujo bidireccional `SIP REGISTER` + `SIP/2.0 200 OK`. |
| **10-03 ~ 17:17** | Bloqueo SIP específico: `DOCKER-USER` para UDP y TCP puerto 5060. Verificado con `-L`. |
| **11-03 al 12-03** | Campañas automatizadas continúan. Cowrie y Sentrypeer activos los 5 días completos. |
| **12-03 ~ 00:00** | Droplet destruido. Fin del proyecto. **Costo total: $5.75 USD.** |

---

## 💡 Lecciones Técnicas Clave

**`ufw` no protege los contenedores Docker.** Al aplicar `sudo ufw deny 5060`, `tcpdump` seguía mostrando tráfico SIP llegando al honeypot. Docker manipula las cadenas de `iptables` en `PREROUTING`, con precedencia sobre `INPUT` donde opera `ufw`. La solución fue insertar la regla en `DOCKER-USER`, que intercepta en `FORWARD` antes del reenvío al contenedor.

**SIP supera a SSH en volumen de ataques.** Contrario a lo que muestran muchos análisis publicados donde SSH domina, en este proyecto SIP generó aproximadamente el 67% del tráfico total. Esto refleja la industrialización del VoIP Toll Fraud como vector económicamente lucrativo y altamente automatizado.

**Los CVEs recientes se explotan de forma automatizada en semanas.** CVE-2024-38816 ya tenía 69 detecciones activas menos de dos años tras su publicación, integrado en frameworks de ataque automatizados.

**Un laboratorio real de Threat Intelligence cuesta menos de $6 USD.** La combinación de DigitalOcean (pago por hora) y T-Pot CE (código abierto) elimina completamente la barrera económica para hacer investigación de seguridad con datos reales del Internet.

---

## 📁 Estructura del Repositorio

```
tpot-honeypot-lab/
│
├── README.md                         # Este archivo
│
├── screenshots/                      # Capturas de pantalla del laboratorio
│   ├── attack_map_livefeed.png       # T-Pot Attack Map con Live Feed activo
│   ├── attack_map_sip.png            # Mapa durante el pico de ataque SIP
│   ├── ssh_bruteforce_cowrie.png     # Campaña SSH desde 198.143.191.202
│   ├── sip_flood_sentrypeer.png      # Flood SIP desde 51.38.144.6
│   ├── elasticsearch_scan.png        # Escaneo coordinado puerto 9200
│   ├── https_scan_h0neytr4p.png      # Scanning HTTPS desde Países Bajos
│   ├── tcpdump_sip_capture.png       # Captura tcpdump de mensajes SIP REGISTER
│   ├── iptables_docker_user.png      # Reglas DOCKER-USER con contadores activos
│   ├── ufw_firewall_rules.png        # Estado UFW con reglas aplicadas
│   └── suricata_dashboard.png        # Dashboard Kibana con top ASNs y CVEs
│
├── videos/                           # Grabaciones de pantalla de sesiones en vivo
│   ├── sesion_monitoreo_01.mp4
│   ├── sesion_monitoreo_02.mp4
│   ├── mitigacion_iptables.mp4
│   └── analisis_tcpdump.mp4
│
└── notas/
    └── resumen_ataques.md            # Notas detalladas por campaña de ataque
```

---

## ⚠️ Aviso Ético y Legal

Este honeypot fue desplegado exclusivamente con fines de **investigación personal en ciberseguridad**. Todos los sistemas documentados son honeypots — no se involucró ningún sistema de producción real, datos de usuarios ni servicios de terceros. Las IPs de atacantes documentadas son direcciones públicas que iniciaron conexiones no solicitadas a un sistema de investigación monitoreado. La observación y documentación de este tráfico es completamente legal en el marco de un sistema propio expuesto al Internet público.

---

## 👥 Autores

**Edyn Dominguez** — Proyecto Personal de Ciberseguridad · Marzo 2026
**Enrique Jou** — Proyecto Personal de Ciberseguridad · Marzo 2026

---

## 📚 Referencias

- [T-Pot CE — Repositorio oficial (Deutsche Telekom Security)](https://github.com/telekom-security/tpotce)
- [Cowrie SSH/Telnet Honeypot](https://github.com/cowrie/cowrie)
- [SentryPeer — VoIP Honeypot](https://github.com/SentryPeer/SentryPeer)
- [Suricata IDS/IPS](https://suricata.io/)
- [CVE-2024-38816 — NVD Detail](https://nvd.nist.gov/vuln/detail/CVE-2024-38816)
- [AbuseIPDB — Threat Intelligence](https://www.abuseipdb.com/)
- [DigitalOcean Droplets](https://www.digitalocean.com/products/droplets)

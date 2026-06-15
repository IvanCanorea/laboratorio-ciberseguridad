# Laboratorio de Ciberseguridad e Infraestructura

Laboratorio personal de seguridad ofensiva y defensiva montado sobre Proxmox con automatizacion mediante agentes de IA.

## Arquitectura

```
+-------------------------------------------------+
|                  PROXMOX VE                      |
|              (192.168.1.156)                     |
|                                                  |
|  +--------------+  +--------------+             |
|  |   Suricata   │  │   Hermes     │             |
|  |   IDS/IPS    │  │   Agente IA  │             |
|  |   (IDS)      │  │   (OWL)      │             |
|  +------+-------+  +------+-------+             |
|         │                 │                      |
|  +------+-----------------+------+              |
|  │      Red Interna (VLAN)       │              |
|  │      192.168.1.0/24           │              |
|  +--------------+----------------+              |
+-------------------------------------------------+
                  │
         +--------+--------+
         │   Kali Linux    │
         │   (Atacante)    │
         │   VMware/Local  │
         └────────────────-+
```

## Componentes

### Suricata IDS/IPS
- **Version:** Suricata 7.0.3
- **Modo:** IDS (deteccion)
- **Interfaz:** eth0@if9 (192.168.1.0/24)
- **Reglas:** 282 reglas base + 9 reglas personalizadas
- **Logs:** `/var/log/suricata/eve.json`

### Reglas Personalizadas (SID 1000001-1000009)

| SID | Descripcion |
|-----|-------------|
| 1000001 | Escaneo SYN (>50 SYN en 10s desde misma IP) |
| 1000002 | NULL scan (puertos con flags vacias) |
| 1000003 | XMAS scan (flags FPU) |
| 1000004 | Fuerza bruta SSH (10 intentos en 60s) |
| 1000005 | Volumen alto DNS (>100 consultas en 60s) |
| 1000006 | Intento de acceso a /admin en HTTP |
| 1000007 | Path traversal (../ en URLs) |
| 1000008 | User-Agent Nmap detectado |
| 1000009 | Escaneo UDP (>50 paquetes en 10s) |

Ver archivo completo: `suricata/rules/local.rules`

### Hermes - Agente IA
- **Framework:** Hermes Agent (Nous Research)
- **Funcion:** Procesar alertas de Suricata y generar respuestas automaticas
- **Integracion:** API REST + n8n workflows

## Servicios del Laboratorio

| Servicio | Puerto | Descripcion |
|----------|--------|-------------|
| Suricata | - | IDS/IPS en modo escucha |
| Hermes API | 3004 | Agente IA para gestion |
| n8n | 5678 | Workflows de automatizacion |
| Web Luna | 3003 | Chat con modelo local (Ollama) |

## Escenarios de Ataque

Documentados en `kali/escenarios/`:

1. **Escaneo de puertos con Nmap** (-sS, -sN, -sX)
2. **Fuerza bruta SSH con Hydra**
3. **Escaneo web con Nikto**
4. **Inyeccion SQL con SQLMap**
5. **Trafico DNS sospechoso**

## Roadmap

- [x] Instalacion de Suricata IDS en Proxmox
- [x] Configuracion de 9 reglas personalizadas
- [ ] Prueba de escenarios de ataque desde Kali
- [ ] Integracion Suricata + Hermes (alertas automaticas)
- [ ] Dashboard de visualizacion de alertas
- [ ] Bloqueo automatico de IPs maliciosas

## Sobre este proyecto

Este laboratorio forma parte de mi formacion en ciberseguridad e infraestructura.
Mi objetivo es entender como funcionan los sistemas de deteccion de intrusos
desde dentro, y como la IA puede ayudar a automatizar la respuesta a incidentes.

**Autor:** Ivan Canorea
**LinkedIn:** [Ivan Canorea](https://www.linkedin.com/in/ivancanorea04)

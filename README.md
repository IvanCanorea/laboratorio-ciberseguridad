# Laboratorio de Ciberseguridad e Infraestructura

Laboratorio personal de seguridad ofensiva y defensiva montado sobre Proxmox con automatizacion mediante agentes de IA.

## Arquitectura

```
┌─────────────────────────────────────────────────┐
│                  PROXMOX VE                      │
│              (192.168.1.156)                     │
│                                                  │
│  ┌──────────────┐  ┌──────────────┐             │
│  │   Suricata   │  │   Hermes     │             │
│  │   IDS/IPS    │  │   Agente IA  │             │
│  │   (IDS)      │  │   (OWL)      │             │
│  └──────┬───────┘  └──────┬───────┘             │
│         │                 │                      │
│  ┌──────┴─────────────────┴──────┐              │
│  │      Red Interna (VLAN)       │              │
│  │      192.168.1.0/24           │              │
│  └──────────────┬────────────────┘              │
│                 │                                │
└─────────────────┼────────────────────────────────┘
                  │
         ┌────────┴────────┐
         │   Kali Linux    │
         │   (Atacante)    │
         │   VMware/Local  │
         └─────────────────┘
```

## Componentes

### Suricata IDS/IPS
- **Version:** Suricata 7.0.3
- **Modo:** IDS (deteccion)
- **Interfaz:** eth0@if9 (192.168.1.0/24)
- **Reglas:** Emerging Threats Open + reglas base del paquete
- **Logs:** `/var/log/suricata/eve.json`
- **Alertas procesadas por:** Hermes (agente IA)

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

## Seguridad

Este laboratorio esta aislado en red interna (192.168.1.0/24). 
El trafico de ataque se genera desde Kali Linux en VMware sobre la misma red local.

**Reglas de uso:**
- Solo para propositos educativos y de aprendizaje
- Todo el trafice se genera en red interna aislada
- No se expone ningun servicio a Internet sin proteccion

## Roadmap

- [x] Instalacion de Suricata IDS
- [ ] Configuracion de reglas personalizadas
- [ ] Integracion Suricata + Hermes (alertas automaticas)
- [ ] Dashboard de visualizacion de alertas
- [ ] Escenarios de ataque controlados desde Kali
- [ ] Documentacion de cada escenario

## Sobre este proyecto

Este laboratorio forma parte de mi formacion en ciberseguridad e infraestructura.
Mi objetivo es entender como funcionan los sistemas de deteccion de intrusos
desde dentro, y como la IA puede ayudar a automatizar la respuesta a incidentes.

**Autor:** Ivan Canorea
**LinkedIn:** [Ivan Canorea](https://www.linkedin.com/in/ivancanorea04)

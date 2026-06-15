# Integracion Hermes + Suricata

Automatizacion de respuesta a alertas mediante agente de IA.

## Arquitectura

Suricata detecta amenazas -> Eve JSON -> Hermes procesa -> Accion automatica

## Flujo de trabajo

1. Suricata genera alerta en `/var/log/suricata/eve.json`
2. Hermas monitoriza cambios en el archivo (inotify)
3. Hermes analiza la alerta (tipo, severidad, origen)
4. Segun reglas predefinidas, ejecuta accion:
 - Alerta baja -> Log + notificacion
 - Alerta media -> Log + notificacion + captura de paquetes
 - Alerta alta -> Log + notificacion + bloqueo temporal (iptables)

## Configuracion

### 1. Script de monitoreo

```bash
#!/bin/bash
# /root/hermes/scripts/suricata_monitor.sh
inotifywait -m -e modify /var/log/suricata/eve.json | while read; do
 NEW_ALERT=$(tail -1 /var/log/suricata/eve.json)
 curl -s -X POST http://localhost:3004/api/suricata/alert \
 -H "Content-Type: application/json" \
 -d "$NEW_ALERT"
done
```

### 2. n8n Workflow

El workflow de n8n recibe la alerta y:
- Clasifica la severidad
- Busca la IP en AbuseIPDB
- Genera informe
- Notifica por Telegram (cuando este configurado)

## Estados

- [x] Recepcion de alertas desde Suricata
- [ ] Clasificacion automatica por Hermes
- [ ] Integracion con n8n
- [ ] Notificaciones Telegram
- [ ] Bloqueo automatico de IPs maliciosas

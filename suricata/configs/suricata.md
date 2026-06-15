# Configuracion de Suricata IDS

Archivo de configuracion principal para Suricata 7.0.3 en modo IDS.

## Ubicacion

```
/etc/suricata/suricata.yaml
```

## Reglas cargadas

| Archivo | Descripcion |
|---------|-------------|
| app-layer-events.rules | Eventos de capa de aplicacion |
| decoder-events.rules | Eventos del decodificador |
| dns-events.rules | Consultas DNS sospechosas |
| files.rules | Deteccion de archivos maliciosos |
| http-events.rules | Trafico HTTP anomalo |
| ssh-events.rules | Intentos SSH sospechosos |
| tls-events.rules | Certificados TLS invalidos |
| smtp-events.rules | Correo sospechoso |
| stream-events.rules | Anomalias en el flujo TCP |

## Logs

| Archivo | Descripcion |
|---------|-------------|
| /var/log/suricata/eve.json | Eventos en JSON (alertas, HTTP, DNS, TLS) |
| /var/log/suricata/fast.log | Alertas en texto plano |
| /var/log/suricata/stats.log | Estadisticas del motor |

## Comandos utiles

```bash
# Ver estado
systemctl status suricata

# Ver alertas en tiempo real
tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'

# Recargar reglas sin reiniciar
suricatasc -c reload-rules

# Ver estadisticas
cat /var/log/suricata/stats.log | tail -20

# Test de configuracion
suricata -T -c /etc/suricata/suricata.yaml
```

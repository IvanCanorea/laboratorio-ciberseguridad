# Escenarios de Ataque desde Kali Linux

Escenarios controlados para probar las detecciones de Suricata.

## Escenario 1: Escaneo de Puertos con Nmap

```bash
# Escaneo SYN rapido
nmap -sS -p 1-1024 192.168.1.156

# Escaneo de servicios
nmap -sV -p 22,80,443 192.168.1.156

# Escaneo agresivo (genera mas alertas)
nmap -A -T4 192.168.1.156
```

**Alertas esperadas en Suricata:**
- Deteccion de escaneo SYN
- Deteccion de fingerprinting de servicios

## Escenario 2: Fuerza Bruta SSH con Hydra

```bash
# Ataque SSH con diccionario basico
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.156 -t 4

# Version mas rapida (menos ruido)
hydra -l admin -P passwords.txt ssh://192.168.1.156 -t 1 -w 30
```

**Alertas esperadas:**
- Multiples conexiones SSH desde misma IP
- Reglas de fuerza bruta SSH

## Escenario 3: Ataque Web con Nikto

```bash
# Escaneo web basico
nikto -h http://192.168.1.156

# Con puerto especifico
nikto -h http://192.168.1.156:8080
```

**Alertas esperadas:**
- Deteccion de escaneo web
- Patrones de ataque HTTP

## Escenario 4: Inyeccion SQL con SQLMap

```bash
# Test basico de SQLi
sqlmap -u "http://192.168.1.156/page?id=1" --batch --level=1

# Solo detectar, sin explotar
sqlmap -u "http://192.168.1.156/page?id=1" --batch --dbs --risk=1
```

**Alertas esperadas:**
- Patrones de inyeccion SQL en HTTP
- Anomalias en parametros URL

## Escenario 5: Trafico DNS Sospechoso

```bash
# Generar consultas DNS masivas
for i in $(seq 1 100); do dig test$i.lab.local @192.168.1.156; done

# Transferencia de zona (si hay DNS)
dig axfr @192.168.1.156 lab.local
```

**Alertas esperadas:**
- Volumen alto de consultas DNS
- Patrones de exfiltracion DNS

## Verificacion de Alertas

```bash
# Ver todas las alertas en tiempo real
tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert") | {timestamp: .timestamp, signature: .alert.signature, src_ip: .src_ip, dest_ip: .dest_ip}'

# Contar alertas por tipo
cat /var/log/suricata/eve.json | jq -r '.alert.signature' 2>/dev/null | sort | uniq -c | sort -rn

# Ver alertas de un escenario especifico
grep "escaneo" /var/log/suricata/fast.log
```

## Notas de Seguridad

- Estos escenarios SOLO se ejecutan en red interna aislada
- Nunca ejecutar contra sistemas sin autorizacion
- Documentar cada escenario con capturas para el portfolio

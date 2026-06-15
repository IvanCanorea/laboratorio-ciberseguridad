# Documentacion del Laboratorio

## Primeros pasos

### Requisitos del sistema
- Proxmox VE como hipervisor
- VM con Ubuntu 24.04 LTS (minimo 2GB RAM, 2 CPU, 40GB disco)
- Kali Linux (atacante) en VMware o bare metal
- Conexión de red entre ambas maquinas

### Instalacion rapida

```bash
# Instalar Suricata
sudo apt update && sudo apt install -y suricata

# Actualizar reglas
sudo suricata-update

# Configurar interfaz de red
sudo nano /etc/suricata/suricata.yaml
# Cambiar 'interface' por tu interfaz (ej: eth0)

# Iniciar servicio
sudo systemctl start suricata
sudo systemctl enable suricata

# Verificar
sudo systemctl status suricata
tail -f /var/log/suricata/fast.log
```

## Posts de LinkedIn relacionados

- [Post 1: Montando mi primer IDS con Suricata](link-post-1)
- [Post 2: Integrando IA en mi laboratorio de ciberseguridad](link-post-2)
- [Post 3: Escenario de ataque: escaneo Nmap vs Suricata](link-post-3)

## Aprendizajes

### Instalacion de Suricata en Ubuntu 24.04

**Problema:** Las reglas de Emerging Threats Open (~44k reglas) daban error de parsing (~25k fallaban).

**Solucion:** Usar las reglas base del paquete Suricata que vienen en `/etc/suricata/rules/`. Son menos reglas pero todas validas y bien mantenidas.

**Leccion:** Mas reglas != mejor deteccion. Mejor reglas de calidad que cantidad.

### Configuracion YAML

**Problema:** El archivo de configuracion no arrancaba. Error: "The configuration file must begin with %YAML 1.1 and ---".

**Solucion:** Aniadir el header YAML al inicio del archivo.

**Leccion:** Suricata es estricto con el formato YAML. Siempre incluir el header.

# Reglas Personalizadas de Suricata

Reglas custom para el laboratorio de ciberseguridad.

## Estructura

Las reglas siguen el formato estandar de Suricata:

```
alert [protocolo] [origen] -> [destino] (msg:"descripcion"; sid:XXXX; rev:1;)
```

## Reglas incluidas

### Escaneo de puertos
Detecta escaneos SYN desde la red interna.

```
alert tcp any any -> $HOME_NET any (msg:"LAB - Posible escaneo SYN"; flags:S; threshold:type both, track by_src, count 20, seconds 60; sid:1000001; rev:1; classtype:attempted-recon;)
```

### Fuerza bruta SSH
Detecta multiples intentos de conexion SSH fallidos.

```
alert ssh any any -> $HOME_NET 22 (msg:"LAB - Posible fuerza bruta SSH"; flow:to_server,established; threshold:type both, track by_src, count 5, seconds 120; sid:1000002; rev:1; classtype:attempted-admin;)
```

### Consultas DNS sospechosas
Detecta consultas a dominios conocidos por C2/malware.

```
alert dns any any -> any any (msg:"LAB - Consulta DNS a dominio sospechoso"; dns.query; content:"malware-test.lab"; nocase; sid:1000003; rev:1; classtype:trojan-activity;)
```

## Como aniadir reglas

1. Edita el archivo `suricata.rules` en `/etc/suricata/rules/`
2. O crea un nuevo archivo `.rules` y aniadeelo a `suricata.yaml`
3. Recarga: `suricatasc -c reload-rules`

## Testing

Para verificar que una regla funciona:

```bash
# Enviar trafico de prueba desde Kali
hping3 -S -p 80 --flood 192.168.1.156

# Ver si se genera alerta
tail -f /var/log/suricata/eve.json | jq 'select(.alert.signature_id==1000001)'
```

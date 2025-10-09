# Configuración de Switches para VLANs

## Configuración Base Cisco
```cisco
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name VLAN_CORPORATIVA
Switch(config-vlan)# exit
Switch(config)# vlan 100
Switch(config-vlan)# name VLAN_SCADA_CONTROL
Switch(config-vlan)# exit
```

### Configuración Puertos Access:
```cisco
Switch(config)# interface FastEthernet0/1
Switch(config-if)# description HMI-LINEA01
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 100
Switch(config-if)# spanning-tree portfast
Switch(config-if)# exit
```

### Configuración Puertos Trunk:

```cisco
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description TRUNK-to-CORE-SW
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 999
Switch(config-if)# switchport trunk allowed vlan 10,100,200
Switch(config-if)# exit
```

## Conceptos Clave

### Puertos Trunk:
- Llevan múltiples VLANs
- Requieren switches compatables
- Nativos para VLAN sin tag

### Puertos Access:
- Una sola VLAN
- Dispositivos finales
- No tagged frames

## Mejores Prácticas

### Seguridad:

```cisco
! Deshabilitar puertos no usados
Switch(config)# interface range FastEthernet0/10-24
Switch(config-if-range)# shutdown
Switch(config-if-range)# exit

! Configurar BPDU Guard
Switch(config)# spanning-tree portfast bpduguard default

! Limitar MAC addresses
Switch(config-if)# switchport port-security maximum 3
```

### Monitoring:

```cisco
! Configurar SNMP para monitoring
Switch(config)# snmp-server community public RO
Switch(config)# snmp-server host 10.0.10.100 version 2c public

! Logging de cambios
Switch(config)# logging host 10.0.10.101
```

## Comandos de Verificación

```cisco
Switch# show vlan brief
Switch# show interface trunk
Switch# show running-config
Switch# show mac address-table
```

# Reglas de Firewall entre VLANs

## Política Base

### Default Deny All:
```bash
# Política por defecto
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP
```

## Reglas Específicas por VLAN

### VLAN_SCADA_CONTROL (100) → VLAN_IoT_FIELD (200):

```bash
# Permitir Modbus TCP desde HMI a PLCs
iptables -A FORWARD -s 10.0.100.0/24 -d 10.0.200.0/24 -p tcp --dport 502 -j ACCEPT

# Permitir OPC UA desde SCADA a PLCs
iptables -A FORWARD -s 10.0.100.0/24 -d 10.0.200.0/24 -p tcp --dport 4840 -j ACCEPT

# Permitir EtherNet/IP
iptables -A FORWARD -s 10.0.100.0/24 -d 10.0.200.0/24 -p tcp --dport 44818 -j ACCEPT
```

### VLAN_VPN (400) → VLAN_SCADA_CONTROL (100):

```bash
# SSH para administración desde direcciones específicas
iptables -A FORWARD -s 10.0.400.50 -d 10.0.100.0/24 -p tcp --dport 22 -j ACCEPT

# RDP para acceso a HMIs
iptables -A FORWARD -s 10.0.400.0/24 -d 10.0.100.100 -p tcp --dport 3389 -j ACCEPT
```

## Protocolos Industriales Comunes

### Modbus TCP:

- **Puerto:** 502
- **Dirección:** Maestro → Esclavo
- **Seguridad:** Sin autenticación nativa

### OPC UA:

- **Puerto:** 4840
- **Características:** Autenticación, Encryption
- **Recomendación:** Permitir bidireccional para discovery

### PROFINET:

- **Puertos:** 34962-34964
- **Tipo:** RT (Real-Time)
- **Consideraciones:** Prioridad alta

## Reglas de Logging

### Log de Intentos Denegados:

```bash
# Log de intentos de acceso denegados
iptables -A FORWARD -j LOG --log-prefix "FIREWALL-DENIED: " --log-level 4

# Log de acceso a VLAN crítica
iptables -A FORWARD -d 10.0.200.0/24 -j LOG --log-prefix "ACCESS-FIELD: " --log-level 6
```

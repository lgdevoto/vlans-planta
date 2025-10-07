# Esquema Básico de VLANs para Planta Industrial

## Esquema Mínimo Recomendado

| VLAN ID | Nombre VLAN | Propósito | Dispositivos Típicos | Subred Recomendada | Política de Acceso |
|---------|-------------|-----------|---------------------|-------------------|-------------------|
| 10 | VLAN_CORPORATIVA | Usuarios oficina | PCs, Teléfonos IP, Servidores | 10.0.10.0/24 | Acceso internet y corporativo |
| 20 | VLAN_INVITADOS | Visitantes | Wi-Fi invitados | 10.0.20.0/24 | Solo salida internet |
| 30 | VLAN_SERVICIOS | Servicios generales | Impresoras, CCTV, NAS | 10.0.30.0/24 | Acceso limitado desde otras VLANs |
| 100 | VLAN_SCADA_CONTROL | Supervisión | HMIs, Clients SCADA, Historian | 10.0.100.0/24 | Solo → VLAN_IoT_FIELD |
| 200 | VLAN_IoT_FIELD | Dispositivos campo | PLCs, PACs, Sensores, VFDs | 10.0.200.0/24 | Solo ← VLAN_SCADA_CONTROL |
| 300 | VLAN_ROBOTICA | Automatización | Robots, CNC, Vision Systems | 10.0.300.0/24 | Comunicación punto-punto |
| 400 | VLAN_VPN | Acceso remoto | Engineers, Proveedores | 10.0.400.0/24 | Limitado a VLAN_SCADA + FIELD |

## Convenciones de Subredes

### Esquema de Direccionamiento:
- **/24 por VLAN** (254 hosts máximo)
- **10.0.X.0/24** donde X = VLAN ID
- **Gateway:** 10.0.X.1
- **Broadcast:** 10.0.X.255

### Excepciones:
- VLANs muy grandes: /23 (510 hosts)
- VLANs punto-punto: /30 (2 hosts)

## Nomenclatura Recomendada

### Switches:
- `SW-PLT01-FLOOR1-CORE`
- `SW-PLT01-CELL01-ACCESS`

### Interfaces:
- `Gi1/0/1 - HMI-LINEA01`
- `Gi1/0/2 - PLC-STATION01`

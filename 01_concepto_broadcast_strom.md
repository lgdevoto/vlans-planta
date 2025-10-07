# Qué es Broadcast Storm para empezar?
Una broadcast storm es una condición de red donde un gran número de paquetes de broadcast son enviados a través de la red, saturando el ancho de banda disponible y consumiendo los recursos de todos los dispositivos conectados.

## Ejemplo Real con Impresora
Caso típico:
- Impresora de oficina desarrolla falla de hardware
- Comienza a enviar paquetes DHCP de forma continua
- Tasa de transmisión: 100-1000 paquetes/segundo
- En red plana, afecta a todos los dispositivos

## Impacto en Redes Industriales

### Consecuencias Directas:
- Alta latencia en comunicación PLC-HMI
- Pérdida de paquetes en protocolos industriales
- Timeouts en comandos de control
- Parada no planificada de líneas de producción

### Costo Estimado:
- Parada de producción: $10,000-$50,000 por hora
- Tiempo de recuperación: 2-4 horas
- Impacto en calidad y entregas

## ¿Por qué las VLANs Mitigan este Problema?

### Mecanismo de Contención:
1. Dominios de Broadcast Separados
2. Límites de Tráfico en Capa 2
3. Control en Capa 3 mediante Firewalls
4. Aislamiento de Fallos

### Resultado:
- Falla en VLAN_SERVICIOS → No afecta VLAN_SCADA
- Comunicaciones críticas protegidas
- Contención del problema al segmento afectado

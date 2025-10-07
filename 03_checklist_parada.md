# Los puntos más importantes a verificar en una parada

## Pre-Parada

### Verificación de Puertos
- [ ] Documentar todos los puertos a modificar
- [ ] Confirmar asignamientos de VLAN en diagramas
- [ ] Verificar configuraciones de backup
- [ ] Comunicar ventana de mantenimiento

### Validación de Reglas Firewall
- [ ] Revisar reglas existentes entre VLANs
- [ ] Confirmar políticas "denegar todo" por defecto
- [ ] Validar reglas específicas permitidas
- [ ] Verificar logging de paquetes denegados

## Durante Parada

### Configuración Switches
- [ ] Aplicar configuraciones VLAN en el switch
- [ ] Configurar puertos trunk entre switches
- [ ] Establecer puertos access por dispositivo
- [ ] Configurar VLAN nativa (si aplica)

### Asignación IPs
- [ ] Configurar interfaces VLAN en routers
- [ ] Asignar IPs del gateway por VLAN
- [ ] Actualizar DNS/DHCP scopes
- [ ] Documentar cambios en inventario

## Post-Parada

### Pruebas de Comunicación
- [ ] Test ping entre dispositivos misma VLAN
- [ ] Validar comunicación inter-VLAN permitida
- [ ] Verificar bloqueo comunicación inter-VLAN denegada
- [ ] Test acceso a internet desde VLANs correspondientes

### Pruebas de Contención
- [ ] Generar tráfico broadcast VLANs de servicios
- [ ] Monitorear latencia VLANs críticas 
- [ ] Verificar inmutabilidad de procesos críticos
- [ ] Validar logs de firewall

## Rollback Plan
- [ ] Backup configuraciones pre-cambio
- [ ] Procedimiento rollback documentado
- [ ] Ventana de rollback definida
- [ ] Comunicación equipos afectados

# Definición de broadcast

transmisión de datos donde un mensaje se envía a todos los dispositivos en una red local o segmento de red al mismo tiempo, sin necesidad de conocer sus direcciones individuales.

# ¿Por qué una impresora puede generar tráfico de broadcast excesivo?
1. Falla de hardware o firmware
Un bug en el firmware de red o una falla en el chip Ethernet puede hacer que la impresora pierda su configuración IP.
Al no tener IP válida, intenta obtener una nueva constantemente vía DHCP, enviando solicitudes en broadcast cada pocos milisegundos.

2. Bucle de resolución ARP
Si la impresora no logra resolver direcciones MAC de dispositivos con los que intenta comunicarse, puede entrar en un bucle de ARP requests.
Esto ocurre si su tabla ARP se corrompe o si no recibe respuestas válidas, lo que la lleva a repetir la solicitud indefinidamente.

3. Mal diseño de recuperación de red
Algunos dispositivos tienen mecanismos de “auto-recovery” mal implementados: si pierden conexión, reinician su stack de red y vuelven a emitir DHCP y ARP como si fueran nuevos en la red.
Si esto ocurre en ciclos rápidos (cada 1–2 segundos), el volumen de tráfico se dispara.

4. Conflicto de IP o MAC
Si hay conflicto de IP (dos dispositivos con la misma IP), ambos pueden entrar en ciclos de broadcast para reclamar la dirección.
También puede ocurrir si hay spoofing o duplicación de MAC en la red.

5. Reintentos sin backoff
Algunos firmwares no implementan correctamente el exponential backoff (espera progresiva entre reintentos).
Resultado: la impresora reintenta DHCP o ARP sin pausa, generando cientos o miles de paquetes por segundo.

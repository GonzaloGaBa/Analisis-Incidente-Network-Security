# Informe de Incidente: Mitigación de Ataque DoS (SYN Flood)

## 1. Resumen Ejecutivo
Este proyecto documenta el análisis y la respuesta ante una interrupción de servicio en un servidor web corporativo. A través del análisis de logs de tráfico (Wireshark), se identificó un ataque de denegación de servicio (DoS) que saturó los recursos del sistema, impidiendo el acceso a usuarios legítimos.

## 2. Detalles del Incidente
* **Activo Afectado:** Servidor Web (IP: 192.0.2.1)
* **Vector de Ataque:** Inundación SYN (SYN Flood)
* **Origen del Ataque:** IP Maliciosa 203.0.113.0
* **Impacto:** Error 504 Gateway Timeout y paquetes [RST, ACK] para tráfico legítimo (empleados en el rango 198.51.100.0/24).

## 3. Análisis Técnico (Protocolo TCP)
El ataque aprovechó la vulnerabilidad en el "Three-way Handshake" de TCP:
1. El atacante envió múltiples paquetes **[SYN]** a una velocidad de varios por segundo.
2. El servidor reservó recursos para cada conexión, pero al no recibir los paquetes **[ACK]** finales, agotó su capacidad.
3. **Resultado:** El servidor dejó de responder a solicitudes legítimas a partir de la entrada de log #125.

## 4. Medidas de Mitigación Propuestas
Basado en el marco **NIST**, se recomiendan las siguientes acciones:
* **Detección:** Configurar alertas en el IDS (Suricata) para picos inusuales de paquetes SYN.
* **Contención:** Implementar "SYN Cookies" en el servidor para evitar la reserva prematura de memoria.
* **Prevención:** Configurar reglas de Firewall para limitar el ritmo (Rate Limiting) de solicitudes desde una misma IP de origen.

## 5. Herramientas Utilizadas
* **Wireshark** (Análisis de capturas de red).
* **Metodología NIST** para reporte de incidentes.

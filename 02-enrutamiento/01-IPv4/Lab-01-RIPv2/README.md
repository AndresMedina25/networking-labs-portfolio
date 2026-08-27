# 🌐 Implementación del Protocolo RIPv2 (IPv4) en Cisco Packet Tracer

Este laboratorio documenta la configuración y convergencia de una topología de red estructurada mediante el protocolo de enrutamiento dinámico **RIP versión 2 (RIPv2)** en un entorno IPv4.

---

## 📌 Descripción del Escenario
La infraestructura está compuesta por **12 routers Cisco 2811** encargados de interconectar múltiples redes de área local (LAN) mediante enlaces de área amplia (WAN) serie.

## 🛠️ Especificaciones Técnicas

* **Dispositivos:** Routers Cisco 2811 y Switches Cisco Catalyst 2960.
* **Protocolo de Enrutamiento:** RIPv2 (`router rip`, `version 2`).
* **Suma de Rutas:** Desactivada (`no auto-summary`) para soporte completo de VLSM/CIDR.
* **Direccionamiento LAN:** Subredes `/24` en el rango `192.168.51.0/24` a `192.168.62.0/24`.
* **Direccionamiento WAN (Enlaces Seriales):** Subredes en el rango `192.169.10.0` a `192.169.40.0`.
* **Sincronización DCE:** Clock rate de `64000` bps en las interfaces seriales DCE.

---

## 📂 Estructura del Repositorio

* **`PROTOCOLO_RIP.pkt`**: Archivo de simulación en Cisco Packet Tracer con la topología totalmente configurada y convergida.
* **`Routers-Configs.txt`**: Script de respaldo con las configuraciones IP y de enrutamiento aplicadas en los 12 routers.
* **`README.md`**: Documentación técnica del laboratorio.

---

## 🚀 Instrucciones de Uso

La topología ya cuenta con todos los dispositivos y protocolos previamente configurados:

1. Abre el archivo **`PROTOCOLO_RIP.pkt`** en **Cisco Packet Tracer**.
2. Espera unos segundos a que la red converja completamente.
3. Procede directamente con las pruebas de verificación de conectividad y enrutamiento.

---

## 🔍 Verificación y Pruebas

Para comprobar el funcionamiento y la convergencia de la red:

* **Tabla de enrutamiento (CLI del Router):**
  ```ios
  show ip route

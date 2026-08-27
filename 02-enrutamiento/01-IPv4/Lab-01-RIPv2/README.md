# 🌐 Implementación de Protocolo RIPv2 (IPv4) en Packet Tracer

Este repositorio contiene la configuración de una topología de red estructurada en Cisco Packet Tracer. El proyecto implementa el protocolo de enrutamiento dinámico RIP versión 2 para lograr la convergencia de toda la red IPv4.

## 📌 Descripción de la Topología

La red está compuesta por 12 routers modelo Cisco 2811 que interconectan múltiples redes de área local (LAN) y enlaces de área amplia (WAN). 


## 🛠️ Tecnologías y Protocolos Utilizados

* **Dispositivos**: Routers Cisco 2811 y Switches de la serie 2960.
* **Enrutamiento**: Protocolo RIPv2 habilitado en todos los routers (`router rip`, `version 2`).
* **Direccionamiento LAN**: Subredes en el rango `192.168.51.0` hasta `192.168.62.0` (máscara /24).
* **Direccionamiento WAN (Serial)**: Subredes en el rango `192.169.10.0` hasta `192.169.40.0`.
* **Sincronización WAN**: Interfaces Serial DCE configuradas con un `clock rate` de 64000.

## 📂 Archivos del Repositorio

* `README.md`: Documentación de la topología y el proyecto.
* `router_configs.txt`: Script de configuración consolidado con las direcciones IP y los procesos de enrutamiento de los 12 routers.
* `PROTOCOLO_RIP.pkt`: Archivo de Cisco Packet Tracert con la configuración de la red y la red funcionando.

## 🚀 Cómo utilizar estas configuraciones

1. Abre tu topología en Cisco Packet Tracer.
2. Ingresa a la pestaña **CLI** de cada router.
3. Entra al modo de configuración global (`enable` > `configure terminal`).
4. Copia y pega el bloque de código correspondiente al router desde el archivo `router_configs.txt`.

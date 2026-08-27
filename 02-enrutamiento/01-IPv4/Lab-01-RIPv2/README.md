# 🌐 Implementación de Protocolo RIPv2 (IPv4) en Packet Tracer

Este repositorio contiene la configuración de una topología de red estructurada en Cisco Packet Tracer. El proyecto implementa el protocolo de enrutamiento dinámico RIP versión 2 para lograr la convergencia de toda la red IPv4.[cite: 1]

## 📌 Descripción de la Topología

La red está compuesta por 12 routers modelo Cisco 2811 que interconectan múltiples redes de área local (LAN) y enlaces de área amplia (WAN).[cite: 1] 

Puedes visualizar el diseño lógico y físico en los siguientes diagramas:
* ![Topología Parte 1](Captura%20de%20pantalla%202026-08-27%20144652.png)[cite: 1]
* ![Topología Parte 2](Captura%20de%20pantalla%202026-08-27%20144708.png)[cite: 1]

## 🛠️ Tecnologías y Protocolos Utilizados

* **Dispositivos**: Routers Cisco 2811 y Switches de la serie 2960.[cite: 1]
* **Enrutamiento**: Protocolo RIPv2 habilitado en todos los routers (`router rip`, `version 2`).[cite: 1]
* **Direccionamiento LAN**: Subredes en el rango `192.168.51.0` hasta `192.168.62.0` (máscara /24).[cite: 1]
* **Direccionamiento WAN (Serial)**: Subredes en el rango `192.169.10.0` hasta `192.169.40.0`.[cite: 1]
* **Sincronización WAN**: Interfaces Serial DCE configuradas con un `clock rate` de 64000.[cite: 1]

## 📂 Archivos del Repositorio

* `README.md`: Documentación de la topología y el proyecto.
* `router_configs.txt`: Script de configuración consolidado con las direcciones IP y los procesos de enrutamiento de los 12 routers.[cite: 1]

## 🚀 Cómo utilizar estas configuraciones

1. Abre tu topología en Cisco Packet Tracer.
2. Ingresa a la pestaña **CLI** de cada router.
3. Entra al modo de configuración global (`enable` > `configure terminal`).
4. Copia y pega el bloque de código correspondiente al router desde el archivo `router_configs.txt`.

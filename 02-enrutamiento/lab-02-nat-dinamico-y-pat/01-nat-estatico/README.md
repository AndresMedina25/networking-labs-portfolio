# Laboratorio 01: NAT Estático

## Objetivo
Implementar y verificar la configuración de NAT Estático en un router Cisco, permitiendo la traducción de una dirección IP interna específica a una dirección IP externa de forma fija.

## Topología de la Red

![Topología de Red NAT Estático](./topologia.png)

La red está compuesta por tres enrutadores principales: **SUCURSAL 1**, **MATRIZ** y **SUCURSAL 2**. 
*   El router **MATRIZ** actúa como núcleo, conectándose a **SUCURSAL 1** a través de la red `195.10.100.0/28` y a **SUCURSAL 2** mediante la red `195.10.100.16/28`.
*   **SUCURSAL 1** administra una red LAN (`195.10.1.0/25`) con los equipos PC1 y PC2, además de un servidor web Cisco (`195.10.2.0/30`).
*   **SUCURSAL 2** administra una red LAN (`195.10.4.0/25`) con los equipos PC3 y PC4, y un servidor FTP (`195.10.3.0/30`).

## Conceptos Clave de Configuración

Para implementar NAT Estático, es necesario seguir dos pasos fundamentales:
1.  **Mapeo de Direcciones:** Crear la asignación entre direcciones locales internas y locales externas. Esto se logra mediante el comando:
    `ip nat inside source static ip-local ip-global`.
2.  **Asignación de Interfaces:** Definir qué interfaces pertenecen a la red interna y cuáles a la red externa. Se utilizan los comandos `ip nat inside` e `ip nat outside` respectivamente.

## Análisis de la Configuración en SUCURSAL 1

En este laboratorio, el router **SUCURSAL 1** es el encargado de realizar la traducción NAT. Según los archivos de configuración provistos:

*   **Interfaz Interna:** La interfaz `GigabitEthernet0/0` (conectada a la LAN 195.10.1.0/25) se define con el comando `ip nat inside`.
*   **Interfaz Externa:** La interfaz `Serial0/0/0` (conectada a la red WAN 195.10.100.0/28) se define con el comando `ip nat outside`.
*   **Regla NAT Estática:** Se implementa el comando `ip nat inside source static 195.10.1.2 195.10.100.2`. Esto mapea directamente el host interno (195.10.1.2) a la dirección IP pública/externa de la interfaz serial (195.10.100.2).
*   **Enrutamiento:** Se utiliza el protocolo RIP versión 2 en todos los routers para la conectividad de la red.

## Archivos de Configuración

Los scripts completos (Cisco IOS) para cada router se encuentran en la carpeta `/configs`:
*   [Router SUCURSAL 1](./configs/SUCURSAL1.txt)
*   [Router SUCURSAL 2](./configs/SUCURSAL2.txt)
*   [Router MATRIZ](./configs/MATRIZ.txt)

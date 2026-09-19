# Módulo 02: Traducción de Direcciones de Red (NAT)

## Descripción General
Network Address Translation es un protocolo utilizado en redes para traducir direcciones IP entre un conjunto de direcciones internas (privadas) y un conjunto de direcciones externas (públicas). Este proceso permite que múltiples dispositivos en una red local (LAN) accedan a recursos externos, como Internet, usando una o más direcciones IP públicas.

### ¿Por qué se utiliza NAT?
*   Conservación de direcciones IP.
*   Seguridad.
*   Flexibilidad en la administración de red.

---

## Estructura de Topologías en Cisco Packet Tracer

Este directorio divide la práctica en tres laboratorios independientes. Cada subcarpeta contiene su respectiva topología simulada, los scripts de configuración de los routers y un archivo de documentación detallando los comandos específicos:

### 1. [01-nat-estatico](./01-nat-estatico)
*   **Concepto:** Asigna una dirección IP pública fija a una dirección IP privada fija.
*   **Aplicación:** Esto es útil para servidores que necesitan ser accesibles desde el exterior con una dirección IP específica.

### 2. [02-nat-dinamico](./02-nat-dinamico)
*   **Concepto:** Asigna una dirección IP pública de un pool de direcciones IP públicas disponibles a las direcciones IP privadas de forma dinámica.
*   **Aplicación:** No garantiza que la misma dirección IP pública se asigne siempre a un dispositivo interno.

### 3. [03-pat-overload](./03-pat-overload)
*   **Concepto:** Port Address Translation (PAT), también conocido como NAT de puerto o NAT de sobrecarga.
*   **Aplicación:** Permite que múltiples dispositivos internos compartan una única dirección IP pública utilizando diferentes números de puerto.

---

## Comandos de Verificación

En cualquiera de las tres implementaciones, puedes utilizar la CLI del router para validar las traducciones activas y el uso de los *pools*:

*   `show ip nat translations`: Muestra la tabla de mapeo actual entre direcciones internas y externas.
*   `show ip nat statistics`: Despliega estadísticas de uso, interfaces asignadas y aciertos de traducción.

---

## Referencias
*   Teoría y configuración basada en el tutorial: [NAT, NAT Dinámico y PAT](https://www.youtube.com/watch?v=_tyySTI6_CE&t=2255s).

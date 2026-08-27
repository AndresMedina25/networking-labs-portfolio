# 🌐 Laboratorio: Enrutamiento Dinámico IPv4 con RIPv2 (R1 a R12)

Este laboratorio documenta la implementación del protocolo de enrutamiento dinámico **RIP versión 2** sobre una topología multizona compuesta por 12 routers Cisco (R1 a R12). La red utiliza un núcleo en anillo interconectado por enlaces seriales y múltiples segmentos FastEthernet distribuidos en subredes con máscara `/24`.

---

## 🏗️ Arquitectura de la Topología

* **Core Backbone (Anillo Serial):** Routers **R9, R10, R11 y R12** interconectados mediante enlaces seriales punto a punto (`192.169.x.0/24`).
* **Zonas de Distribución:** Cada router del núcleo conecta mediante un switch local a dos routers de acceso/extremo:
  * **R9:** Conecta a R1 y R2 (`192.168.53.0/24`).
  * **R10:** Conecta a R5 y R6 (`192.168.54.0/24`).
  * **R11:** Conecta a R3 y R4 (`192.168.57.0/24`).
  * **R12:** Conecta a R7 y R8 (`192.168.60.0/24`).
* **Segmentos LAN (Extremos):** Subredes IPv4 asignadas hacia los PCs finales A-H.

---

## 📊 Tabla de Resumen de Direccionamiento IPv4

| Dispositivo | Interfaz | Dirección IPv4 / Subred | Función / Destino |
| :--- | :--- | :--- | :--- |
| **R1** | Fa0/0 | `192.168.53.1/24` | Enlace hacia Switch0 (R9/R2) |
| | Fa0/1 | `192.168.51.1/24` | Gateway LAN PC-A |
| **R2** | Fa0/0 | `192.168.53.3/24` | Enlace hacia Switch0 (R9/R1) |
| | Fa0/1 | `192.168.52.1/24` | Gateway LAN PC-B |
| **R3** | Fa0/0 | `192.168.57.2/24` | Enlace hacia Switch1 (R11/R4) |
| | Fa0/1 | `192.168.58.1/24` | Gateway LAN PC-C |
| **R4** | Fa0/0 | `192.168.57.3/24` | Enlace hacia Switch1 (R11/R3) |
| | Fa0/1 | `192.168.59.1/24` | Gateway LAN PC-D |
| **R5** | Fa0/0 | `192.168.54.2/24` | Enlace hacia Switch3 (R10/R6) |
| | Fa0/1 | `192.168.55.1/24` | Gateway LAN PC-E |
| **R6** | Fa0/0 | `192.168.54.3/24` | Enlace hacia Switch3 (R10/R5) |
| | Fa0/1 | `192.168.56.1/24` | Gateway LAN PC-F |
| **R7** | Fa0/0 | `192.168.60.2/24` | Enlace hacia Switch2 (R12/R8) |
| | Fa0/1 | `192.168.61.1/24` | Gateway LAN PC-G |
| **R8** | Fa0/0 | `192.168.60.3/24` | Enlace hacia Switch2 (R12/R7) |
| | Fa0/1 | `192.168.62.1/24` | Gateway LAN PC-H |
| **R9 (Core)** | Fa0/0 | `192.168.53.2/24` | Segmento Distribución Oeste |
| | Se0/1/0 | `192.169.10.2/24` | Enlace Serial hacia R10 |
| | Se0/0/0 | `192.169.40.1/24` | Enlace Serial hacia R12 |
| **R10 (Core)**| Fa0/0 | `192.168.54.1/24` | Segmento Distribución Norte |
| | Se0/1/0 | `192.169.10.1/24` | Enlace Serial hacia R9 |
| | Se0/0/0 | `192.169.20.2/24` | Enlace Serial hacia R11 |
| **R11 (Core)**| Fa0/0 | `192.168.57.1/24` | Segmento Distribución Este |
| | Se0/1/0 | `192.169.20.1/24` | Enlace Serial hacia R10 |
| | Se0/0/0 | `192.169.30.2/24` | Enlace Serial hacia R12 |
| **R12 (Core)**| Fa0/0 | `192.168.60.1/24` | Segmento Distribución Sur |
| | Se0/0/0 | `192.169.30.1/24` | Enlace Serial hacia R11 |
| | Se0/1/0 | `192.169.40.2/24` | Enlace Serial hacia R9 |

---

## 🛠️ Configuración Clave de RIPv2

Para que el protocolo RIPv2 funcione correctamente en esta red IPv4, se requiere:
1. Acceder al proceso de enrutamiento global (`router rip`).
2. Especificar el uso de la versión 2 (`version 2`).
3. Declarar directamente las redes directamente conectadas utilizando el comando `network [ip_de_red]`.

---

## 🔍 Comandos de Verificación

```cisco
! Verificar la tabla de enrutamiento IPv4 para RIP
show ip route rip

! Inspeccionar los parámetros del proceso de enrutamiento activo
show ip protocols

! Probar conectividad extremo a extremo
ping 192.168.51.2

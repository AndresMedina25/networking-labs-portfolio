# 🌐 Laboratorio: Enrutamiento Dinámico IPv6 con RIPng (R1 a R12)

Este laboratorio documenta la implementación del protocolo de enrutamiento dinámico **RIPng (RIP Next Generation)** sobre una topología multizona compuesta por 12 routers Cisco (R1 a R12). La red utiliza un núcleo en anillo interconectado por enlaces seriales y múltiples segmentos FastEthernet distribuidos en prefijos `/112`.

---

## 🏗️ Arquitectura de la Topología

* **Core Backbone (Anillo Serial):** Routers **R9, R10, R11 y R12** interconectados mediante enlaces seriales punto a punto (`1A:BACA:101A::x/112`).
* **Zonas de Distribución:** Cada router del núcleo conecta mediante un switch local a dos routers de acceso/extremo:
  * **R9:** Conecta a R1 y R2 (`C01A:E1:CAFE::51:0/112`).
  * **R10:** Conecta a R5 y R6 (`C01A:E1:CAFE::54:0/112`).
  * **R11:** Conecta a R3 y R4 (`C01A:E1:CAFE::57:0/112`).
  * **R12:** Conecta a R7 y R8 (`C01A:E1:CAFE::60:0/112`).
* **Segmentos LAN (Extremos):** Subredes IPv6 asignadas hacia los PCs finales A-H (`C01A:E1:CAFE::x:0/112`).

---

## 📊 Tabla de Resumen de Direccionamiento IPv6

| Dispositivo | Interfaz | Dirección IPv6 / Subred | Función / Destino |
| :--- | :--- | :--- | :--- |
| **R1** | Fa0/0 | `C01A:E1:CAFE::51:2/112` | Enlace hacia Switch0 (R9/R2) |
| | Fa0/1 | `C01A:E1:CAFE::52:1/112` | Gateway LAN PC-A |
| **R2** | Fa0/0 | `C01A:E1:CAFE::51:3/112` | Enlace hacia Switch0 (R9/R1) |
| | Fa0/1 | `C01A:E1:CAFE::53:1/112` | Gateway LAN PC-B |
| **R3** | Fa0/0 | `C01A:E1:CAFE::57:2/112` | Enlace hacia Switch1 (R11/R4) |
| | Fa0/1 | `C01A:E1:CAFE::58:1/112` | Gateway LAN PC-C |
| **R4** | Fa0/0 | `C01A:E1:CAFE::57:3/112` | Enlace hacia Switch1 (R11/R3) |
| | Fa0/1 | `C01A:E1:CAFE::59:1/112` | Gateway LAN PC-D |
| **R5** | Fa0/0 | `C01A:E1:CAFE::54:2/112` | Enlace hacia Switch3 (R10/R6) |
| | Fa0/1 | `C01A:E1:CAFE::55:1/112` | Gateway LAN PC-E |
| **R6** | Fa0/0 | `C01A:E1:CAFE::54:3/112` | Enlace hacia Switch3 (R10/R5) |
| | Fa0/1 | `C01A:E1:CAFE::56:1/112` | Gateway LAN PC-F |
| **R7** | Fa0/0 | `C01A:E1:CAFE::60:2/112` | Enlace hacia Switch2 (R12/R8) |
| | Fa0/1 | `C01A:E1:CAFE::61:1/112` | Gateway LAN PC-G |
| **R8** | Fa0/0 | `C01A:E1:CAFE::60:3/112` | Enlace hacia Switch2 (R12/R7) |
| | Fa0/1 | `C01A:E1:CAFE::62:1/112` | Gateway LAN PC-H |
| **R9 (Core)** | Fa0/0 | `C01A:E1:CAFE::51:1/112` | Segmento Distribución Oeste |
| | Se0/1/0 | `1A:BACA:101A::10:2/112` | Enlace Serial hacia R10 |
| | Se0/0/0 | `1A:BACA:101A::40:1/112` | Enlace Serial hacia R12 |
| **R10 (Core)**| Fa0/0 | `C01A:E1:CAFE::54:1/112` | Segmento Distribución Norte |
| | Se0/1/0 | `1A:BACA:101A::10:1/112` | Enlace Serial hacia R9 |
| | Se0/0/0 | `1A:BACA:101A::20:2/112` | Enlace Serial hacia R11 |
| **R11 (Core)**| Fa0/0 | `C01A:E1:CAFE::57:1/112` | Segmento Distribución Este |
| | Se0/1/0 | `1A:BACA:101A::20:1/112` | Enlace Serial hacia R10 |
| | Se0/0/0 | `1A:BACA:101A::30:2/112` | Enlace Serial hacia R12 |
| **R12 (Core)**| Fa0/0 | `C01A:E1:CAFE::60:1/112` | Segmento Distribución Sur |
| | Se0/0/0 | `1A:BACA:101A::30:1/112` | Enlace Serial hacia R11 |
| | Se0/1/0 | `1A:BACA:101A::40:2/112` | Enlace Serial hacia R9 |

---

## 🛠️ Configuración Clave de RIPng

A diferencia de IPv4, el protocolo **RIPng** requiere:
1. Habilitación global del enrutamiento IPv6 (`ipv6 unicast-routing`).
2. Declaración del proceso RIPng global (`ipv6 router rip RIP-IPV6`).
3. Activación directa dentro de cada interfaz mediante el comando `ipv6 rip RIP-IPV6 enable`.

---

## 🔍 Comandos de Verificación

```cisco
! Verificar la tabla de enrutamiento IPv6 para RIP
show ipv6 route rip

! Inspeccionar los parámetros del proceso RIPng y vecinos
show ipv6 rip RIP-IPV6

! Probar conectividad extremo a extremo
ping C01A:E1:CAFE::52:2

## 📐 Topología de Red

Debido a la extensión de la red, puedes inspeccionar y ejecutar la topología interactiva completa descargando directamente el proyecto simulado:

📂 **[Descargar archivo .pkt del laboratorio](./PROTOCOLO_RIP_IPV.6.pkt)**

> ℹ️ **Nota de compatibilidad:** Requiere **Cisco Packet Tracer v8.0** o superior.



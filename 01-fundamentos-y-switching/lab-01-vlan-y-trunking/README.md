🌐 Virtual Local Area Networks (VLANs)

## ¿Qué es una VLAN?
Una **VLAN (Red de Área Local Virtual)** es una tecnología de redes de Capa 2 (Enlace de Datos) que permite crear redes lógicas independientes dentro de una misma red física. Funciona segmentando lógicamente un switch, de modo que los dispositivos conectados a distintas VLANs se comportan como si estuvieran conectados a switches físicos completamente diferentes, aislando su tráfico en distintos dominios de broadcast.

## ¿Por qué se utilizan?
En una red LAN tradicional, todos los dispositivos conectados a un switch reciben las tramas de difusión (broadcast). A medida que la red crece, el exceso de tráfico de broadcast puede saturar el ancho de banda y degradar el rendimiento. Las VLANs se utilizan para contener este tráfico, segmentar la red según funciones lógicas (independientemente de la ubicación física de los usuarios) y aplicar políticas de seguridad granulares.

## Ventajas Principales
*   **Reducción del tamaño de los dominios de broadcast:** Al dividir la red, el tráfico de broadcast de una VLAN no se propaga a otra, liberando ancho de banda.
*   **Mejora de la seguridad:** Los datos confidenciales pueden separarse del resto de la red. Un usuario en la VLAN de "Invitados" no puede acceder de forma nativa a la VLAN de "Finanzas".
*   **Reducción de costos:** No es necesario comprar switches físicos separados para cada departamento o subred.
*   **Mayor eficiencia de TI y flexibilidad:** Facilita la administración de la red. Si un empleado cambia de escritorio físico pero se mantiene en el mismo departamento, solo basta con reasignar el puerto del switch a su VLAN correspondiente sin mover cables en el centro de datos.

## Principales Usos
1.  **Segmentación por Departamentos:** Separar tráfico de RRHH, Contabilidad, TI y Ventas.
2.  **Redes de Invitados:** Proveer internet a visitantes sin darles acceso a los recursos corporativos.
3.  **Aislamiento de Tráfico Sensible:** Separar el tráfico de voz (VoIP) del tráfico de datos para garantizar Calidad de Servicio (QoS).
4.  **Gestión de Dispositivos IoT:** Aislar cámaras IP, sensores y sistemas de control de acceso del tráfico de usuarios.

---

## Tipos de VLANs

| Tipo de VLAN | Descripción |
| :--- | :--- |
| **VLAN de Datos** | Dedicada exclusivamente al tráfico generado por los usuarios (archivos, correos, navegación web). También se le conoce como VLAN de usuario. |
| **VLAN Predeterminada** | Es la VLAN asignada a todos los puertos de un switch por defecto al encenderlo. En los equipos Cisco, es la **VLAN 1**. No se puede renombrar ni eliminar. Por seguridad, se recomienda no usarla para datos. |
| **VLAN Nativa** | Se asigna a un puerto troncal (802.1Q). Su función es transportar el tráfico "sin etiquetar" (untagged) que llega al enlace troncal. Por defecto es la VLAN 1, pero por seguridad siempre debe cambiarse a otra VLAN no utilizada (ej. VLAN 99). |
| **VLAN de Administración** | Es una VLAN configurada con una interfaz virtual (SVI) para acceder al switch remotamente mediante SSH, Telnet o HTTP. |
| **VLAN de Voz** | Una VLAN separada específicamente para teléfonos IP. Permite aplicar políticas de QoS para evitar latencia o pérdida de paquetes en las llamadas, separándola del tráfico de datos de la PC conectada al mismo teléfono. |

---

## Comandos de Configuración (Cisco IOS)

A continuación, se detallan los comandos esenciales para configurar cada aspecto de las VLANs en dispositivos Cisco.

### 1. Creación y asignación de VLAN de Datos
```bash
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name VENTAS
Switch(config-vlan)# exit

# Asignar un puerto de acceso a la VLAN
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

### 2. Configuración de VLAN de Voz
```bash
Switch(config)# vlan 20
Switch(config-vlan)# name VOIP
Switch(config-vlan)# exit

Switch(config)# interface fastEthernet 0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10      # VLAN para la PC
Switch(config-if)# switchport voice vlan 20       # VLAN para el Teléfono IP
```
### 3. Configuración de VLAN de Administración (SVI)
```bash
Switch(config)# vlan 99
Switch(config-vlan)# name ADMIN
Switch(config-vlan)# exit

Switch(config)# interface vlan 99
Switch(config-if)# ip address 192.168.99.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.99.1   # Para alcance fuera de la red local
```
### 4. Configuración de Enlaces Troncales y VLAN Nativa
```bash
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# switchport mode trunk

# Cambiar la VLAN Nativa (Buena práctica de seguridad)
Switch(config-if)# switchport trunk native vlan 999

# Restringir las VLANs permitidas (Buena práctica de seguridad)
Switch(config-if)# switchport trunk allowed vlan 10,20,99,999
```
### 5. Comandos de Verificación
```bash
Switch# show vlan brief             # Muestra las VLANs creadas y sus puertos asignados
Switch# show interfaces trunk       # Muestra los puertos troncales y la VLAN nativa
Switch# show interfaces fa0/1 switchport # Muestra el estado detallado de capa 2 del puerto
```

# Práctica: Implementación de VLANs, Enlaces Troncales y Telefonía IP

En esta práctica se simula una red corporativa distribuida, implementando segmentación lógica mediante VLANs para diferentes departamentos, configuración de enlaces troncales entre switches, y la integración de VLANs de voz para teléfonos IP.


### Direccionamiento y Segmentación
La red está segmentada en las siguientes VLANs con sus respectivas subredes:
*   🟡 **VLAN 10 (Facultad):** Red `172.16.10.0/24`.
*   🟢 **VLAN 20 (Estudiantes):** Red `172.16.20.0/24`.
*   🟣 **VLAN 30 (Invitados):** Red `172.16.30.0/24`.
*   🔵 **VLAN 150 (Voz):** Dedicada a la telefonía IP.
## ⚙️ Proceso de Configuración

La red cuenta con un Switch de distribución central (Switch 2) y dos Switches de acceso (SW1 y SW3).

### 1. Configuración del Switch 1 (SW1)
Este switch da acceso a los equipos del ala izquierda de la topología.
*   **VLANs creadas:** 10, 20, 30 y 150.
*   **Enlace Troncal:** El puerto `FastEthernet 0/1` se configura en modo Trunk para conectarse con el Switch 2.
*   **Puertos de Acceso:**
    *   `f0/11`: Asignado a la VLAN 10 (Facultad).
    *   `f0/6`: Asignado a la VLAN 30 (Invitados).
*   **Puerto Híbrido (Datos + Voz):**
    *   `f0/18`: Se configura con acceso a la VLAN 20 para la PC y VLAN de voz 150 para el Teléfono IP. Además, se aplica el comando `mls qos trust cos` para confiar en las etiquetas de prioridad del tráfico de voz.

### 2. Configuración del Switch 2 (Distribución)
Este equipo central interconecta los switches de acceso.
*   **VLANs creadas:** Se replican las VLANs 10, 20, 30 y 150 en su base de datos para que reconozca el tráfico etiquetado.
*   **Enlaces Troncales:** Utilizando el comando de rango `interface range f0/1, f0/3`, se configuran los puertos de conexión hacia SW1 y SW3 en modo Trunk simultáneamente.
### 3. Configuración del Switch 3 (SW3)
Este switch da acceso a los equipos del ala derecha.
*   **VLANs creadas:** 10, 20, 30 y 150.
*   **Enlace Troncal:** El puerto `FastEthernet 0/3` se configura en modo Trunk hacia el Switch 2.
*   **Puertos de Acceso:**
    *   `f0/11`: Asignado a la VLAN 10.
    *   `f0/6`: Asignado a la VLAN 30.
*   **Puerto Híbrido (Datos + Voz):**
    *    al igual que en SW1, el puerto `f0/18` se configura con acceso a la VLAN 20, Voice VLAN 150, y se habilita la confianza QoS (`mls qos trust cos`).

## 🧪 Verificación
Para comprobar que la configuración se aplicó correctamente, se utilizaron los siguientes comandos en los switches:
*   `show vlan brief`: Permite verificar que las VLANs se crearon con sus nombres correctos y que los puertos de acceso están asignados a la VLAN adecuada.
*   `show interface f0/1 switchport`: En SW1, este comando confirmó que el puerto opera administrativamente como troncal con encapsulamiento `dot1q`.
*   `show run`: Utilizado para hacer una revisión global de la configuración en ejecución (Running-config).

---
*Para probar esta topología, puedes descargar el archivo `.pkt` ubicado en la carpeta `simulaciones` y abrirlo con Cisco Packet Tracer.*

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

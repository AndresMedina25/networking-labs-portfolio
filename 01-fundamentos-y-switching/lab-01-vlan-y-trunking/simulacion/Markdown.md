# Práctica: Implementación de VLANs, Enlaces Troncales y Telefonía IP

En esta práctica se simula una red corporativa distribuida, implementando segmentación lógica mediante VLANs para diferentes departamentos, configuración de enlaces troncales entre switches, y la integración de VLANs de voz para teléfonos IP.

## Topología de la Red

*(Coloca aquí la imagen de la topología)*
![Topología de VLANs](image_c25a5f.png)

### Direccionamiento y Segmentación
La red está segmentada en las siguientes VLANs con sus respectivas subredes[cite: 4]:
*   🟡 **VLAN 10 (Facultad):** Red `172.16.10.0/24`[cite: 1, 4]
*   🟢 **VLAN 20 (Estudiantes):** Red `172.16.20.0/24`[cite: 1, 4]
*   🟣 **VLAN 30 (Invitados):** Red `172.16.30.0/24`[cite: 1, 4]
*   🔵 **VLAN 150 (Voz):** Dedicada a la telefonía IP[cite: 1, 4]

## ⚙️ Proceso de Configuración

La red cuenta con un Switch de distribución central (Switch 2) y dos Switches de acceso (SW1 y SW3)[cite: 1, 2, 3, 4].

### 1. Configuración del Switch 1 (SW1)
Este switch da acceso a los equipos del ala izquierda de la topología[cite: 4].
*   **VLANs creadas:** 10, 20, 30 y 150[cite: 1].
*   **Enlace Troncal:** El puerto `FastEthernet 0/1` se configura en modo Trunk para conectarse con el Switch 2[cite: 1].
*   **Puertos de Acceso:**
    *   `f0/11`: Asignado a la VLAN 10 (Facultad)[cite: 1].
    *   `f0/6`: Asignado a la VLAN 30 (Invitados)[cite: 1].
*   **Puerto Híbrido (Datos + Voz):**
    *   `f0/18`: Se configura con acceso a la VLAN 20 para la PC y VLAN de voz 150 para el Teléfono IP. Además, se aplica el comando `mls qos trust cos` para confiar en las etiquetas de prioridad del tráfico de voz[cite: 1].

### 2. Configuración del Switch 2 (Distribución)
Este equipo central interconecta los switches de acceso[cite: 4].
*   **VLANs creadas:** Se replican las VLANs 10, 20, 30 y 150 en su base de datos para que reconozca el tráfico etiquetado[cite: 2].
*   **Enlaces Troncales:** Utilizando el comando de rango `interface range f0/1, f0/3`, se configuran los puertos de conexión hacia SW1 y SW3 en modo Trunk simultáneamente[cite: 2].

### 3. Configuración del Switch 3 (SW3)
Este switch da acceso a los equipos del ala derecha[cite: 4].
*   **VLANs creadas:** 10, 20, 30 y 150[cite: 3].
*   **Enlace Troncal:** El puerto `FastEthernet 0/3` se configura en modo Trunk hacia el Switch 2[cite: 3].
*   **Puertos de Acceso:**
    *   `f0/11`: Asignado a la VLAN 10[cite: 3].
    *   `f0/6`: Asignado a la VLAN 30[cite: 3].
*   **Puerto Híbrido (Datos + Voz):**
    *    al igual que en SW1, el puerto `f0/18` se configura con acceso a la VLAN 20, Voice VLAN 150, y se habilita la confianza QoS (`mls qos trust cos`)[cite: 3].

## 🧪 Verificación
Para comprobar que la configuración se aplicó correctamente, se utilizaron los siguientes comandos en los switches:
*   `show vlan brief`: Permite verificar que las VLANs se crearon con sus nombres correctos y que los puertos de acceso están asignados a la VLAN adecuada[cite: 1, 2, 3].
*   `show interface f0/1 switchport`: En SW1, este comando confirmó que el puerto opera administrativamente como troncal con encapsulamiento `dot1q`[cite: 1].
*   `show run`: Utilizado para hacer una revisión global de la configuración en ejecución (Running-config)[cite: 1, 3].

---
*Para probar esta topología, puedes descargar el archivo `.pkt` ubicado en la carpeta `simulaciones` y abrirlo con Cisco Packet Tracer.*

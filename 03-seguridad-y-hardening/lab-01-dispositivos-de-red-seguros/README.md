# 🔒 Laboratorio: Configuración de Dispositivos de Red Seguros (R1 y S1)

Este laboratorio se enfoca en el fortalecimiento de seguridad (**hardening**) para routers y switches Cisco IOS, aplicando políticas de contraseñas complejas, acceso remoto seguro mediante SSH, temporizadores de inactividad, protección contra ataques de fuerza bruta y mitigación de accesos físicos no autorizados mediante la desactivación de puertos inactivos.

---

## 📊 Tabla de Direccionamiento IP

| Dispositivo | Interfaz | Dirección IP | Máscara de Subred | Gateway Predeterminado |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | G0/0/1 | `192.168.1.1` | `255.255.255.0` | N/A |
| **S1** | VLAN 1 (SVI) | `192.168.1.11` | `255.255.255.0` | `192.168.1.1` |
| **PC-A** | NIC | `192.168.1.3` | `255.255.255.0` | `192.168.1.1` |

---

## 🔑 Medidas de Seguridad Implementadas

1. **Gestión Compleja de Contraseñas:**
   * Encriptación global de contraseñas guardadas en texto plano (`service password-encryption`).
   * Longitud mínima obligatoria de 12 caracteres (`security passwords min-length 12`).
   * Actualización de contraseñas cifradas para EXEC privilegiado, Consola y VTY.

2. **Acceso Remoto Seguro (SSH v2):**
   * Creación de usuario administrador local (`sshAdmin`).
   * Generación de llaves criptográficas RSA de 1024 bits.
   * Restricción exclusiva de líneas VTY para recibir tráfico SSH (`transport input ssh`).

3. **Protección y Tolerancia a Fallos:**
   * Cierre automático de sesión tras 5 minutos de inactividad (`exec-timeout 5 0`).
   * Bloqueo de inicios de sesión por 120 segundos tras 3 intentos fallidos en un lapso de 60 segundos (`login block-for 120 attempts 3 within 60`).

4. **Hardening de Capa 2 (Switching):**
   * Apagado administrativo (`shutdown`) en rango de todas las interfaces FastEthernet y GigabitEthernet no utilizadas en S1.

---

## 📂 Archivos del Repositorio

* `Dispositivos de red seguros.pkt` — Archivo ejecutable del laboratorio en Cisco Packet Tracer.
* `topologia.pkt` — Archivo png de la topoligía utilizada en el laboratorio.
* `R1-config.txt` — Script completo con los comandos de configuración aplicados en R1.
* `S1-config.txt` — Script completo con los comandos de configuración aplicados en S1.

---

## 🔍 Comandos de Verificación

```cisco
! Verificar estado de las interfaces
show ip interface brief

! Verificar el estado del bloqueo de login e historial de intentos
show login

! Inspeccionar la configuración activa en RAM
show running-config

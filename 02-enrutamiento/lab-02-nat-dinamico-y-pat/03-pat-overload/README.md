# Laboratorio 03: PAT (NAT Sobrecargado)

## Objetivo
Implementar y verificar la configuración de Port Address Translation (PAT), permitiendo que múltiples dispositivos de una red local privada compartan una única dirección IP pública para acceder a redes externas.

## Topología de la Red

![Topología de Red PAT](./Topología.png)

La topología mantiene la estructura de tres enrutadores principales: **SUCURSAL 1**, **MATRIZ** y **SUCURSAL 2**.
*   El router **MATRIZ** actúa como el núcleo de interconexión entre las sucursales.
*   **SUCURSAL 1** proporciona conectividad a la LAN (PC1, PC2) y a un servidor web institucional.
*   **SUCURSAL 2** proporciona conectividad a la LAN (PC3, PC4) y a un servidor FTP.

## Conceptos Clave de Configuración

Para configurar NAT Sobrecargado (PAT), se deben seguir estos pasos técnicos en el router perimetral:
1.  **Identificación de Interfaces:** Identificar las interfaces internas y externas utilizando los comandos `ip nat inside` e `ip nat outside`.
2.  **Lista de Control de Acceso (ACL):** Definir una ACL estándar que permita la traducción de las direcciones locales internas usando el comando `access-list número lista permit red comodin`.
3.  **Traducción Sobrecargada:** Establecer la traducción de direcciones especificando la ACL, la interfaz de salida pública y la opción *overload* mediante el comando `ip nat inside source list número intertace type name overload`.

## Análisis de la Configuración en SUCURSAL 1

El router **SUCURSAL 1** es el responsable de ejecutar el proceso PAT para su red interna:

*   **Interfaces de Traducción:** La interfaz local `GigabitEthernet0/0` se marca con `ip nat inside`, y la interfaz pública `Serial0/0/0` se marca con `ip nat outside`.
*   **ACL configurada:** Se creó la `access-list 10`, la cual permite el tráfico proveniente de la red interna `195.10.1.0` usando la máscara *wildcard* `0.0.0.127`.
*   **Regla PAT:** Se aplica el comando `ip nat inside source list 10 interface Serial0/0/0 overload`. Esto instruye al router a traducir todo el tráfico permitido por la ACL 10 a la única dirección IP pública asignada a la interfaz `Serial0/0/0` (195.10.100.2), diferenciando las sesiones mediante números de puerto dinámicos.

## Archivos de Configuración

Los scripts completos de los routers en Cisco IOS se encuentran en la carpeta `/configs`:
*   [Router SUCURSAL 1](./configs/SUCURSAL1.txt)
*   [Router SUCURSAL 2](./configs/SUCURSAL2.txt)
*   [Router MATRIZ](./configs/MATRIZ.txt)

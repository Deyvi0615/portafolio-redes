# Lab 3: Diseño L3 con VLSM y Enrutamiento de Borde (NAT) - ISP OpticNet

## 🏢 Escenario Técnico
Simulación de la cabecera (Headend) de un ISP de Fibra Óptica (FTTH/GPON). Se tomó el bloque base `192.168.10.0/24` y se aplicó **VLSM** para optimizar el direccionamiento de la infraestructura sin desperdicio de hosts.

## 🧮 Tabla de Direccionamiento Calculada (VLSM)
| Subred / Propósito | Dirección de Red | Rango IP Útil | Dirección de Broadcast | Máscara CIDR |
| :--- | :--- | :--- | :--- | :---: |
| **A** - Clientes Corporativos | `192.168.10.0` | `192.168.10.1` - `192.168.10.62` | `192.168.10.63` | `/26` |
| **B** - Gestión de OLTs | `192.168.10.64` | `192.168.10.65` - `192.168.10.94` | `192.168.10.95` | `/27` |
| **C** - Servidores del NOC | `192.168.10.96` | `192.168.10.97` - `192.168.10.110` | `192.168.10.111` | `/28` |
| **D1** - Enlace Troncal 1 | `192.168.10.112` | `192.168.10.113` - `192.168.10.114` | `192.168.10.115` | `/30` |
| **D2** - Enlace Troncal 2 | `192.168.10.116` | `192.168.10.117` - `192.168.10.118` | `192.168.10.119` | `/30` |

## ⌨️ Configuración de RouterOS v7 (MikroTik Core)
```fib
# Asignación de direccionamiento IP a interfaces lógicas
/ip address add address=192.168.10.1/26 interface=ether2 comment="LAN_Corporativos"
/ip address add address=192.168.10.65/27 interface=ether3 comment="Gestion_OLTs"
/ip address add address=192.168.10.97/28 interface=ether4 comment="Servidores_NOC"

# Configuración de enmascaramiento NAT para salida a Internet (Borde WAN)
/ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade comment="Salida_a_Internet"

Verificación en el NOC

    Las rutas lógicas transicionaron correctamente de estado Inactivo (DIc) a Activo (DAc) en la tabla de enrutamiento al detectar enlace físico simulado de Capa 2 en GNS3.

    Se validó conectividad de extremo a extremo (End-to-End) desde hosts internos (VPCS) hacia el exterior (8.8.8.8) mediante traducción NAT exitosa.

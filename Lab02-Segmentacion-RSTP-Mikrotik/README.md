# Lab 2: Segmentación Corporativa VLAN, Alta Disponibilidad (RSTP) y Loop Protect en MikroTik v7

## 📌 Escenario del Mundo Real
Simulación de la infraestructura LAN de una sede central corporativa que requiere segmentación de tráfico por departamentos (Administración y Servidores) y alta disponibilidad mediante enlaces redundantes en malla.

## 🛠️ Tecnologías y Herramientas
* **Simulador:** GNS3 ejecutado de forma nativa sobre Linux Mint.
* **Core Router:** MikroTik CHR v7.22.1.
* **Access Switches:** Switches genéricos Open vSwitch / L2 de emulación.
* **Hosts:** VPCS (Virtual PC Simulator).

## 📐 Topología Física y Lógica
* **VLAN 10 (Administración):** Subred `192.168.10.0/24` -> Gateway: `192.168.10.1`
* **VLAN 20 (Servidores):** Subred `192.168.20.0/24` -> Gateway: `192.168.20.1`
* **Redundancia:** Enlace troncal doble (bucle físico controlado) entre Switch 1 y Switch 2.

## ⚙️ Configuración Destacada (MikroTik RouterOS v7)

### 1. Creación del Bridge y Activación de RSTP
```mitrokit
/interface bridge add name=BR-LAN protocol-mode=rstp vlan-filtering=no
/interface bridge port add bridge=BR-LAN interface=ether2
/interface bridge port add bridge=BR-LAN interface=ether3

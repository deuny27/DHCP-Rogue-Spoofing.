# DHCP-Rogue-Spoofing.

Este proyecto está basado en una práctica realizada con fines educativos, en la cual se implementó un ataque DHCP Rogue (DHCP Spoofing) utilizando un script desarrollado en Python con la librería Scapy.

El objetivo del laboratorio fue demostrar cómo un atacante puede introducir un servidor DHCP malicioso dentro de una red local, respondiendo a las solicitudes DHCP antes que el servidor legítimo, con el fin de redirigir el tráfico de las víctimas y comprometer la seguridad de la red.

Este proyecto fue desarrollado únicamente en un entorno controlado de laboratorio (PNETLab) con fines académicos.


# Descripción del Ataque

El ataque DHCP Rogue consiste en responder a solicitudes DHCP Discover y DHCP Request antes que el servidor legítimo, ofreciendo configuraciones de red falsas.

Cuando la víctima acepta la configuración enviada por el atacante, puede:

Redirigir su tráfico al gateway malicioso

Usar un DNS controlado por el atacante

Permitir ataques Man-in-the-Middle (MITM)

Comprometer la confidencialidad de la información


# Tecnologías Utilizadas

- Python 3

- Scapy Project

- PNETLab

- Linux

# El servidor DHCP malicioso ofrece:

- Dirección IP falsa

- Gateway controlado por el atacante

- DNS personalizado

- Tiempo de concesión manipulado


# Video del Ataque
https://youtu.be/O-GS53GXcbM

# Topología en PNETLab
<img width="975" height="762" alt="image" src="https://github.com/user-attachments/assets/fd215256-eb14-4bfd-ba4e-afdef03aa1af" />

Topología Utilizada (PNETLab)

# En la topología se utilizaron las siguientes conexiones:

- Atacante (Linux con Scapy)
e0 → SW2 e0/1
e1 → SW1 e0/2

- SW2
e0/1 → Linux e0
e0/0 → SW1 e0/1
e0/2 → VPC1 eth0

- SW1
e0/2 → Linux e1
e0/1 → SW2 e0/0
e0/0 → R1 e0/1
e1/0 → VPC2 eth0

- R1 (Servidor DHCP legítimo)
e0/1 → SW1 e0/0

- VPC1 (Víctima 1)
eth0 → SW2 e0/2

- VPC2 (Víctima 2)
eth0 → SW1 e1/0


# Parámetros del Script

El script permite modificar los siguientes valores para personalizar el comportamiento del servidor DHCP malicioso:

ATTACK_INTERFACE = "eth0"

ROGUE_DNS_SERVER = "8.8.8.8"
ROGUE_SUBNET_MASK = "255.255.255.0"
ROGUE_ROUTER = "192.25.45.254"
ROGUE_DOMAIN = "local.lan"
LEASE_TIME = 86400

ROGUE_SERVER_IP = "192.25.45.254"

IP_POOL_START = "192.25.45.150"
IP_POOL_END = "192.25.45.200"

# Funcionamiento del Script

- El script realiza las siguientes acciones:

Escucha tráfico DHCP en los puertos UDP 67 y 68.

Detecta mensajes:

DHCP Discover

DHCP Request

- Genera una dirección IP dentro del pool configurado.

- Construye un paquete DHCPOFFER con:

IP ofrecida

Gateway falso

DNS personalizado

Dominio

Lease time

- Envía la respuesta a la MAC de la víctima.

- Si la víctima acepta la oferta, su tráfico será redirigido al atacante.

# Medidas de Mitigación

- Para prevenir ataques DHCP Rogue se recomienda:

- Activar DHCP Snooping en switches gestionables

- Marcar puertos confiables

- Limitar servidores DHCP autorizados

- Implementar Port Security

- Segmentar VLANs

- Monitorear tráfico DHCP sospechoso

- Implementar IDS/IPS

## Video demostrativo

Pendiente de grabación y publicación en YouTube. El enlace se colocará aquí antes de la entrega.

# Laboratorio A3 · DHCP y monitoreo SNMP con Zabbix

Adrian Magallanes · Matrícula 2025-2139 · GNS3 · Ubuntu Desktop 24.04.5 LTS · Zabbix 7.0

## Propósito

Asignar por DHCP la dirección del servidor Zabbix y comprobar desde su panel los datos SNMP del router y del switch. La comunidad SNMP tiene permiso de solo lectura y una ACL limita las consultas a la IP del servidor.

## Topología

![Topología del laboratorio en GNS3](evidencias/topologia-gns3.png)

![Diagrama lógico del laboratorio](docs/topologia.svg)

| Equipo | Dirección | Función |
|---|---|---|
| R1 · Cisco c7200 | 10.21.39.129/29 | Puerta de enlace, servidor DHCP y agente SNMP |
| Switch · Cisco IOSvL2 | 10.21.39.130/29 en Vlan1 | Conmutación LAN y agente SNMP |
| PC-USUARIOS-1 · Ubuntu Desktop | 10.21.39.131/29 por DHCP | Servidor Zabbix y gestor SNMP |

La red 10.21.39.128/29 usa los octetos 21 y 39 derivados de la matrícula. R1 conecta Fa0/0 con Gi0/0 del switch. Ubuntu Desktop se conecta al switch y recibe la única dirección disponible del pool DHCP. El enlace R1–Switch quedó configurado a 100 Mb/s y full-duplex en ambos extremos.

## Configuración y resultados

- R1 excluye del pool 10.21.39.129–130 y 10.21.39.132–134. La concesión automática para Ubuntu es 10.21.39.131. [Ver concesión en R1](evidencias/dhcp-r1.png) y [ver IP y ping desde Ubuntu](evidencias/ubuntu-dhcp.png).
- R1 y Switch tienen la ACL 31 con permiso para 10.21.39.131 y una comunidad SNMP RO 31. La comunidad se oculta en las copias publicadas de los running-configs.
- Ubuntu consultó el OID sysName.0 por SNMPv2c y recibió R1 y Switch. [Ver la consulta](evidencias/snmp-ubuntu.png).
- Zabbix utiliza la plantilla Cisco IOS by SNMP para ambos hosts. [Datos recientes de R1](evidencias/zabbix-r1.png) y [datos recientes de Switch](evidencias/zabbix-switch.png) muestran métricas obtenidas por SNMP.

## Archivos

- [Procedimiento y pruebas](docs/VERIFICACION.md)
- [Configuraciones IOS aplicadas](scripts/README.md)
- [Running-configs con comunidad oculta](configs/README.md)
- [Índice de evidencias](evidencias/README.md)
- [Archivo TXT de entrega](AdrianMagallanes_2025-2139_A3.txt)

La prueba desde una IP no autorizada y el video individual quedan pendientes. El TXT se completará con el enlace de YouTube antes de la entrega.


# Implementación y verificación

## Direccionamiento

| Equipo | IP | Evidencia |
|---|---|---|
| R1 Fa0/0 | 10.21.39.129/29 | Running-config de R1 |
| Switch Vlan1 | 10.21.39.130/29 | Running-config de Switch |
| Ubuntu Desktop ens33 | 10.21.39.131/29 por DHCP | Capturas de R1 y Ubuntu |

El pool LAN-ZABBIX deja disponible 10.21.39.131. La puerta de enlace entregada es 10.21.39.129. La red 10.21.39.128/29 usa los octetos 21 y 39 de la matrícula 2025-2139.

## Preparación de Zabbix

Ubuntu Desktop 24.04.5 LTS recibió la concesión por DHCP. Para instalar Zabbix 7.0 se conectó temporalmente la máquina a NAT, se descargó el repositorio oficial y se instalaron MySQL, Zabbix Server, el frontend Apache y las utilidades SNMP. Luego volvió a la LAN de GNS3. La base zabbix se creó en MySQL, se importó el esquema inicial y se configuró la conexión en zabbix_server.conf. La contraseña de MySQL se mantiene fuera de este repositorio.

El frontend se abrió en http://localhost/zabbix. Se crearon los hosts R1 y Switch con interfaz SNMPv2c en UDP 161, plantilla Cisco IOS by SNMP y la macro de comunidad como texto secreto.

## Pruebas realizadas

1. En R1, show ip dhcp binding mostró la concesión 10.21.39.131. En Ubuntu, ip -4 -br addr mostró esa misma dirección en ens33.
2. Ubuntu respondió al ping de 10.21.39.129 y 10.21.39.130.
3. Desde Ubuntu, snmpget al OID .1.3.6.1.2.1.1.5.0 devolvió R1 y Switch.
4. Las ACL 31 de ambos equipos permitieron 10.21.39.131 y registraron coincidencias. Ambos running-configs muestran SNMP RO 31.
5. Monitoring → Latest data de Zabbix mostró valores recientes de R1 y Switch. En el switch se observaron métricas de Gi0/0, incluido estado operativo up y tráfico.
6. El enlace R1 Fa0/0 ↔ Switch Gi0/0 quedó a 100 Mb/s y full-duplex en ambos extremos. Las configuraciones se guardaron en startup-config.

La prueba de rechazo SNMP desde una segunda IP no se ha realizado. La restricción se documenta mediante las ACL y las consultas exitosas desde 10.21.39.131.

## Video individual

El video debe durar como máximo 10 minutos y mostrar fecha y hora, rostro y voz del autor. La demostración puede recorrer la topología, la concesión DHCP, las ACL y los datos recientes de ambos hosts en Zabbix. Tras subirlo a YouTube, colocar el enlace al principio del README y en el TXT de entrega.

## Referencias

- [Zabbix · Monitorear un switch o router](https://www.zabbix.com/documentation/current/en/manual/guides/monitor_switch)
- [Cisco · SNMP y ACL en IOS](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)


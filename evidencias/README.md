# Evidencias del laboratorio A3

## Topología y direccionamiento

El proyecto GNS3 conecta R1 con Switch y Ubuntu Desktop con el switch.

![Topología en GNS3](topologia-gns3.png)

Ubuntu recibió 10.21.39.131/29 y respondió al ping de R1. R1 registró la concesión DHCP.

![IP de Ubuntu obtenida por DHCP](ubuntu-dhcp.png)

![Concesión DHCP en R1](dhcp-r1.png)

La ruta predeterminada de Ubuntu se registró con `proto dhcp`. La captura corresponde al momento previo a la conexión temporal NAT para instalar Zabbix; por eso también muestra un fallo de resolución DNS externa.

![Ruta de Ubuntu recibida por DHCP](ubuntu-ruta-dhcp.png)

## SNMP y control de acceso

La ACL 31 de R1 permite consultas desde 10.21.39.131. La configuración final del switch contiene la misma ACL. Las consultas SNMPv2c desde Ubuntu devolvieron R1 y Switch.

![ACL 31 de R1](acl-r1.png)

La ACL del switch también registró coincidencias. El aviso de dúplex al pie de esta captura es anterior a la corrección final del enlace, demostrada más abajo.

![ACL 31 de Switch antes del ajuste final](acl-switch-previo-ajuste.png)

![Respuestas SNMP desde Ubuntu](snmp-ubuntu.png)

## Hosts y datos en Zabbix

Ambos hosts tienen una interfaz SNMPv2 en UDP 161 y la plantilla Cisco IOS by SNMP. La comunidad se asignó mediante una macro cuyo valor se ocultó en Zabbix.

Latest data mostró valores recientes de los dos equipos. En Switch se observó tráfico y el estado operativo de Gi0/0.

![Datos recientes de R1](zabbix-r1.png)

![Datos recientes de Switch](zabbix-switch.png)

## Enlace entre R1 y Switch

Las dos interfaces quedaron activas a 100 Mb/s y full-duplex.

El ítem de velocidad mostrado por Zabbix para Gi0/0 indica 1 Gbps en su captura. La diferencia frente a la consola Cisco no se atribuye a una causa no verificada; para el estado operativo del enlace se presentan las salidas directas de ambos equipos.

![Interfaz Fa0/0 de R1](enlace-r1-full-duplex.png)

![Interfaz Gi0/0 de Switch](enlace-switch-full-duplex.png)

La prueba SNMP desde otra IP no se realizó. La comunidad y la contraseña de MySQL no aparecen en estas capturas.

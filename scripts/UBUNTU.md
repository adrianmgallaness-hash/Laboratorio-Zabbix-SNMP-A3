# Comandos manuales de verificación en Ubuntu

La instalación de Zabbix se realizó mediante comandos interactivos, sin un script de instalación. Esta página registra los comandos de verificación mostrados durante el laboratorio. La contraseña de MySQL y la comunidad SNMP no se publican.

```bash
lsb_release -ds
ip -4 -br addr
ip -4 route
ping -c 3 10.21.39.129
ping -c 3 10.21.39.130
systemctl is-active zabbix-server apache2
```

La ruta predeterminada desde la LAN mostraba `proto dhcp`, con origen `10.21.39.131`. R1 registró la misma IP en `show ip dhcp binding`.

Para repetir las consultas de solo lectura sin exponer la comunidad en la terminal:

```bash
read -rsp 'Comunidad SNMP: ' comunidad
echo
snmpget -v2c -t 2 -r 1 -c "$comunidad" 10.21.39.129 .1.3.6.1.2.1.1.5.0
snmpget -v2c -t 2 -r 1 -c "$comunidad" 10.21.39.130 .1.3.6.1.2.1.1.5.0
unset comunidad
```

El OID consultado es `sysName.0`; las respuestas observadas fueron `R1` y `Switch`.

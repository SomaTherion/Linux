Creación de los Raids:
Para la creación de los RAIDS deberemos disponer en nuestro equipo (en nuestra máquina virtual en este ejemplo) el número mínimo de discos duros necesarios para cada tipo de RAID. 


Para ello nos iremos a la configuración de nuestra mñaquina virtual, almacenamiento y en los controladores SATA o SCASI añadiremos una “unidad de disco”. Seguimos los pasos del asistente para configurar el tamaño y tipo de disco. Repetimos el proceso con tantas unidades como necesitemos.

 
Server (RAID 1)
Requiere mínimo 2 HDD

Indicaremos en la configuración que son para tipo RAID e indicaremos el tipo.

La selección de los discos se hace mediante la tecla espaciadora.


 
Server DNS (RAID 0)
Requiere mínimo 2HDD

Se hace exactamente igual que la anterior, simplemente esta vez escogeremos que es RAID1.


 
Server Apache (RAID 5)
Requiere mínimo 3HDD

Como las anteriores, pero en esta ocasión necesitaremos disponer de un tercer disco. Esta vez seleccionamos RAID5.




Seguiremos los pasos que nos indica el instalador. 
Instalar servidor DNS(Bind9)
Bind9

Instalacion:

apt-get update para actualizar los respositorios

Ejecutamos apt-get install bind9:

Configuración servidor de DNS
Ahora vamos a configurar los archivos de configuración de bind9

El primero el named.conf.local

A continuación crearemos los archivos que hemos definido en named.conf.local. Usaremos el archivo db.empty como plantilla.

Hacemos lo correspondiente con los otros archivos.
Configuramos el fichero de resolución inversa. Usaremos como plantilla db.127.

Después configuraremos que en caso de que el servidor no conozca el nombre de dominio le pregunte a otro servidor.


Lo que haremos es modificar donde pone “forwarders” poner la IP o Ips  de los DNS a quien preguntaremos. En el ejemplo hemos puesto los de Google.

Cuidado de descomentar la línea (quitar la //), si no lo hacemos no funcionará.

Una vez listo reiniciaremos bind9
	
sudo /etc/init.d/bind9 restart

Para comprobar si tiene fallos podemos usar estos comandos
	named-checkconf
	named-checkzone sitioa /etc/bind/rd.sitioa.com

Recuerda que hemos establecido una IP en los ficheros, por lo que deberemos tener una IP estática en lugar de dinámica en la configuración de nuestro servidor. Para ello vamos a editar el fichero correspondiente.


Vamos a configurar también el reenvío de paquetes. Esto se hace en:


Descomentando la siguiente linea.

Ahora deberemos configurar la tarjeta de red en nuestro servidor web y/o ftp para que use nuestro DNS. Si el servidor fuese el mismo no sería necesario.

Ahora comprobamos desde otro equipo que las peticiones de nombres las resuelve correctamente. En ese equipo deberemos configurar como DNS la IP del servidor DNS que hemos creado.




Instalando el servidor FTP
Instalación del servicio:
apt-get update para actualizar los repositorios

En nuestro caso vamos a instalar VSFTPD.

Para ello ejecutamos apt-get install vsftpd

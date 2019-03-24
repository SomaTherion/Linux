Creación de los Raids:
Para la creación de los RAIDS deberemos disponer en nuestro equipo (en nuestra máquina virtual en este ejemplo) el número mínimo de discos duros necesarios para cada tipo de RAID. 
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/11.png)

Para ello nos iremos a la configuración de nuestra mñaquina virtual, almacenamiento y en los controladores SATA o SCASI añadiremos una “unidad de disco”. Seguimos los pasos del asistente para configurar el tamaño y tipo de disco. Repetimos el proceso con tantas unidades como necesitemos.

 
Server (RAID 1)
Requiere mínimo 2 HDD
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/12.png)
Indicaremos en la configuración que son para tipo RAID e indicaremos el tipo.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/13.png)
La selección de los discos se hace mediante la tecla espaciadora.


 
Server DNS (RAID 0)
Requiere mínimo 2HDD
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/14.png)
Se hace exactamente igual que la anterior, simplemente esta vez escogeremos que es RAID1.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/15.png)

 
Server Apache (RAID 5)
Requiere mínimo 3HDD

Como las anteriores, pero en esta ocasión necesitaremos disponer de un tercer disco. Esta vez seleccionamos RAID5.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/16.png)

![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/17.png)

Seguiremos los pasos que nos indica el instalador. 
Instalar servidor DNS(Bind9)
Bind9

Instalacion:

apt-get update para actualizar los respositorios

Ejecutamos apt-get install bind9:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/18.png)
Configuración servidor de DNS
Ahora vamos a configurar los archivos de configuración de bind9

El primero el named.conf.local
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/19.png)
A continuación crearemos los archivos que hemos definido en named.conf.local. Usaremos el archivo db.empty como plantilla.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/20.png)
Hacemos lo correspondiente con los otros archivos.
Configuramos el fichero de resolución inversa. Usaremos como plantilla db.127.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/21.png)
Después configuraremos que en caso de que el servidor no conozca el nombre de dominio le pregunte a otro servidor.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/22.png)

Lo que haremos es modificar donde pone “forwarders” poner la IP o Ips  de los DNS a quien preguntaremos. En el ejemplo hemos puesto los de Google.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/23.png)
Cuidado de descomentar la línea (quitar la //), si no lo hacemos no funcionará.

Una vez listo reiniciaremos bind9
	
sudo /etc/init.d/bind9 restart

Para comprobar si tiene fallos podemos usar estos comandos
	named-checkconf
	named-checkzone sitioa /etc/bind/rd.sitioa.com

Recuerda que hemos establecido una IP en los ficheros, por lo que deberemos tener una IP estática en lugar de dinámica en la configuración de nuestro servidor. Para ello vamos a editar el fichero correspondiente.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/24.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/25.png)

Vamos a configurar también el reenvío de paquetes. Esto se hace en:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/26.png)

Descomentando la siguiente linea.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/27.png)
Ahora deberemos configurar la tarjeta de red en nuestro servidor web y/o ftp para que use nuestro DNS. Si el servidor fuese el mismo no sería necesario.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/28.png)
Ahora comprobamos desde otro equipo que las peticiones de nombres las resuelve correctamente. En ese equipo deberemos configurar como DNS la IP del servidor DNS que hemos creado.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/29.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/30.png)



Instalando el servidor FTP
Instalación del servicio:
apt-get update para actualizar los repositorios

En nuestro caso vamos a instalar VSFTPD.

Para ello ejecutamos apt-get install vsftpd
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/31.png)

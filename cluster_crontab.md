-- CREACION Y CONFIGURACION DE UN CLUSTER --

Vamos a crearnos una maquina virtual que será nuestro clustar, así cómo los distintos nodos.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/32.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/33.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/34.png)

Configuramos la red de nuestra maquina virtual para que sea tipo puente y así puedan "verse" entre ellas.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/35.png)

Configuramos el arranque de los nodos para que inicien desde tarjeta de red.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/36.png)

Lanzamos nuestra maquina cluster.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/37.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/38.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/39.png)

Cuando el sistema operativo este cargado y funcionando, lanzamos los nodos. si están bien configurados para que arranquen desde red recibiran la información necesaria del cluster.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/40.png)

Podemos ver cómo en el cluster nos aparecen 3 CPU en total (cluster+2 nodos).
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/41.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/42.png)

-- FSTAB --

A una máquina virtual de linux añadirle dos discos duros:
Comprobamos la etiqueta de nuestros discos nuevos
```fdisk -l```
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/43.png)

Disco A

Accedemos al disco duro que se desea crear la particiones:
```sudo fdisk /dev/sdb```
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/44.png)
  Crear las siguientes particiones:
    creamos la tabla de particiones pulsando la ```o```
Linux   
    - Pulsaremos la p para indicar particion primaria
    - Seleccionamos la primera particion con el 1
    - Pulsamos intro para que coja automaticamente el sector de inicio
    - Introducimos hasta que sector quiere la particion
    
 ![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/46.png) 

Fat
    - Creamos otra particion con la tecla n    
    - Pulsaremos la p para indicar particion primaria    
    - Seleccionamos la segunda particion con el 2    
    - Pulsamos intro para que coja automaticamente el sector de inicio    
    - Pulsamos intro para que coja lo que queda deL disco
  
  ![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/47.png)
  
  Para cambiar el tipo de la 2 particion a FAT32 para ello pulsamos ```t```
  Seleccionamos la particion 2
  Podemos listar los tipos de particiones que hay con la tecla L
  ![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/49.png)
  
  Para fat32 introducimos ```b```
  
  Guardamos y escribimos los datos presionando ```w```
  
  Para completar el formato de la primera particion introducimos:
  ```sudo mkfs.ext4 /dev/sdb1```
  
  Para completar el formato de la segunda particion introducimos:
  ```sudo mkfs.fat /dev/sdb2```
  
    ![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/50.png)
    
Disco B

Repetimos el proceso anterior, ahora trabajando sobre el disco sdc.

    ```sudo fdisk /dev/sdc```
    
Crear las siguientes particiones:
Una vez creada la tabla de particiones crearemos las particiones con la tecla n, igual que en el disco anterior.
Esta vez añadiremos una partición mas.
Linux
Por defecto la partición se crea en este sistema de archivos, por lo que está no será necesario modificarla.

NTFS
Para cambiar el tipo de la 2 particion a NTFS pulsamos 
```t```
Seleccionamos la particion 
```2```
Podemos mostrar todos los tipos de particiones que hay con la tecla 
```L```
En el caso de escoger NTFS introducimos 
```7```

Fat
Seleccionamos la particion 
```3```
En el caso de escoger fat32 introducimos 
```b```

Guardamos y escribimos los datos presionando 
```w```
Para completar el formato de la primera particion introducimos:

```sudo mkfs.ext4 /dev/sdc1```

Para completar el formato de la segunda particion introducimos:

```sudo mkfs.ntfs /dev/sdc2```

Para completar el formato de la tercera particion introducimos:

```sudo mkfs.fat /dev/sdc3```

Modificar el fichero fstab, añadiendo todas las particiones. Las del disco A se montarán manualmente. Las del disco B se montarán automáticamente al arrancar.

Editamos el fichero fstab
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/51.png) 

Lo configuramos con los valores acordes a nuestros discos duros.

```# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/ubuntu--vg-root /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda1 during installation
UUID=c29a4022-e6c4-429c-84e5-243099e9b66f /boot           ext2    defaults        0       2
/dev/mapper/ubuntu--vg-swap_1 none            swap    sw              0       0
#Disco 2
UUID=c3e52e7b-dd1a-45d5-bbef-492582f2cedc       /media/sdb1     ext4    defaults,noauto         0       0
UUID=9BED-B5F0  /media/sdb2     vfat    defaults,noauto 0       0

#Disco 3
/dev/sdc1       /media/sdc1     ext4    defaults        0       0
/dev/sdc2       /media/sdc2     ntfs    defaults        0       0
/dev/sdc3       /media/sdc3     vfat    defaults        0       0

```
Para montar automaticamente las particiones podemos reiniciar ```sudo reboot``` o con este comando ```sudo mount -a```.

Para montar manualmente usaremos el siguiente comando ```sudo mount /dev/sdb1 /media/sdb1``` y para la segunda particion sudo mount ```/dev/sdb2 /media/sdb2```.

-- EJERCICIOS CRON --


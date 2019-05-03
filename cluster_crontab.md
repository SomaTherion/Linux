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
disco A
Accedemos al disco duro que se desa crear la particiones:
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
    
disco B
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

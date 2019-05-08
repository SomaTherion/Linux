Escriba un script que elimine un archivo o directorio pasado como parámetro, y le pregunte si está seguro de llevar a cabo la acción.

```
#! /bin/bash

if [ -e $1]; then
	read -p "Borrar archivo o directorio (s/n)" op
	if [ "$op" = "s"] || [ "$op" = S]; then
		rm -rf $1
		exit 0
	else
		exit 1
	fi
else
	echo "El archivo o directorio no existe"
fi
exit
```

Escribir un script que pueda mostrar información de un comando al ejecutar dicho script y pasar como parámetro el comando.

```
#!/bin/bash

if [ $# -lt 1 ]; then
 	echo "No se ha introducido ningun parametro"
 	exit 1
else
	man $1
fi
exit
```

Realiza un script que compruebe si el usuario actual del sistema es blas, si es así visualiza su nombre 5 veces, sino te despides de él amigablemente.

```
#!/bin/bash

usuario=`whoami`

if [ $usuario == 'blas' ]; then
 	for i in {0..4}
 	do
 		echo "$usuario"
 	done
 	exit 0
else
	echo "Adios"
fi
exit
```

En un fichero tengo una palabra clave. Haz un script que muestre si dicha palabra es el parámetro pasado o no.

```
#!/bin/bash

palabra=`cat ./palabra.txt`

if [ $palabra == $1 ]; then
	echo "Palabra correcta"
else
	echo "Palabra incorrecta"
fi
exit
```

Tenemos un menu principal: (1) Suma Lee dos números y los suma. (2) Resta Lee dos números y los resta. (3) Multiplicación Lee dos números y los multiplica. (4) Salir Termina el script.

```
#! /bin/bash

echo "1: Suma"
echo "2: Resta"
echo "3: Multiplica"
echo "4: Salir"
x=1
until [ $x ==4 ]; do
	case $x in
	1)
		read -p "Introduce un numero" a
		read -p "introduce un numero" b
		echo $((a+b))
	2)
		read -p "Introduce un numero" a
		read -p "introduce un numero" b
		echo $((a-b))
	3)
		read -p "Introduce un numero" a
		read -p "introduce un numero" b
		echo $((a*b))
	4)
		exit 0
	esac
done
exit
```
Nos pide la edad y nos dice si es mayor de edad o menor.

```
#! /bin/bash

read -p "Introduce tu edad: " edad

if [ $edad -lt 18 ]; then
	echo "Eres menor de edad"

elif [ $edad -gt 17 ]; then
	echo "Eres mayor de edad"
fi
exit
```
Script que reciba un nombre de fichero, verifique que existe y que es un fichero de lectura-escritura, lo convierta en ejecutable para el usuario y el grupo y muestre el estado final de los permisos.

```
#! /bin/bash

if [ -f $1 ]; then
	echo "Es un fichero"
	ls -l $1
    if [ -r $1 ];then
    echo "Permiso de lectura"
        if [ -w $1 ];then
        	echo "Permisos de escritura "
        	chmod ug+x $1
        	ls -l $1
        else
        	echo "No es un fichero"
        fi
    else
    	echo "No es un fichero"
    fi
else
	echo "El fichero no existe "
fi
exit
```

Script que nos diga al pulsar una tecla, si es letra, numero o caracter especial.

```
#! /bin/bash

read -p "Introduce algo " x

case $x in
	[a-z,A-Z]) 
		echo "Es una letra"
	;;
	[0-9]) 
		echo "Es un numero"
	;;
	*) 
		echo "Es un caracter especial"
	;;
esac
exit
```

Realizar un script que reciba varios parametros y nos diga cuantos de esos parametros son de directorios y cuantos son archivos. $# contador que indica cuantos parametros se pasan.

```
#! /bin/bash

a=0
b=0

for i in $*;
do
   if [ -d $i ]; then
		a=$(($a+1))
   elif [ -f $i ]; then
		b=$(($b+1))
   else
		echo "$i no es fichero ni directorio"
   fi
done

echo "Se han introducido $a directorios y $b ficheros"
echo "Parametros introducidos $#"

exit
```
Mostramos menu, con productos para vender, luego nos pide que introduzcamos la opcion. luego mensaje que indica que introduzca moneda. Si ponemos precio exacto nos da mensaje, "Gracias buen provecho", si ponemos menos, nos diga falta. Si poner mas valor, nos indique el cambio con mensaje.

```
#! /bin/bash

echo" Tienda"
echo "1. Paquete de folios, 5 euros"
echo "2. Disco de musica, 15 euros"
echo "3. Peluche 4 euros"

read -p "Introduzca opcion:" x
read -p " Introduzca dinero: " dinero

case $op in
	1)
		precio=5
	;;
	2)
		precio=15
	;;
	3)
		precio=4
	;;
	*)
		echo "Opcion incorrecta"
	;;
esac

while [ $dinero -lt $precio ];
do
	faltaDinero=$(($precio-$dinero))
	read -p " Faltan $faltaDinero euros" extra
	dinero = $(($dinero+$extra))
done

if [ $din -gt $precio ]; then
	cambio=$(($dinero - $precio))
	echo "Su cambio es $cambio euros"
	echo "Gracias por su compra"
fi
exit
```

Realizar un script que pida introducir la ruta de un directorio por teclado (Hay que validar que la variable introducida sea un directorio) nos diga cuantos archivos y cuantos directorios hay dentro de ese directorio.

```
#! /bin/bash

read -p "Introduzca la ruta de el directorio :" direc
until [ -d $direc ]; do
	read -p "Introduzca la ruta del directorio :" direc
done

contadorDir=0
contadorFichero=0

for x in `ls $direc`; 
	do
            if [ -d $x ]; then
            	contadorDir=$(($contadorDir+1))
            elif [ -f $x ]; then
            	contadorFichero=$(($contadorFichero+1))
            fi
     done
echo "Hay $contadorDir directorios y $contadorFichero ficheros"
exit
```

Realiza un script que introduzca número por parámetro y muestre tabla de multiplicar.

```
#! /bin/bash

for x in {0..10};
do
	echo "$1 x $x: " $(($1*$x))
done
exit
```

Script que limpie todas las reglas, y de permiso a todas las conexiones.

```
#! /bin/bash

`iptables -F`
`iptables -I INPUT -j ACCEPT`
`iptables -I OUTPUT -j ACCEPT`

exit
```
Script que limpie todas las reglas, y prohíba cualquier conexión.

```
#! /bin/bash

`iptables -F`
`iptables -I INPUT -j REJECT`
`iptables -I OUTPUT -j REJECT`

exit
```

Tendrá 3 parámetros: red(ip), entrada-salida, aceptar-denegar. Dará estos permisos a iptables.

```
#! /bin/bash

`iptables -A $2 -p tcp -s $1 -j $3`

`service iptables restart`

exit
```

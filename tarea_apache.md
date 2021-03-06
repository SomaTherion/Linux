Tarea APACHE2

Instalacion:
-apt-get update para actualizar los respositorios

-Ejecutamos apt-get install apache2:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/1.png)

El archivo .conf con la configuración, una vez instalado, se encuentra en “/etc/apache2/apache2.conf”. Es el que editaremos para modificar los valores de configuracion. 

Nos encontraremos con algo así:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/2.png)
Las líneas que tienen una almohadilla delante son comentarios o comandos “anulados”.

Creación de dominios:

Una vez instalado el servicio debemos crear los hosts virtuales para cada página. La carpeta que usa apache está en “var/www”.

sudo mkdir -p /var/www/gato.com/html
sudo mkdir -p /var/www/mosquito.com/html
sudo mkdir -p /var/www/escheriacholi.es/html
sudo mkdir -p /var/www/chip555.org/html

Para dar la propiedad de las carpetas a Apache usaremos el comando chown de la siguiente manera:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/3.png)
El contenido ahora pertenece al usuario y al grupo www-data.

La carpeta por defecto donde apache2 trabaja con sus dominios está ubicada en “/var/www/html/”.

Ahora creamos las páginas web de cada host con su index.html correspondiente.

Una vez tenemos nuestras páginas web, lo que haremos es crear sus correspondientes archivos de configuracion en Apache2. Para ello usaremos el archivo 000-default.conf como plantilla.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/4.png)

Debemos crear archivos de configuración para cada uno de los host virtuales
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/gato.com.conf

Repetiremos con el resto de páginas, asegurándonos de dar a cada una su nombre. Una vez hecho pasaremos a configurarlos editándolos.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/5.png)
gato.com.conf
<VirtualHost *:80>
        ServerAdmin admin@gato.com
        ServerName gato.com
        ServerAlias www.gato.com
        DocumentRoot /var/www/gato.com/html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

mosquito.com.conf
<VirtualHost *:80>
    ServerAdmin admin@mosquito.com
    ServerName mosquito.com
    ServerAlias www.mosquito.com
    DocumentRoot /var/www/mosquito.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

escheriacoli.es.conf
<VirtualHost *:80>
    ServerAdmin admin@escheriacoli.es
    ServerName escheriacoli.es
    ServerAlias www.escheriacoli.es
    DocumentRoot /var/www/escheriacoli.es/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<Directory /var/www/escheriacoli.es/html>
        AuthType Basic
        AuthName "ACCESO RESTRINGIDO."
        AuthUserFile /var/www/escheriacoli.es/.htpasswd
        Require user usuario1
</Directory>
<Directory /var/www/escheriacoli.es/html>        
        Options Indexes FollowSymLinks MultiViews
        AllowOverride  none
        Order Allow,deny
        allow from all
</Directory>

chip555.org.conf
<VirtualHost *:80>
    ServerAdmin admin@chip555.org
    ServerName chip555.org
    ServerAlias www.chip555.org
    DocumentRoot /var/www/chip555.org/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<Directory /var/www/chip555.org/html>
        AuthType Basic
        AuthName "ACCESO RESTRINGIDO."
        AuthUserFile /var/www/chip555.org/.htpasswd
        Require valid-user
</Directory>
<Directory /var/www/chip555.org/html>        
        Options Indexes FollowSymLinks MultiViews
        AllowOverride  none
        Order Allow,deny
        allow from all
</Directory>

En el caso de los hosts escheriacoli.es y chip555.org nos piden de cumplir unas condiciones de acceso. Para ello debemos hacer uso de un archivo password.

Crearemos este fichero en la carpeta correspondiente a cada dominio.

En el caso de chip555.org es un usuario especifico llamado user01, por lo que lo añadimos al crear el fichero.
	sudo htpasswd -c /var/www/chip555.org/.htpasswd user01

Para eschearicoli:

	sudo htpasswd -c /var/www/escheriacoli.es/.htpasswd

Si vemos los archivos de configuración que pusimos anteriormente vemos que en los correspondientes a estos dos dominios hemos definido una linea de acceso para usuarios que apunta a la ruta con ese fichero.

El ponerle un punto (.) delante es para que esté oculto. 

htpasswd ejecuta la orden, -c indica que es una creación de archivo nueva y no una edición, a continuación viene la ruta donde vamos a crear el fichero (/var/www/privado/) seguido del nombre del archivo (.htpasswd) y por último el nombre de usuario. Se nos pedirá una contraseña para el usuario y que la confirmemos, como en este ejemplo:
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/6.png)
Si pusiéramos la misma orden con la misma dirección pero sin “-c” lo que haríamos sería añadir usuarios a la lista.

Supongamos que queremos hacer, en lugar de un dominio privado, una carpeta privada dentro de un dominio público. Para ello el proceso es idéntico solo que el archivo “.htaccess” lo añadiremos en la carpeta que queremos hacer privada.

Habilitar dominios:
Por último, una vez tengamos hechos los archivos .conf de nuestros dominios, los pondremos disponibles. Esto se hace con la orden  “a2ensite nombre_dominio.conf”, donde “nombre_dominio” es el nombre que hemos puesto al archivo de cada dominio.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/7.png)
Una vez hecho con todos nuestros dominios, reiniciamos el servicio.
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/8.png)
Ya estarían en principio funcionando, recuerda configurar el servidor DNS para que la resolución de nombres sea correcta.
Mostraremos un par de ejemplos con la web de www.gato.com

![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/9.png)
![Texto alternativo](https://github.com/SomaTherion/Linux/blob/master/10.png)

Si intentamos entrar a la web con usuario nos pedirá hacer login con el usuario que hemos definido en el .htpasswd.

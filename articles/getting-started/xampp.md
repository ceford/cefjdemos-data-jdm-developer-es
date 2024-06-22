<!-- Filename: XAMPP / Display title: Configuración de un servidor XAMPP para desarrollo de Joomla! -->

## Introducción

XAMPP es un paquete fácil-de-instalar que incluye: un servidor web
Apache, PHP, XDEBUG y la base de datos MySql. Esto permite que puedas
crear el ambiente que necesitas para ejecutar Joomla! en tu equipo
local. La última versión de XAMPP está disponible en
<a href="http://www.apachefriends.org/es/index.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">el sitio web de XAMPP</a>. Las
descargas están disponibles para Linux, Windows, Mac OS X y Solaris.
Descarga el paquete para tu plataforma.

"Nota importante Sobre XAMPP y Skype:" Apache y Skype ambos utilizan el
puerto 80 como una alternativa para las conexiones entrantes. Si usas
Skype, ingresa en Herramientas-Opciones-Avanzadas-Panel de conexión y
anula la selección de la opción "Utilizar los puertos 80 y 443 como
alternativas para las conexiones entrantes". Si Apache se inicia como un
servicio, tomará el puerto 80 antes que Skype se inicie y no se vera
como un problema. Pero, para estar seguro, deshabilita la opción de
Skype.

### Instalación en Windows

Instalación para Windows es muy sencilla. Puedes utilizar el instalador
ejecutable de XAMPP (por ejemplo, "xampp-win32-1.7.3-installer.exe").
Instrucciones detalladas de instalación para Windows están disponibles
<a href="http://www.apachefriends.org/es/faq_windows.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">aquí</a>.

If you are on Windows XP or 2003 they are not supported by the main
package but there are compatible versions of XAMPP for these platforms
listed on the download page (but you will only be able to run PHP 5.4 or
lower - and therefore will only be able to test Joomla 3.x and lower).

Para Windows, se recomienda instalar XAMPP en "c:\xampp" (no en
"c:\archivos de programas"). Si haces esto, tu Joomla! (y cualquier otra
carpetas de un sitio web local) ira a la carpeta "c:\xampp\htdocs". (Por
convención, todos los contenido de la web pasa por debajo de la carpeta
"htdocs".)

Si tienes varios servidores http (como IIS) puede cambiar el puerto de
escucha de xampp. En \apache\conf\httpd.conf, modificar la línea Listen
80 para Escuchar \[portnumber\] (ej: "Listen 8080").

Joomla Community Magazine Tutorial

You can find a detailed tutorial on installing XAMPP on Windows, along
with the Joomla 4 Beta, the Joomla Patch Tester and Git in this <a
href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git"
class="external text" target="_blank" rel="noreferrer noopener">Joomla
Community Magazine article</a>.

### Instalación en Linux

#### Instalar XAMPP

Abre una Terminal y escribe:

    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt

(reemplace "xampp-linux-1.7.7.tar.gz" con la versión de xammp que has
descargado). Se ha informado que la base de datos MYSQL de xampp 1.7.4
no funciona con Joomla 1.5.22

De esta forma se instala ... Apache2, mysql y php5, así como un servidor
ftp.

    sudo /opt/lampp/lampp start

y

    sudo /opt/lampp/lampp stop

inicia/detiene todos los servicios

#### Prueba tu servidor localhost XAMPP

Abre tu Navegador y apunta a

    http://localhost

El index.php se redirigirá a

    http://localhost/xampp

Allí encontrará las instrucciones sobre cómo cambiar los nombres
predeterminados de usuarios/contraseñas. En un PC que no sirve de
archivos a través de Internet o LAN cambiar los valores predeterminados
es una decisión personal.

#### Obtener Joomla!

Descarga la última instalación zip de Joomla!
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Descomprime el archivo en el disco duro

Conecta al localhost con el cliente de FTP Predeterminado

    nobody
    lampp

Crea una carpeta para tu Joomla! en el servidor localhost

Copia por FTP los archivo desempaquetados de instalación de Joomla! a la
nueva carpeta Joomla!.

**Importante:**

- La instalación de xammp debe tener establecida de forma correcta la
  Propiedad de los archivos y los permisos.
- Usar el **comando CHOWN causa problemas de Propiedad con xampp**.
- **Usar nautilus** para manipular los archivos/carpetas en localhost
  **causa problemas de Propiedad con xampp**.

**Información de base de datos**

Host

    localhost

Nombre predeterminado de la Base de datos

    test

Usuario predeterminado de la base de datos

    root

**NO** hay Contraseña predeterminada.

La contraseña del administrador es de tu elección.

La instalación de Datos de Ejemplo se recomienda para el usuario novato.

Después de la instalación elimina el directorio de instalación y apunta
tu Navegador a:

    http://localhost/yournewjoomlafolder

o

    http://localhost/yournewjoomlafolder/administrator

#### Crear un enlace en el menú de Ubuntu

**Para crear una interfaz gráfica de usuario para xammp conectado a tu
menú de Ubuntu**

Abre la Terminal y escribe

    sudo gedit /usr/share/applications/xampp-control-panel.desktop

A continuación, copia lo siguiente en gedit y guarda.

    [Desktop Entry]
    Encoding=UTF-8
    Name=XAMPP Control Panel
    Comment=Start and Stop XAMPP
    Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
    Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
    Terminal=false
    Type=Application
    Categories=GNOME;Application;Network;
    StartupNotify=true

Si el panel de control no se inicia, intenta ejecutar el comando Exec
directamente en el terminal:

    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"

Si recibe el mensaje de error:

    Error importing pygtk2 and pygtk2-libglade

Instala las bibliotecas faltante:

    sudo apt-get install python-glade2

#### XDebug PHP debugger

El paquete XAMPP para Linux no incluye XDebug PHP debugger. Para
instalar XDebug en Debian o Ubuntu:

\- Instala el paquete "build-essential":

    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf

Descarga <a href="http://www.apachefriends.org/es/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">paquete de desarrollo</a> para su
versión de XAMPP y extráelo sobre la instalación existente:

    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 

\- Construir XDebug:

    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize

Después de esto, tendrás la siguiente salida en la consola...

    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 

La salida será este.. por favor monitorea el directorio especificado.

    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 

Crear una carpeta en la carpeta temp que contenga el archivo de datos
generado por XDebug:

    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 

Instalaciones alternativas:

Instalar usando extensiones de la biblioteca de la comunidad PHP (PECL)
incluidas con xampp:

    sudo /opt/lampp/bin/pecl install xdebug

En Ubuntu/Debian se puede instalar con:

    apt-get install php5-xdebug 

(advertencia: esto también va a instalar Apache y PHP desde los
repositorios apt).

**Advertencia a los usuarios de 64 bits**

Al compilar o instalar XDebug a través de apt-get, recibirás un mensaje
de error cuando se (re)inicie xampp:

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64

Esto es debido a que xampp corre a 32 bits pero XDebug es de 64 bits.
Para superar este problema, crea "xdebug.so" en una máquina de 32 bits o
descárgalo de:

    http://code.activestate.com/komodo/remotedebugging/

Descarga el archivo: "PHP Remote Debugging Client" para "Linux (x86)"
Extrae el contenido del archivo en tu computadora, este archivo
comprimido contiene varias carpetas con números de versión, por ej.:
4.4, 5.0, 5.1 ... 5.3 y así sucesivamente, entra en la carpeta con el
número de versión más alto o uno que funcione para ti, a continuación,
copia manualmente el archivo "xdebug.so" a la siguiente ubicación,
sobrescribirlo si es necesario

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/

Recuerda esta ubicación puede ser diferente en tu equipo

### Instalación en Mac OS X

Mac OS X incluye un servidor Apache out-of-the-box, pero la mayoría de
los desarrolladores prefieren utilizar herramientas integradas y la
capacidad de configuración proporcionada por XAMPP.

Como con la mayoría de los programas en Mac, la instalación es una
brisa. Visita <a href="https://www.apachefriends.org/es/download.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> para
descagar el binario universal.

Una vez que el archivo haya terminado de descargar, sólo tienes que
abrir la imagen de disco, y arrastre la carpeta XAMPP a la carpeta alias
"Aplicaciones".

Para iniciar el servidor, abre "XAMPP Control.app" y pulsa el botón de
inicio junto a Apache.

##### Un Poco De Solución De Problemas

Muchos usuarios de Mac tienen un poco de dificultad en esta etapa cuando
se trata de establecer otro tipo de instancia de Apache en su máquina.
Si no puede iniciar XAMPP Apache, tienes dos opciones:  
**Puedes cambiar el puerto de escucha de XAMPP.** En
\Aplicaciones\XAMPP\xamppfiles\etc\httpd.conf, modifica la línea que
dice "Listen 80" a Listen \[número de puerto\]. Por ej.:

    Listen 8080

**Puedes cambiar el puerto de escucha del servidor Apache
pre-instalado.** En finder, ve a "/etc" (CMD+SHIFT+G); desde aquí serás
capaz de navegar a través de los archivos normalmente ocultos de Apache.
Encuentra la carpeta Apache2 y editar el archivo "http.conf". Modifica
la línea que dice "Listen 80" Listen \[número de puerto\]. Por ej.:

    Listen 8080

"Nota: Si seleccionas cambiar el puerto del servidor Apache
pre-instalado, es posible que debas reiniciar el equipo para que los
cambios surtan efecto. También tendrás que autenticarte como
administrador para cambiar estos archivos."

### Prueba de la Instalación de XAMPP

Una vez que XAMPP está instalado y se ha iniciado el servicio de Apache
con la herramienta del Panel de Control de XAMPP, puedes probar mediante
la apertura de tu navegador y navegar a
"<a href="http://localhost" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://localhost</a>". Debes ver la
pantalla de bienvenida de XAMPP similar a la de abajo.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo on xampp.png" />

Selecciona el enlace llamado "phpinfo()" en el menú de la izquierda.
Esto mostrará una larga pantalla con información acerca de la
configuración de PHP, como se muestra a continuación.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

En este punto, XAMPP está instalado correctamente. Presta atención al
archivo "Loaded Configuration File" que se destaca en la imagen de
arriba. Vamos a editar este archivo en la siguiente sección para
configurar XDebug.

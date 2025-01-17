<!-- Filename: XAMPP / Display title: XAMPP -->

## Introducción

XAMPP es un paquete fácil de instalar que incluye el servidor web Apache, PHP, XDEBUG y la base de datos MySQL. Esto te permite crear el entorno que necesitas para ejecutar Joomla! en tu máquina local. La última versión de XAMPP está disponible en <a href="http://www.apachefriends.org/en/index.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">el sitio web de XAMPP</a>. Las descargas están disponibles para Linux, Windows, Mac OS X y Solaris. Descarga el paquete para tu plataforma.

*Nota importante sobre XAMPP y Skype:* Tanto Apache como Skype utilizan el puerto 80 como una alternativa para conexiones entrantes. Si usas Skype, ve al panel Herramientas-Opciones-Avanzado-Conexión y deselecciona la opción "Usar 80 y 443 como alternativas para conexiones entrantes". Si Apache se inicia como un servicio, tomará el puerto 80 antes de que Skype inicie y no verás un problema. Pero, para estar seguro, desactiva la opción en Skype.

### Instalación en Windows

La instalación para Windows es muy sencilla. Puedes usar el ejecutable del instalador de XAMPP (por ejemplo, "xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Las instrucciones detalladas de instalación para Windows están disponibles <a href="https://www.apachefriends.org/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">aquí</a>.

Si estás en Windows XP o 2003, estos no son compatibles con el paquete principal, pero hay versiones compatibles de XAMPP para estas plataformas listadas en la página de descargas (pero solo podrás ejecutar PHP 5.4 o inferior - y por lo tanto, solo podrás probar Joomla 3.x y versiones anteriores).

Para Windows, se recomienda instalar XAMPP en "c:\xampp" (no en "c:\program files"). Si hace esto, su Joomla! (y cualquier otra carpeta de sitio web local) irá en la carpeta "c:\xampp\htdocs". (Por convención, todo el contenido web va en la carpeta "htdocs").

Si tienes múltiples servidores http (como IIS) puedes cambiar el puerto de escucha de xampp. En \apache\conf\httpd.conf, modifica la línea Listen 80 a Listen \[número de puerto\] (ej: "Listen 8080").

Tutorial de la Revista Comunitaria de Joomla

Puedes encontrar un tutorial detallado sobre cómo instalar XAMPP en Windows, junto con Joomla 4 Beta, Joomla Patch Tester y Git en este <a href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git" class="external text" target="_blank" rel="noreferrer noopener">artículo de la Joomla Community Magazine</a>.

### Instalación en Linux

#### Instalar XAMPP

Abre Terminal e ingresa:

```bash
    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt
```

(reemplace *xampp-linux-1.7.7.tar.gz* con la versión de xammp que
descargaste). Se ha informado que la base de datos MYSQL de xampp 1.7.4
no funciona con Joomla 1.5.22.

Esto instala... Apache2, mysql y php5, así como un servidor ftp.

```bash
    sudo /opt/lampp/lampp start
```

y

```bash
    sudo /opt/lampp/lampp stop
```

inicia/detiene todos los servicios

#### Prueba tu servidor localhost de XAMPP

Abre tu navegador y dirígelo a

```bash
    http://localhost
```

El index.php redirigirá a

```bash
    http://localhost/xampp
```

Allí encontrará instrucciones sobre cómo cambiar los
nombres de usuario/contraseñas predeterminados. En una PC que no sirve archivos a Internet
o LAN, entonces cambiar los valores predeterminados es una decisión personal.

#### Obtener Joomla

Descargar el último archivo zip de instalación de Joomla
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Descomprime en tu disco duro

Conéctese a localhost con un cliente FTP predeterminado

```
    nobody
    lampp
```

Crea una carpeta para tu Joomla en el servidor local

Transfiera por FTP los archivos de instalación de Joomla descomprimidos a la carpeta de Joomla recién creada.

**Importante:**

- La instalación de xampp establece la propiedad correcta de los archivos y los permisos.
- Usar el **comando CHOWN** **causará problemas de propiedad con xampp**.
- **Usar nautilus** para manipular carpetas/archivos en localhost **causará problemas de propiedad con xampp**.

**Información de la base de datos**

- Host: localhost
- Nombre de la base de datos por defecto: test
- Usuario por defecto de la base de datos: root
- **No** hay contraseña por defecto.

La contraseña del Administrador es de tu elección.

Se recomienda instalar datos de muestra para el usuario principiante.

Después de la instalación, elimina el directorio de instalación y dirige tu navegador a:

```
    http://localhost/yournewjoomlafolder
```

o

```
    http://localhost/yournewjoomlafolder/administrator
```

#### Crear un enlace en el menú de Ubuntu

**Para crear una interfaz gráfica para xampp conectada a tu menú de Ubuntu**

Abre la Terminal y escribe

```bash
    sudo gedit /usr/share/applications/xampp-control-panel.desktop
```

Luego, copie lo siguiente en el gedit y guarde.

```bash
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
```

Si el panel de control no se inicia, intenta ejecutar el comando Exec directamente en la terminal:

```bash
    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
```

Si recibe el error:

```bash
    Error importing pygtk2 and pygtk2-libglade
```

Instala las bibliotecas faltantes:

```bash
    sudo apt-get install python-glade2
```

#### Depurador PHP XDebug

El paquete XAMPP para Linux no incluye el depurador XDebug PHP.  
Para instalar XDebug en Debian o Ubuntu:

- Instala el paquete *build-essential*:
```bash
    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf
```

- Descarga el <a href="http://www.apachefriends.org/en/xampp-linux.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">paquete de desarrollo</a> para tu versión de XAMPP y extráelo sobre tu instalación existente:
```bash
    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 
```

- Construir XDebug:
```bash
    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize
```

Después de esto tendrás la siguiente salida en tu consola…

```bash
    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 
```

Entonces la salida será esta.. por favor, supervise el directorio especificado.

```bash
    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 
```

Crea una carpeta en tu carpeta temporal que contendrá el archivo de datos generado por XDebug:

```bash
    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 
```

Instalaciones alternativas:

Instalar usando la biblioteca comunitaria de extensiones de PHP (PECL) incluida con xampp:

```bash
    sudo /opt/lampp/bin/pecl install xdebug
```

En Ubuntu/Debian puedes instalar usando:

```bash
    apt-get install php5-xdebug 
```

(advertencia: esto también instalará Apache y PHP desde los repositorios apt).

**Advertencia para usuarios de 64 bits**

Al compilar XDebug o instalarlo a través de apt-get, recibirás un
error al (re)iniciar xampp:

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64
```

Esto se debe a que xampp se ejecuta en 32 bits pero XDebug es de 64 bits. Para superar este problema, puedes hacer xdebug.so en una máquina de 32 bits o descargarlo de:

```bash
    http://code.activestate.com/komodo/remotedebugging/
```

Descarga el archivo: "PHP Remote Debugging Client" para "Linux (x86)"  
Extrae el contenido del archivo en tu computadora, este archivo comprimido  
contiene varias carpetas con números de versión, por ejemplo: 4.4, 5.0, 5.1 ... 5.3  
y así sucesivamente, entra en la carpeta con el número de versión más alto o la  
que funcione para ti, luego copia manualmente el archivo "xdebug.so" a la  
siguiente ubicación, sobrescribe si es necesario

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/
```

Recuerda que esta ubicación podría ser diferente en tu computadora.

### Instalación en Mac OS X

Mac OS X en realidad incluye un servidor Apache listo para usar, pero la mayoría de los desarrolladores preferirán utilizar las herramientas integradas y la capacidad de configuración que ofrece XAMPP.

Al igual que con la mayoría de los programas en Mac, la instalación es muy sencilla. Visita <a href="https://www.apachefriends.org/en/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> para descargar el binario universal.

Una vez que el archivo haya terminado de descargarse, simplemente abre la imagen de disco y arrastra la carpeta XAMPP al alias de la carpeta "Aplicaciones".

Para iniciar el servidor, abre "XAMPP Control.app" y presiona el botón de inicio junto a Apache.

##### Un poco de solución de problemas

Muchos usuarios de Mac tienen un poco de dificultad en esta etapa al intentar configurar otra instancia de Apache en su máquina. Si no puedes iniciar el Apache de XAMPP, tienes dos opciones:  
**Puedes cambiar el puerto de escucha de XAMPP.** En \Applications\XAMPP\xamppfiles\etc\httpd.conf, modifica la línea que dice, "Listen 80" a Listen \[portNumber\]. Ej.:

```bash
    Listen 8080
```

**Puedes cambiar el puerto de escucha del servidor Apache preinstalado.** En Finder, ve a "/etc" (CMD+SHIFT+G); desde aquí podrás navegar a través de los archivos de Apache que normalmente están ocultos. Encuentra la carpeta etiquetada como Apache2 y edita el archivo "http.conf". Modifica la línea que dice "Listen 80" a Listen \[portNumber\]. Por ejemplo:

```bash
    Listen 8080
```

*Nota: Si decide cambiar el puerto del servidor Apache preinstalado, es posible que deba reiniciar su computadora para que los cambios surtan efecto. También tendrá que autenticarse como administrador para cambiar estos archivos.*

### Probar la instalación de XAMPP

Una vez que XAMPP esté instalado y hayas iniciado el servicio Apache con la herramienta XAMPP Control Panel, puedes probarlo abriendo tu navegador y navegando a "<a href="http://localhost" class="external free" target="_blank" rel="nofollow noreferrer noopener">http://localhost</a>". Deberías ver la pantalla de bienvenida de XAMPP similar a la que se muestra a continuación.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo en xampp.png" />

Seleccione el enlace llamado "phpinfo()" en el menú superior. Esto mostrará una larga pantalla con información sobre la configuración de PHP, como se muestra a continuación.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

En este punto, XAMPP se ha instalado correctamente. Observe el "Archivo de Configuración Cargado". Editaremos este archivo en la siguiente sección para configurar XDebug.

*Traducido por openai.com*


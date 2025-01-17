<!-- Filename: Visual_Studio_Code_Primer / Display title: Visual Studio Code -->

## VS Code - Un IDE Gratuito Popular

De [Wikipedia](https://en.wikipedia.org/wiki/Visual_Studio_Code):

> Visual Studio Code, también conocido comúnmente como VS Code, es un
> editor de código fuente creado por Microsoft para Windows, Linux y macOS.
> Las características incluyen soporte para depuración, resaltado de sintaxis,
> autocompletar inteligente de código, fragmentos de código, refactorización de código y Git integrado.
> Los usuarios pueden cambiar el tema, los atajos del teclado, las preferencias e
> instalar extensiones que añaden funcionalidad adicional.

## Instalación

La página predeterminada del sitio de [VS Code](https://code.visualstudio.com/) tiene una lista desplegable para cada plataforma compatible. Lo más probable es que tu plataforma esté preseleccionada. Así que descarga e instala y estarás listo para comenzar.

### Comenzando

La página de *Comenzar* de VS Code tiene algunos elementos de *Inicio*, una lista de elementos *Recientes* y una breve lista de *Guías*. Si eres completamente nuevo en VS Code, te recomendamos verlos. Solo toman unos minutos.

La documentación de VS Code está disponible en el menú *Ayuda / Documentación*. Los videos introductorios valen la pena ser vistos. Cada uno dura de 2 a 6 minutos y ofrece una excelente introducción a las características de VS Code:

[VS Code Videos](https://code.visualstudio.com/docs/getstarted/introvideos)

La documentación oficial es el lugar al que debes acudir si deseas buscar información específica.

### Extensiones de VS Code

VS Code se puede usar para cualquier tipo de texto, incluyendo una amplia gama de lenguajes de programación. Funciona con JavaScript sin agregar extensiones. Otros lenguajes son detectados por contexto, así que si comienzas a crear y guardar código PHP, probablemente se te solicitará instalar un paquete de soporte para PHP.

Haz clic en el icono de *Extensiones* en la *Barra de Actividades* izquierda para ver lo que tienes instalado y lo que se recomienda. ¡Necesitarás la extensión PHP Debug!

### La Disposición de la Pantalla de VS Code

Algunos términos utilizados en instrucciones posteriores:

- **Barra de actividad:** la barra estrecha a la izquierda de la pantalla. Selecciona cualquier ícono para abrir o cerrar la Barra lateral principal.
- **Barra lateral principal:** cuando está abierta muestra detalles de la actividad seleccionada.
- **Barra de estado:** en la parte inferior de la pantalla. Muestra lo que está sucediendo.
- **Panel:** un área debajo de los editores de texto para mostrar otra información.

Seleccione un icono de diseño en la parte superior derecha para abrir o cerrar cualquiera de estos elementos.

## Codificando una Extensión de Joomla

Para crear una extensión, tu objetivo es crear un archivo zip que puedas instalar en un sitio Joomla en funcionamiento. Por lo tanto, necesitas una carpeta para contener tu código. Esto debe estar dentro de tu espacio de archivos personal en tu computadora portátil o de escritorio utilizada para el desarrollo local. No debe estar en el árbol de tu sitio web. Por ejemplo, podrías usar *~/jextensions* para contener subcarpetas para diferentes extensiones. Yo uso *~/git* porque es corto y fácil de deletrear, aunque potencialmente confuso porque cada subcarpeta utiliza un repositorio git separado.

### Código de ejemplo

Si desea un ejemplo de código para trabajar, hay una extensión disponible en GitHub llamada *mod_debugme*. Como su nombre lo indica, es un módulo con algunos errores. Además del código del módulo, hay un archivo *build.xml* para ilustrar una forma de automatizar la construcción para pruebas y crear un archivo zip.

El módulo está diseñado para mostrar los próximos pocos eventos (3 por defecto) (cumpleaños) de una lista almacenada en una tabla de base de datos. Podrías imaginar que se usa en una oficina o en un sitio familiar con la expectativa de pastel.

Puede ser mejor comenzar utilizando comandos de git desde la línea de comandos. Primero crea una carpeta para tu código y luego clona el repositorio remoto:

```sh
    mkdir ~/git
    cd ~/git
    git clone https://github.com/ceford/j4xdemos-mod-debugme
```

La respuesta debería tomar sólo unos segundos:

```sh
    Cloning into 'j4xdemos-mod-debugme'...
    remote: Enumerating objects: 23, done.
    remote: Counting objects: 100% (23/23), done.
    remote: Compressing objects: 100% (16/16), done.
    remote: Total 23 (delta 3), reused 23 (delta 3), pack-reused 0
    Unpacking objects: 100% (23/23), done.
```

Deberías tomarte un momento para mirar el contenido de la carpeta:

```sh
    cd j4xdemos-mod-debugme
    ls -al
    total 16
    drwxr-xr-x   6 ceford  staff   192  2 Sep 17:48 .
    drwxr-xr-x   3 ceford  staff    96  2 Sep 17:48 ..
    drwxr-xr-x  12 ceford  staff   384  2 Sep 17:48 .git
    -rw-r--r--   1 ceford  staff  1402  2 Sep 17:48 README.md
    -rw-r--r--   1 ceford  staff   927  2 Sep 17:48 build.xml
    drwxr-xr-x   8 ceford  staff   256  2 Sep 17:48 mod_debugme
```

La carpeta *.git* contiene información sobre el repositorio. El archivo *README.md* es un documento markdown que describe este repositorio. El archivo *build.xml* es un archivo utilizado para construir la extensión con una herramienta externa, Phing - descrita más adelante. La carpeta *mod_debugme* contiene el código de la extensión.

### Instalar en Joomla

Comprime la carpeta de la extensión para crear un archivo zip instalable:

```sh
    zip -r mod_debugme.zip mod_debugme
```

Ahora puedes instalar el archivo zip en el sitio de Joomla que usas para pruebas. Después de la instalación, necesitas crear un módulo del sitio y asignarlo a una posición de módulo. Como es un módulo en desarrollo, podrías asignarlo a una posición en *Todas las páginas* mientras trabajas en él; o podrías asignarlo a una posición en una sola página; o podrías posicionarlo en un artículo que tenga su propio ítem de menú.

Después de la instalación, elimina el archivo zip.

### Activa el modo de depuración

En la Configuración Global de Joomla, configura *Debug System* a *Yes* y *Error Reporting* a *Maximum*.

Cuando abras una página que contenga el módulo con errores, verás un seguimiento de la pila que te indicará dónde se produjo un error.

![Stack trace](../../../en/images/getting-started/vscode-primer-stack-trace.png)

A veces, el error de codificación está en la primera línea del seguimiento de la pila. De lo contrario, si el error se desencadena en el código de una biblioteca, por ejemplo, al pasar datos no válidos a una función de base de datos, el error de codificación puede estar más abajo en la lista de llamadas a la función.

## Abrir carpeta de extensiones en VS Code

En VS Code, utiliza el elemento de menú Archivo / Abrir carpeta para localizar y abrir la carpeta que contiene tu copia local del código de la extensión *mod_debugme*. Deberías ver algo similar a lo siguiente:

![VS Code screen](../../../en/images/getting-started/vscode-primer-screen.png)

Es posible que puedas diagnosticar el problema simplemente leyendo el código. En el caso del error *Clase "DebugHelper" no encontrada* verás que una declaración *use* ha sido comentada unas pocas líneas antes. ¡Olvidar insertar una declaración *use* es un error común durante el desarrollo inicial!

Arregla ese problema y luego comprime e instala el módulo nuevamente. Ese paso se vuelve un poco tedioso cuando tienes múltiples problemas. Ahí es donde las herramientas de construcción resultan útiles.

## La Herramienta de Construcción Phing

Phing es una herramienta de línea de comandos, disponible para todas las plataformas, que se utiliza para construir paquetes de software utilizando instrucciones almacenadas en un archivo xml, llamado build.xml por defecto. Para trabajar con el código de extensión se puede utilizar para hacer dos cosas:

- copia los archivos modificados de la carpeta fuente de tu extensión a los lugares correctos en la carpeta de tu sitio web.
- genera un nuevo archivo zip para nuevas instalaciones.

Descarga e instala [Phing](https://www.phing.info/). ¡Existen otras herramientas de construcción disponibles! Podrías instalar Phing en tu propia carpeta bin o en una carpeta bin del sistema. Necesitas anotar la ruta a tu código de Phing. En este ejemplo es *~/bin/phing-latest.phar*. Puedes probarlo desde la línea de comandos después de cambiar al directorio que contiene el código de tu extensión:

```sh
    cd ~/git/j4xdemos-mod-debugme
    php ~/bin/phing-latest.phar
```

Respuesta:

```sh
    Buildfile: /Users/ceford/git/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:
          ... Any copied files will be mentioned here
          [zip] Building zip: /Users/ceford/zips/mod_debugme.zip

    BUILD FINISHED

    Total time: 0.0863 seconds
```

## Tareas de VS Code

Para ejecutar Phing desde dentro de VS Code, necesitas crear un archivo *tasks.json* en la carpeta *.vscode* en la raíz de la carpeta *j4xdemos-mod-debugme*. Si esta última no existe, primero créala. Luego crea el archivo *tasks.json* y introduce el siguiente código:

```json
    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "tasks": [
          {
            "label": "Build mod_debugme",
            "type": "shell",
            "command": "php ~/bin/phing-latest.phar",
            "windows": {
              "command": "php ~/bin/phing-latest.phar"
            },
            "group": "build",
            "presentation": {
              "reveal": "always",
              "panel": "shared"
            }
          }
        ]
    }
```

Los usuarios de Windows necesitan corregir el comando específico de Windows. Ahora puedes compilar la extensión usando el menú *Terminal / Ejecutar tarea de compilación*. El resultado del comando debería aparecer en el panel de la terminal debajo del área de edición.

```sh
      *  Executing task: php ~/bin/phing-latest.phar

    Buildfile: /Users/ceford/git/gitdemo/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:

          [zip] Nothing to do: /Users/ceford/zips/mod_debugme.zip is up to date.

    BUILD FINISHED

    Total time: 0.1031 seconds

     *  Terminal will be reused by tasks, press any key to close it.
```

Puede haber mensajes de error incomprensibles. La causa más probable es que haya rutas inválidas a carpetas en el archivo *build.xml* o que no se haya creado una carpeta. ¡Solo otro tipo de problema para depurar!

## Depuración

Deberías poder arreglar el primer error mediante la inspección del código. Problemas más complicados requieren seguir el código paso a paso con el depurador. Eso te permite inspeccionar variables para ver si contienen los valores que esperas, por ejemplo al pasar argumentos a funciones de biblioteca.

### Configuraciones de *php.ini*

Para configurar la depuración con Xdebug, necesitas hacer algunas entradas en la parte superior de tu archivo *php.ini*.

```sh
    zend_extension="xdebug.so"
    xdebug.mode="debug"
    xdebug.client_port=9003
    xdebug.start_with_request=yes
    xdebug.log_level=0
```

Después de guardar cualquier cambio, reinicie su servidor Apache.

### Añadir Ventana de Sitio Web

Tu carpeta de extensiones contiene solo unos pocos archivos, las ***fuentes*** de los archivos instalados en tu sitio web. La depuración en tiempo de ejecución implica establecer puntos de interrupción en los archivos de tu ***sitio***, por lo que necesitas acceso a esos archivos. Podrías usar el menú *Archivo / Añadir Carpeta al Espacio de Trabajo...* para agregar la carpeta del sitio a tu Espacio de Trabajo. Sin embargo, hay una gran probabilidad de que termines haciendo cambios en los archivos del sitio en lugar de los archivos fuente. Así que probablemente sea mejor abrir una ventana separada de VS Code para la depuración.

- **Abrir nueva ventana:** Desde el menú de VS Code, selecciona *Archivo / Nueva Ventana* y selecciona la carpeta que contiene tu sitio web de Joomla.
- **Abrir carpeta:** En la ventana recién abierta, selecciona *Archivo / Abrir Carpeta...* desde el menú de VS Code. Encuentra la carpeta de tu sitio web y selecciónala. Deberías ver una lista de todos los archivos de tu sitio web de Joomla en la barra lateral principal.

### Configuración de Lanzamiento

Para que la depuración funcione realmente en VS Code, necesitas una configuración de lanzamiento. En la raíz de tu sitio web, crea una carpeta llamada *.vscode* (nota el punto al principio) que contenga un archivo llamado *launch.json* con el siguiente contenido:

```json
    {
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "hostname": "0.0.0.0",
                "port": 9003,
                "stopOnEntry": true,
                "pathMappings": {
                    "/Users/ceford/Sites/j421rc2": "${workspaceFolder}"
                }
            }
        ]
    }
```

Recuerda reemplazar el elemento pathMappings en este ejemplo con los pathMappings reales en tu propio sitio. El elemento stopOnEntry pausará la ejecución en la primera línea de código PHP ejecutada.

### Depurar *mod_debugme*

Ahora estás listo para encontrar y corregir los errores en el módulo instalado.

- **Encuentra el código del módulo:** Encuentra el primer error en la línea 16 de JROOT/modules/mod_debugme/mod_debugme.php.
- **Establece un punto de interrupción:** Haz clic en el espacio a la izquierda del número 16. Aparecerá una burbuja roja pálida al pasar el ratón y se volverá completamente roja después de hacer clic para indicar que se ha establecido un punto de interrupción.
- **Iniciar depuración:** Desde el menú de VS Code selecciona *Run / Start Debugging*. En tu navegador, recarga tu sitio. Tu ventana de VS Code debería reaparecer con el código detenido en la primera línea del archivo *index.php* del sitio. En la parte superior de la pantalla hay algunos íconos para controlar el proceso de depuración. Deberían ser autoexplicativos. Si no, consúltalos en la Ayuda / Documentación de VS Code.
- **Continuar:** Selecciona el botón de continuar - el código se ejecutará hasta tu primer punto de interrupción. Examina el código para ver cuál es el problema.
- **Pasar el ratón:** Si pasas el ratón sobre una variable a la que se le ha asignado un valor, aparecerá una pequeña Información sobre herramientas resumiendo los atributos de esa variable. No hay Información sobre herramientas para variables a las que no se les han asignado valores.
- **Variables:** La columna izquierda contiene más información sobre el estado del código en el punto de interrupción. Hay demasiadas para cubrirlas todas aquí. ¡Explóralas según sea necesario!
- **Detener depuración:** Probablemente sea mejor seleccionar el ícono de Continuar, de lo contrario, la página web se mostrará en blanco. De lo contrario, podrías usar el botón de Detener o el menú Run / Stop Debugging.

### Corregir un error

**Recuerda:** ¡No arregles el error en el código del sitio web! ¡Arréglalo en el código fuente!

Corrige el código fuente y luego usa *Terminal / Ejecutar tarea de construcción...*.

Reiniciar depuración.

### Consejos

Algunos problemas no tan obvios:

- Solucionas el primer error, pero aún se bloquea en esa línea. Busca en mod_debugme.xml para ver dónde se define el src de las clases con espacio de nombres. Cuando lo arregles en la fuente, necesitas reinstalar el archivo zip o eliminar *administrator/cache/autoload_psr4.php*. Cuando está ausente, Joomla reconstruye ese archivo a partir de los archivos de manifiesto. Pero si tiene una entrada incorrecta o falta, no se arregla hasta que se reinstala la extensión.
- A veces necesitas establecer un punto de interrupción unas líneas antes de la línea donde ocurre el error, especialmente si deseas verificar los valores pasados a las llamadas de función.
- La tabla *xxx.yyy\\debugme* no existe. Ah, sí, el código para crear una tabla en la instalación y eliminarla en la desinstalación nunca fue creado. Necesitarás ejecutar una consulta sql en phpMyAdmin usando el contenido del archivo *mod\\debugme.sql*. Recuerda cambiar *\#\\* en los nombres de las tablas por el prefijo de tu base de datos. Y cuando aún falle, verifica el nombre de la tabla en el código.

## Captura de pantalla

Cuando todo esté arreglado, esto es lo que podrías ver:

![Site view of debugged module working](../../../en/images/getting-started/vscode-primer-debugme-fixed.png)

¿Días de pastel?

## Referencias

De la Documentación de Joomla!: [Visual Studio Code](https://docs.joomla.org/Visual_Studio_Code "Visual Studio Code"), que también cubre la configuración de otras herramientas, por ejemplo, CodeSniffer y PHPUnit.

*Traducido por openai.com*


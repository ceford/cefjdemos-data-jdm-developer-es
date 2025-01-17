<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Configurando Eclipse PDT 2020 y Git para Pulls -->

## Introducción

Comencé a usar Eclipse hace muchos años para un proyecto privado de Joomla y un repositorio git remoto privado que no estaba en Github. Leí los tutoriales disponibles y avancé felizmente hasta este año cuando decidí ayudar con las pruebas centrales y la corrección de errores para Joomla! 4. Y luego algunos pull requests. Fue entonces cuando me di cuenta de que no entendía exactamente lo que estaba haciendo. Finalmente decidí comenzar de nuevo y documentar el proceso para otros desarrolladores con experiencia media que son nuevos en Joomla, especialmente las partes que no valoré completamente la primera vez. Primero en la lista:

## Estructura de Archivos

No es evidente antes de que descargues e instales Eclipse que existen al menos dos lugares donde se almacenan los datos aparte de donde se encuentra el código de Eclipse.

### El espacio de trabajo

Aquí es donde Eclipse almacena sus propios datos para cada proyecto. ¡No debería estar en el árbol web! Como trabajo principalmente en una laptop Mac, la ubicación predeterminada para mí es **/Users/username/eclipse-workspace**. Hay una subcarpeta para cada proyecto. Así que una podría ser **/Users/username/eclipse-workspace/joomla-cms**. Contiene una carpeta: .metadata. Los usuarios de Linux probablemente sabrán sustituir home por Users.

### El repositorio local de git y el sitio web de prueba

Aquí es donde se almacena el código del proyecto. Podría estar en el árbol web local si deseas usarlo para pruebas. Para mí es **/Users/username/Sites/joomla-cms-4**. Los usuarios de Linux probablemente sabrán que deben sustituir public_html por Sites. Ten cuidado con los pasos adicionales necesarios para hacer que el repositorio local funcione como un sitio de Joomla.

### Un sitio web de prueba separado

Esto es opcional pero requiere que el repositorio de git tenga un archivo build.xml personalizado, por ejemplo, build-local.xml, cubierto en el Apéndice.

## Software Requerido

Para configurar un sitio web de desarrollo y prueba en funcionamiento, primero necesitarás instalar el siguiente software:

- Apache
- MySQL o Maridb
- PHP
- phpMyAdmin - [PhpMyAdmin Home page](https://www.phpmyadmin.net/)
- Composer - [Composer Home page](https://getcomposer.org/)
- Node.js - [Node.js Home page](https://nodejs.org/en/)
- Eclipse PDT - [Eclipse PHP Development Tools](https://www.eclipse.org/pdt/)
- git (opcional para usuarios de git en línea de comandos) - [git Home page](https://git-scm.com/)
- phing (opcional para compilaciones personalizadas) - [phing Hom page](https://www.phing.info/)

Los primeros tres o cuatro a menudo vienen como un paquete para tu plataforma, conocido como una pila LAMP, WAMP o XAMP. Simplemente usa lo que tu plataforma ofrece o mira Apache Friends [XAMPP](https://www.apachefriends.org/)

Supongamos que has instalado todo excepto Eclipse.

## Bifurcar joomla-cms en Github

Hay un flujo de trabajo descrito en [Mi primera solicitud de extracción a Joomla! en Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") que no puedo elogiar lo suficiente. Muestra exactamente lo que necesitas hacer:

![Github work flow](../../../en/images/getting-started/core-work-flow-joomla.png)

Pasos

- Ve a [Github](https://github.com/) y obtén una cuenta, gratuita y rápida.
- En tu cuenta de Github, ve al repositorio joomla/joomla-cms: escribe joomla/joo... en el cuadro de búsqueda superior izquierdo y selecciona cuando aparezcan las opciones.
- En el repositorio joomla/joomla-cms, haz clic en el botón de bifurcar en la parte superior derecha. Eso te da una copia bifurcada de todo el código de Joomla 4, Joomla 5, ..., todo, en tu propia cuenta de Github.

De vuelta a tu estación de trabajo.

## Instalar Eclipse

Ya tengo dos versiones de Eclipse instaladas, Oxygen de hace unos años y Cocoa de 2020. A partir de aquí estoy instalando una segunda instancia de Cocoa. Veamos qué sucede:

- Ve al [sitio de Eclipse](https://www.eclipse.org/pdt/) y descarga la versión para tu plataforma.
- Sigue el procedimiento de instalación y finalmente inicia la aplicación de Eclipse, para mí **Eclipse 2.app**.
- Advertencia: *Eclipse 2.app* es una aplicación descargada de Internet. ¿Estás seguro de que deseas abrirla? **Abrir**
- Selecciona un directorio como espacio de trabajo - El predeterminado es /Usuarios/usuario/eclipse-workspace. **Examinar** y crea una subcarpeta joomla-cms-4.
- Lanzamiento: Eclipse está instalado y muestra la página de bienvenida.
- Revisa la configuración del IDE. Configura los elementos 1, 2, 5 y 6.
- Selecciona el icono de Workbench en la parte superior derecha.

En lugar de instalar otra versión de Eclipse, podría haber abierto un nuevo espacio de trabajo vacío.

¡Hasta ahora todo bien!

## Importar fork en Eclipse

En su nueva instalación local de Eclipse:

- Abre Preferencias y establece Equipo / Git / Carpeta de repositorio predeterminada a /Users/username/git (puedes o no utilizar esa ubicación)
- Abre Archivo / Importar / Git / Proyectos desde Git (con importación inteligente). Siguiente ...
- Clonar URI. Siguiente ...
- Copia la barra de URL de **tu bifurcación de Github** y pégala en el campo URL. No necesitas agregar credenciales de autenticación. Siguiente ...
- Ramas para importar ... eh ... Probablemente sea mejor importar todo. Siguiente ...
- Directorio. Aquí es donde podrías elegir una ubicación diferente para guardar el código. Navegué a /Users/username/public_html/joomla-cms-4, creándolo si es necesario.
- Rama inicial - si importaste todas las ramas. Estoy trabajando en Joomla 4 así que seleccioné 4.0-dev. Y noté que mi clon será **origen**. Siguiente ...
- Sorbo de café. La clonación tomará unos minutos.
- Importar fuente. La fuente debería ser la ubicación del código. Finalizar.
- Eclipse ahora muestra la raíz del árbol de código en el panel Explorador de Proyectos. Haz clic para ver más del árbol. Observa que este código aún no funcionará como un sitio Joomla. Entre otras cosas, no hay una carpeta de medios.

## Agregar el repositorio original de joomla-cms a Eclipse

- Mostrar la ventana de Repositorios Git: selecciona Ventana / Mostrar Vista / Otro
- Selecciona Git / Repositorios Git - la ventana aparece en la parte inferior de la pantalla.
- Expande joomla-cms / remotos / origen - si haces cambios en tu código y envías a origen, aquí es donde va.
- Haz clic derecho en Remotos y selecciona Crear Remoto...
- Establece el nombre del Remoto a **joomla-origin** y selecciona **Configurar fetch**.
- En Configurar Fetch selecciona **Cambiar**.
- En Seleccionar un URI pega la URL del repositorio original de joomla-cms: `https://github.com/joomla/`
- Deja Credenciales en blanco. Terminar.
- en Configurar Fetch: **Guardar y Fetch**.

## Crear un sitio funcional

Su copia del código joomla-cms necesita más pasos para que sea utilizable como un sitio web.

- Abre una terminal y cambia al directorio que contiene tu código clonado.
- Ejecuta composer install:
  - Los usuarios de Linux y OSX pueden configurar el siguiente alias de bash colocando lo siguiente dentro del archivo `~/.bash_profile` o `~/.zsh` (`\$ source ~/.bash_profile` para hacer efecto inmediatamente):
```sh
    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
```

- composer no está en mi ruta, así que lo sustituyo por php ~/composer/composer.phar
- jinstall
- que obtiene todas las dependencias de PHP de Joomla, las dependencias de Javascript, compila todo el Javascript ES6 y coloca los archivos en sus ubicaciones apropiadas.
- sorbo de café mientras se descargan las dependencias y se crean los archivos multimedia.
- En Eclipse, haz clic derecho en la raíz del proyecto y selecciona Actualizar. Verás que tu código ahora tiene una carpeta de medios.
- Si te interesa, usa un administrador de archivos para ver que la raíz también contiene un archivo llamado .gitignore y la carpeta de medios se menciona en él. Esto significa que no se comprometerá a tus repositorios git bifurcados locales o remotos.

Ahora estás listo para una instalación de Joomla:

- Crea una base de datos usando phpMyAdmin (o la línea de comandos de mysql si lo prefieres).
- Creé una base de datos llamada joomla-cms-4 con la intercalación utf8mb4-unicode-ci.
- Crea un nuevo usuario: el mío es jcms4 con una contraseña aleatoria generada (GAOC26r77bBLkkdA). Necesitas anotar la contraseña para usarla durante la instalación. Termina en texto plano en el archivo de configuración.
- Otorga todos los privilegios en la base de datos a este usuario - el valor predeterminado.
- En tu navegador favorito, ve a la raíz del nuevo sitio: localhost/joomla-cms-4/

### En mi Mac con macOS Catalina

- Configuración del entorno incompleta: Más detalles. Bah, solo arréglalo y verifica que la barra de direcciones contenga la URL que escribiste; la mía había añadido de alguna manera algunos caracteres extra.
- De lo contrario, pasa a un sitio en funcionamiento: no elimines la carpeta de instalación.

### En mi estación de trabajo Linux con Linux Mint 20

¡Ups! ¡Algo salió mal!

El inconveniente es que tengo mi código Joomla en mi espacio de archivo personal para que Eclipse tenga acceso de lectura/escritura. El servidor web también necesita acceso de escritura para escribir archivos de configuración, caché y registro, pero se está ejecutando como un usuario y grupo con bajos privilegios. Como estoy en una red doméstica privada, edité /etc/apache2/apache2.conf para comentar User \${APACHE_RUN_USER} y Group \${APACHE_RUN_GROUP} y agregar User miusuario y Group migrupo. Reinicia apache, luego...

- Pasa por un sitio en funcionamiento - no elimine la carpeta de instalación.

## Revisión de Código

En resumen:

- Obtén de joomla-origin para asegurarte de que mi clon local esté actualizado.
- Equipo / Fusionar / Seleccionar rama para fusionar - cuidado - seleccioné joomla-origin/4.0-dev
- Empuja a origin para asegurarte de que mi fork remoto esté actualizado.
- Crea una rama para algunos cambios de código que quiero hacer.
- Haz los cambios de código.
- Si los cambios son a archivos fuente css o js (los que están en sass o es6) ve a una ventana de terminal y ejecuta jinstall nuevamente.
- PRUEBA tu instalación local para ver si hay problemas.
- Comete los cambios de código.
- Empuja a origin para actualizar mi fork remoto con mis cambios.
- Ve a tu cuenta en Github y selecciona la rama que creaste con el código actualizado. Algo aterrador: dice **Esta rama está 11084 commits por delante, 134 commits por detrás de joomla:staging.** ¿Hice algo mal? ¡Aparentemente no!
- Selecciona el botón de Pull Request. Asegúrate de seleccionar la rama joomla correcta para fusionar. Para mí esto es 4.0-dev. Y asegúrate de haber seleccionado tu rama con el código alterado. ¡Adelante!
- La solicitud de extracción debe ser probada y aprobada y puede tardar días, semanas o meses. ¡Y el cambio puede ser rechazado!

## Mientras tanto

De vuelta en tu estación de trabajo:

- Vuelve a tu rama original, para mí: Team / Switch To / 4.0-dev
- Reconstruir: para mí Project / Build Project
- Observa que los archivos previamente cambiados se copian nuevamente en tu sitio de trabajo.
- PRUEBA: tu sitio de trabajo ha vuelto a estar como antes de tus cambios de código.

Ahora estás listo para crear otra rama para otro conjunto de cambios en el código.

## Si ocurre un desastre

En una etapa, mi clon local de alguna manera se corrompió y no tenía idea de cómo solucionarlo. Así que eliminé mi clon local y todos sus archivos asociados, vacié la base de datos, y luego volví a la etapa de **Importar fork en Eclipse** mencionada anteriormente. Eso puso mi clon local en sincronía con mi fork remoto, incluyendo cualquier rama que hubiera creado para solicitudes de extracción. ¡La nueva instalación funcionó sin problemas y estaba contento!

## Otros recursos

- [Configurando Eclipse para desarrollo de Joomla](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development "Special:MyLanguage/Configuring Eclipse for joomla development") (2012-2013) ¡Pero consigue la última versión de Eclipse PDT!
- [Mi primer pull request a Joomla! en Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") (2011-2014) Buena visión general aunque algo desactualizada.
- [Trabajando con git y github](https://docs.joomla.org/Working_with_git_and_github "Special:MyLanguage/Working with git and github") (2011-2015)
- [Configurando tu entorno local](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment") (2018-2020)
- [Configurando Eclipse y Xdebug](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug "Special:MyLanguage/Configuring Eclipse and Xdebug") (2013) Todo sobre depuración.
- [Trabajando con Git y Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse "Special:MyLanguage/Working with Git and Eclipse") (2014) Método para ediciones antiguas de Eclipse.
- [Ejecutando pruebas automatizadas desde Eclipse](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse "Special:MyLanguage/Running Automated Tests from Eclipse") (2020) ¿Para usuarios avanzados?
- [Configurando Xdebug para desarrollo PHP/Linux](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux "Special:MyLanguage/Configuring Xdebug for PHP development/Linux") (2016) Instalar y configurar.
- [Configurando el IDE Eclipse para desarrollo PHP/Linux](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux "Special:MyLanguage/Configuring Eclipse IDE for PHP development/Linux") (2019) Instalar y configurar para Linux.
- [Configurando Xdebug para desarrollo PHP](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development "Special:MyLanguage/Configuring Xdebug for PHP development") (2014) Pasos para Linux, Windows y Mac OS X.
- [Webinar: Usando Eclipse para desarrollo de Joomla!](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development "Special:MyLanguage/Webinar: Using Eclipse for Joomla! Development") (2014) Este video webinar de 45 minutos, grabado el 30 de abril de 2009, ofrece una visión general de las características de Eclipse para desarrollo de Joomla!.
- [Configurando tu estación de trabajo para el desarrollo de Joomla](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development "Special:MyLanguage/Setting up your workstation for Joomla development") (2020) Breve resumen del software requerido y IDEs alternativos.

## Apéndice

En una etapa, tenía mi repositorio local de git fuera de mi raíz web y necesitaba copiar cualquier cambio que hiciera a un sitio local instalado por separado. Eso necesitaba un archivo de compilación personalizado, mostrado aquí como referencia:

### Crear un archivo build-local.xml

El clon de Eclipse contiene un archivo build.xml, pero se utiliza para probar y crear un archivo zip descargable para una nueva instalación. Lo que quiero hacer es copiar cualquier cambio que haga en mi código clonado a mi sitio de prueba en mi portátil. Tenga en cuenta que solo quiero hacer cambios en el código PHP y no en JavaScript o CSS. Para hacer eso, creé un archivo separado llamado build-local.xml en la raíz del proyecto:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

    <property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />

    <property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
            <fileset refid="allfiles" />
        </copy>
    </target>
</project>
```

### Añadir una herramienta de construcción

- Haz clic derecho en la raíz del proyecto y selecciona Propiedades.
- Selecciona Builders / Nuevo / Programa / OK.
- Nombre: Construcción local con phing
- Ubicación: donde sea que phing esté instalado. Para mí es /usr/local/php5/bin/phing aunque estoy usando PHP 7.4.
- Directorio de trabajo: Navega por el espacio de trabajo y selecciona joomla-cms - se muestra como \${workspace_loc:/joomla-cms}
- Argumentos: -f build-local.xml
- Aplicar / OK
- En Builders: Aplicar y Cerrar
- Proyecto / Construir Proyecto
- Ve lo que sucede:

```sh
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:

    BUILD FINISHED

    Total time: 0.2121 seconds
```

*Traducido por openai.com*


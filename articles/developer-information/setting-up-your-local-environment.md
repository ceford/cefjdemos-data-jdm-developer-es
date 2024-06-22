<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Configuración de su Entorno Local -->

<span id="main-portal-heading">Tutorial  
Cómo configurar un Entorno Local para Joomla 4</span> Joomla!  4.x

Con Joomla! 4 hemos cambiado el proceso de desarrollo. Ya no es posible
clonar el repositorio y tener una instalación de Joomla utilizable.
Seguimos las mejores prácticas e implementamos un proceso de
construcción para el CMS.

## Guía de Inicio Rápido

Los pasos para configurar su entorno de desarrollo dependen de su
sistema operativo. No podemos escribir documentación para todos los
sistemas operativos (SO), utilice su motor de búsqueda favorito para
encontrar un HowTo.

### Herramientas que necesita

1.  *PHP*: básicamente lo mismo que necesita para ejecutar un sitio
    Joomla, pero necesita la versión PHP CLI (interfaz de línea de
    comandos). (Consulte la página [Configuración de un servidor LAMPP
    para el desarrollo de
    PHP](https://docs.joomla.org/Configuring_a_LAMPP_server_for_PHP_development "Special:MyLanguage/Configuring a LAMPP server for PHP development")).
2.  *Composer*: para administrar las dependencias PHP de Joomla. Para
    obtener ayuda con la instalación de Composer, lea la documentación
    en
    <a href="https://getcomposer.org/doc/00-intro.md" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>
    (en Inglés).
3.  *Node.js*: para compilar archivo JavaScript y SASS de Joomla. Para
    obtener ayuda con la instalación de Node.js, siga las instrucciones
    disponibles en
    <a href="https://nodejs.org/en/" class="external free" target="_blank"
    rel="nofollow noreferrer noopener">https://nodejs.org/en/</a>. Tenga
    en cuenta que necesitará NodeJS 12 o superior para instalar Joomla.
4.  *Git*: para la gestión de versiones.

### Pasos para Configurar el Entorno Local

1.  Clonar el repositorio
2.  Checkout de la rama *4.0-dev*
3.  Ejecutar `composer install` desde la raiz del repositorio git.
    (Puede agregar *--ignore-platform-reqs* si no tiene PHP-LDAP
    instalado localmente y no lo necesita)
4.  Ejecutar `npm ci` desde la raiz del repositorio git. (Tenga en
    cuenta que necesita npm 6.13.4 o superior para esto. Ejecutar
    `npm install -g npm@lts` para actualizar su versión de npm a la
    versión LTS).

Los usuarios de Linux y OSX pueden configurar el siguiente alias de bash
colocando lo siguiente dentro del archivo *~/.bashrc file*:

    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"

Esto eliminará todos los archivos compilados en su sistema y ejecutará
una nueva instalación como un comando llamando a `jinstall` dentro de su
instalación de Joomla. También puede usar el comando `jclean` para
volver a una rama de Joomla 3.x

## Una guía de inicio un poco más larga

Joomla es similar a muchas otras herramientas web en estos días. Tiene
una gran parte de PHP y cada vez tiene más código JavaScript. Si bien la
codificación PHP no necesita tanta preparación, JavaScript necesita
muchas herramientas. La razón principal es que nadie escribe código de
una manera que todos los navegadores entiendan, por lo que el código
debe transpilarse, por ejemplo, de ES6 a una versión compatible de
JavaScript. Lo mismo ocurre con CSS. Para Joomla estamos usando SASS y
esto se convertirá a CSS nativo para que cualquier navegador lo
entienda. El lado negativo, es que configurar un entorno de desarrollo
es un poco más complicado, pero las herramientas hacen que la
codificación sea más conveniente. Gracias a los observadores y a la
recarga automática del navegador, puede ver sus cambios en tiempo real.

### PHP

Debería ser suficiente ejecutar `composer install` ya que esto instalará
las dependencias PHP guardadas en el archivo *composer.lock*. Puedes
hacer esto tantas veces como quieras. Solo instalará paquetes nuevos
cuando se cambie el archivo *composer.lock*. No ejecute
`composer update` ya que esto actualizará todos los paquetes a versiones
más recientes y actualizará el archivo *composer.lock*.

**Nota:** Es posible que deba ejecutar `composer install` con la opción
`--ignore-platform-reqs` para ignorar los requisitos de plataforma
especificados en Composer. Es decir, si no tiene instalada la extensión
LDAP de PHP.

### Scripts de Node/npm

Node.js viene con un administrador de paquetes llamado NPM (en cierto
modo, es el mismo que Composer). NPM tiene un comando `run` y hemos
preparado algunos scripts para facilitarle el desarrollo. Debe ejecutar
los comandos en la raíz del repositorio cuando cambia los archivos JS o
SASS. Previamente, la primera vez necesita ejecutar `npm ci` para
instalar las dependencias.

#### npm run build:css

Compilará los archivos SASS en CSS y también creará los archivos
minificados.

#### npm run build:js

Compilará y transpilará los archivos JavaScript al formato correcto y
creará los archivos minificados.

#### npm run watch

Es lo mismo que el comando `build:js`, pero observará los cambios y
creará automáticamente los archivos actualizados en el directorio
*media*. Los archivos SASS aún no están incluidos.

#### npm run lint:js

Esto realizará una verificación de sintaxis en todos los archivos
JavaScript ES6 respecto al estándar de código javascript (para obtener
más información sobre el estándar de estilo de código de Joomla 4, lea
el manual de estándares de codificación en <a
href="https://developer.joomla.org/coding-standards/introduction.html%7Cel"
class="external text" target="_blank" rel="noreferrer noopener">manual
de estándares de codificación</a>).

#### npm run test

Esto ejecutará un conjunto de tests de JavaScript.

## Posibles Problemas

Cuando ejecute `composer install`, puede encontrarse con estos errores

    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.

La solución es ejecutar `composer install` con la opción
`--ignore-platform-reqs` para ignorar los requisitos de plataforma
especificados en Composer. Es decir, si no tiene instalada la extensión
LDAP de PHP.

    composer install --ignore-platform-reqs

Si recibe un error de inicio de sesión como el que se muestra a
continuación, elimine el archivo `library/autoload_psr4.php` como se
muestra en la segunda imagen.

<img
src="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/400px-Install-error.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/600px-Install-error.png 1.5x, https://docs.joomla.org/images/thumb/b/b3/Install-error.png/800px-Install-error.png 2x"
data-file-width="1920" data-file-height="1080" width="400" height="225"
alt="Login After Install Error" />

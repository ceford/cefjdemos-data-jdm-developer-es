<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Configuración de un Entorno Local -->

## Guía de Inicio Rápido

Para una configuración de prueba, necesitarás un conjunto de software que incluya Apache, MySQL y PHP. Los pasos requeridos se tratan en otra parte de este manual. En caso de duda, utiliza un conjunto de software apropiado para tu plataforma.

## Herramientas adicionales

Para probar solicitudes de extracción de Joomla y contribuir al código principal de Joomla necesitarás herramientas adicionales, todas fácilmente instalables, a menudo con un comando de una sola línea:

1. **Composer** - para gestionar las dependencias PHP de Joomla. Lea la [documentación de Composer](https://getcomposer.org/doc/00-intro.md) para obtener ayuda con la instalación.
2. **Node.js** - para compilar los archivos JavaScript y SASS de Joomla. Lea la [documentación de Node.js](https://nodejs.org/en/) para obtener ayuda con la instalación.
3. **Git** - para la gestión de versiones. Lea la [documentación de git](https://git-scm.com/) para obtener ayuda con la instalación.

## Pasos para Configurar un Sitio de Prueba Local

1. Ve al [repositorio de Joomla! en GitHub](https://github.com/joomla/joomla-cms).
2. Selecciona la rama de Joomla en la que trabajar. Esto selecciona las instrucciones README apropiadas.
3. Desplázate hacia abajo en la página hasta la sección **Pasos para configurar el entorno local:** del texto README.
4. Sigue los pasos listados allí. Puedes cambiar el nombre de la carpeta clonada joomla-cms para que puedas hacer tantos clones como desees para diferentes propósitos.
5. Crea una base de datos e instala Joomla, pero no elimines la carpeta de instalación al final del proceso de instalación.

## Actualizando Joomla

De vez en cuando, puede que necesite actualizar un clon de Joomla. Este es un comando de una sola línea que generalmente es rápido. Sin embargo, generalmente es necesario reconstruir los archivos CSS y JavaScript, lo cual es un proceso más complicado y largo.

Los usuarios de Linux y OSX pueden configurar el siguiente alias de bash colocando lo siguiente dentro del *archivo ~/.bashrc*:

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
    alias jinstall="jclean; composer install; npm ci"
```

Esto eliminará todos los archivos compilados y ejecutará una instalación nueva como un solo comando al llamar `jinstall` dentro de tu instalación de Joomla. También puedes usar el comando `jclean` para cambiar a una rama diferente de Joomla.

**Nota:** Es posible que necesites ejecutar `composer install` con la opción `--ignore-platform-reqs` para ignorar los requisitos de la plataforma especificados en Composer. Es decir, si no tienes instalada la extensión LDAP de PHP.

### Cambios en la base de datos

Si una actualización de Joomla incluye cambios en la base de datos, es posible que necesites encontrar los cambios individuales y ejecutarlos manualmente o comenzar de nuevo con un nuevo clon.

## Scripts de Node/npm

La instalación de Joomla desde un clon de repositorio utilizó dos comandos:

- **composer install** Instala todos los paquetes necesarios de composer.
- **npm ci** Instala todos los paquetes necesarios de npm.

Node.js viene con un gestor de paquetes llamado NPM que tiene un comando `run`. Hay algunos scripts disponibles para hacer el proceso de construcción más rápido si solo se han cambiado los archivos CSS o JavaScript.

### npm run build:css

Este comando compila archivos SASS a CSS y también crea los archivos minificados.

### npm run build:js

Este comando compila y transpila los archivos de JavaScript al formato correcto y crea archivos minificados.

### npm run watch

Este comando es el mismo que el comando `build:js` pero observará los cambios y construirá automáticamente los archivos actualizados en el directorio de medios. Los archivos SASS aún no están incluidos.

### npm run lint:js

Este comando realiza una verificación de sintaxis en todos los archivos JavaScript ES6 según el estándar de código JavaScript. Para más información, consulte el [manual de estándares de codificación de Joomla](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

Este comando ejecutará un conjunto de pruebas de JavaScript.

## Posibles Problemas

Al ejecutar composer install puedes encontrarte con estos errores

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

La solución es ejecutar el composer install con la opción `--ignore-platform-reqs` para ignorar los requisitos de plataforma especificados en Composer. Es decir, si no tienes instalada la extensión LDAP de PHP.

```sh
    composer install --ignore-platform-reqs
```

Si recibe un error de inicio de sesión, elimine el archivo `administrator/cache/autoload_psr4.php`.

*Traducido por openai.com*


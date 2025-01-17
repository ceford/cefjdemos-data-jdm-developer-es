<!-- Filename: Joomla_CodeSniffer / Display title: Estándares de Codificación -->

<div class="alert alert-warning">
¡La última parte de este artículo necesita ser actualizada!
</div>

## Una visión general de la IA

Los estándares de codificación son importantes para el desarrollo de software porque:

- **Mejorar la calidad del código**
  - Los estándares de codificación ayudan a garantizar que el código sea confiable, seguro y protegido. También pueden ayudar a reducir problemas de rendimiento y preocupaciones de seguridad que puedan surgir de malas prácticas de codificación.
- **Hacer el código más legible y mantenible**
  - Los estándares de codificación ayudan a que el código sea más fácil de entender, leer y mantener. Esto también puede facilitar que nuevos desarrolladores trabajen con el código.
- **Acelerar el desarrollo**
  - Los estándares de codificación pueden ayudar a los desarrolladores a evitar errores comunes que pueden ralentizar los procesos de codificación.
- **Mejorar la colaboración**
  - Los estándares de codificación pueden ayudar a facilitar la colaboración entre desarrolladores, incluso en equipos grandes.
- **Garantizar la consistencia**
  - Los estándares de codificación ayudan a garantizar que el código sea consistente en todos los proyectos. Esto puede facilitar que los desarrolladores trabajen juntos en los mismos proyectos.
- **Mejorar la escalabilidad**
  - Los estándares de codificación pueden ayudar a asegurar que el código pueda escalar sin volverse inmanejable.
- **Proporcionar criterios claros para revisiones de código**
  - Los estándares de codificación pueden ayudar a proporcionar criterios claros para las revisiones de código, lo que puede llevar a comentarios más efectivos.

Los estándares de codificación a menudo incluyen reglas para la indentación, la longitud de las líneas, la colocación de llaves y el espaciado.

Joomla utiliza el estándar de codificación [PSR-12](https://www.php-fig.org/psr/psr-12/). Puedes habilitar este estándar de codificación en tu IDE y recibir sugerencias si no sigues el estándar de codificación, o también usar una corrección automática. Recomendamos que sigas este estándar al desarrollar tus propias extensiones para mantener la compatibilidad con el núcleo y asegurar que tu código funcione, con suerte, con futuras versiones.

## PHP Code Sniffer

Consulte la fuente actual de [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) para obtener información sobre esta utilidad y cómo instalarla. Tenga en cuenta que la fuente original está abandonada, pero puede aparecer en los motores de búsqueda. Hay dos scripts:

- **phpcs** detecta infracciones de un estándar de codificación y produce un informe.
- **phpcbf** intenta corregir las infracciones de los estándares de codificación y produce un informe sobre lo que ha hecho.

Puedes instalar los scripts individuales o usar composer para instalarlos. También puedes encontrarlos en un clon de Joomla ubicado en libraries/vendor/bin junto con algunas otras utilidades útiles.

## Probando PHP Code Sniffer

Si pones una instalación global en tu `$PATH`, puedes ejecutar el analizador de código desde la línea de comandos en la raíz de un proyecto. Intenta esto para mostrar una lista de estándares de codificación:

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

Como ejemplo, el siguiente comando se utilizó en una carpeta de un proyecto anterior que usaba el antiguo estándar de codificación personalizada de Joomla. Entre otras cosas, usaba tabulaciones en lugar de espacios para el diseño. Muchas líneas similares se han omitido a continuación por brevedad.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

El ejemplo de proyecto anterior contenía un único archivo php y no archivos css o js. phpcs produce un informe para cada archivo php, css y js en un proyecto. Aquí hay algunos ejemplos para archivos css y js:

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```

## Variaciones de Comandos

Puede obtener ayuda con los comandos de phpcs:

```sh
phpcs --help
```

### Excluir uno o más archivos

Utiliza una lista de patrones de archivo separados por comas para excluir archivos de la validación de estilo de código. Ejemplo

```php
phpcs --standard=PSR12 --ignore='libraries/*' .
```

### Excluir una o más reglas

Joomla permite líneas más largas que el estándar PSR, 560 en lugar de 120. El siguiente comando se puede utilizar para omitir las advertencias de líneas largas:

```sh
phpcs --standard=PSR12 --ignore='libraries/*' --exclude=Generic.Files.LineLength .
```

Puedes encontrar la regla que se está violando con este comando:

```sh
phpcs -s yourfile.php
```

### Excepciones de Joomla

Para el desarrollo de una extensión utilizando un IDE, puedes decidir usar el estándar PSR12 sin ninguna excepción. Joomla tiene un [conjunto de reglas personalizadas](https://github.com/joomla/joomla-cms/blob/5.2-dev/ruleset.xml) que permite muchas excepciones. Se utiliza para la validación de toda la instalación de Joomla durante las pruebas del sistema.

## Reparando Infracciones

El único archivo php que `SE ENCONTRARON 132 ERRORES Y 3 ADVERTENCIAS QUE AFECTAN 116 LÍNEAS` mostrado arriba se puede arreglar principalmente de la siguiente manera:

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Ejecutar la utilidad phpcs nuevamente muestra qué problemas permanecen:

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```

Conclusión: ¡una extensión codificada para una versión anterior de Joomla y usando un estándar de codificación diferente ahora necesita algo de trabajo!

## Instalación en IDEs

La mayoría de los entornos de desarrollo integrados pueden usar el detector de código mientras trabajas o cuando guardas un archivo.

### Instalación en VSCode

En VSCode (y/o VScodium), selecciona el ícono de Extensiones, busca PHP_CodeSniffer e instálalo. Necesita configuración:

1. En **Ajustes** busca PHP_CodeSniffer
2. Ajusta el campo **PHP Code Sniffer → Exec:** para que se adapte a la ubicación de tu binario phpcs.
3. En la lista **PHP Code Sniffer: Standard** selecciona **PSR12**.

Cierra y estarás listo para la acción.

Algunos de los problemas mostrados arriba pueden solucionarse con directivas PSR1.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

¡Otros necesitan algo de reflexión!

Después de la instalación en VSCode, las violaciones de estilo de código serán detectadas y marcadas con una línea ondulada roja. Pasa el cursor sobre el problema para ver un mensaje y una posible solución, como en este ejemplo:

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
¡Las siguientes secciones necesitan ser actualizadas!
</div>

### Instalación en PhpStorm

Code Sniffer es compatible de inmediato en PhpStorm. Ve a Configuración y bajo **Editor → Inspecciones** verás la lista de sniffers que has instalado.

#### Establecer Ruta a Code Sniffer

1. Abre Configuración (CTRL-ALT-S / CMD-,)
2. Ve a Idiomas y Marcos de Trabajo
3. Haz clic en PHP
4. Haz clic en Herramientas de Calidad
5. Haz clic en la flecha desplegable de PHP cs fixer
6. La configuración está establecida en Local por defecto
7. Haz clic en los 3 puntos detrás para abrir la pantalla de configuración
8. La primera opción es la ruta del PHP Code Sniffer (phpcs)
9. Haz clic en los 3 puntos detrás de la ruta para seleccionar la ubicación del archivo phpcs. Consulta arriba para ver dónde puede estar instalado phpcs en tu sitio
10. Haz clic en Validar para asegurarte de que la ruta es correcta y phpcs está funcionando
11. Haz clic en OK

#### Activando el estilo de código

1. Abre Configuraciones (CTRL-ALT-S / CMD-,)
2. Ve a Editor
3. Haz clic en Inspecciones
4. En la lista, ve a PHP
5. Haz clic en Validación de PHP Code Sniffer
6. Haz clic en la casilla detrás de ella para activarla
7. Haz clic en el botón Recargar (2 flechas) para forzar una recarga de las reglas desde el disco
8. Joomla debería estar ahora disponible en la lista. Vea la imagen siguiente.
9. Haz clic en OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

### Instalación en Netbeans

Netbeans tiene la funcionalidad de sniffer integrada en el sistema central.

1. Inicia tu IDE Netbeans
2. Abre **Herramientas → Opciones → PHP → Análisis de Código → Code Sniffer**
3. Tienes que establecer la ruta a *phpcs.bat* del paquete PHP_CodeSniffer PEAR instalado
   - Para sistemas basados en Unix, la ruta es algo como /usr/bin/phpcs
   - En XAMPP (Windows) puedes encontrar el archivo en la carpeta raíz de PHP (por ejemplo, C:\xampp\php\phpcs.bat)
4. Como "Estándar Predeterminado," elige "Joomla" para usar el estándar de Joomla!
5. Ahora puedes hacer clic en *OK* para comenzar a olfatear
6. Abre desde el menú superior **Fuente → Inspeccionar...**
7. Disfruta

### Instalación en Eclipse

1. **Ayuda → Instalar nuevo software...**
2. **Trabajar con** Rellena una de las URLs del sitio de actualización.
3. Selecciona las herramientas deseadas.
4. Reinicia Eclipse.
5. **Ventana → Preferencias**
6. **Herramientas PHP → PHP CodeSniffer**

![Eclipse PTI settings](../../../en/images/getting-started/core-eclipse-pti-settings.png)

Ahora puedes detectar infracciones de código contra estándares comunes.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Instalación en Geany

- Abra un archivo PHP. (De lo contrario, el menú de construcción no es accesible.)
- En el menú superior, seleccione **Build → Set Build Commands**.
- Seleccione el segundo campo y nómbrelo como desee. Ingrese este código en el Comando:<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Ingrese este código en el campo de Expresión Regular de Error:  `(.+) [|]\s+([0-9]+)`
- Seleccione OK.
- Si la Ventana de Mensajes no está abierta, muéstrela seleccionándola en el menú superior Ver.
- Al ver cualquier archivo PHP, presione F9 para ver los errores encontrados.

### Instalación en Atom

- Instala phpcs y los estándares de Joomla como se describe arriba.
- En **Atom → Preferences → Install** instala **linter-phpcs** y todos sus requisitos.
- En **Atom → Preferences → Packages → linter-phpcs → Settings** ajusta
  - **Executable Path** a la ruta de tu phpcs
  - **Code Standard Or Config File**: PSR12
  - **Tab Width**: 4

*Traducido por openai.com*


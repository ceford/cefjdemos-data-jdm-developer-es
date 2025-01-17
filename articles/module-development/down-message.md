<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Ejemplo: Mensaje de Sitio Caído -->

## Introducción

Este artículo presenta un módulo completo de sitio Joomla para que los nuevos desarrolladores lo desarmen y vean su estructura y cómo funciona. Es un reemplazo de un módulo anterior que usaba convenciones de codificación más antiguas. Este está diseñado para Joomla 5 y posteriores.

Hay artículos introductorios sobre *Conceptos Generales* y *Módulos* en la Documentación para Programadores de Joomla, y se reproducen en Jdocmanual para su comodidad. Este artículo es similar al JPD [Basic Module](jdocmanual?article=docus/modules/basic-module) pero tiene algunas funcionalidades adicionales.

## Propósito

El módulo está diseñado para mostrar un mensaje temporal en uno de varios idiomas durante un corto período antes de que el sitio cierre por mantenimiento del sistema. Esto da a los usuarios la oportunidad de terminar lo que están haciendo antes de que ocurra el cierre.

El nombre del módulo es `mod_downmsg` y el mensaje que muestra en inglés es similar a *Este sitio cerrará por un corto período a las 12:00 GMT*. Se pueden seleccionar la hora y el mensaje para adaptarse al cierre inminente del sitio.

El módulo se puede descargar de GitHub para instalarlo o examinarlo según corresponda.

## Estructura del Código Fuente

La siguiente es una ilustración esquemática de la estructura del código fuente utilizado para crear la extensión:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Esta es la estructura tal como se ve en el IDE de VSCode o VSCodium:

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## El Archivo Manifiesto

El archivo de manifiesto controla los procesos de instalación, actualización y desinstalación de la extensión:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Notas del Elemento

- Los atributos de **extension**:
  - **type="module"** indica el tipo de extensión.
  - **method="upgrade"** significa que esta extensión puede ser instalada y actualizada si versiones más nuevas están disponibles.
  - **client="site"** le dice a Joomla que este es un módulo de *Sitio* en lugar de un módulo de *Administrador*.
- La declaración de **versión** se utiliza para controlar la actualización. Si hay un servidor de actualización disponible, se compara con la versión registrada allí. También previene que una versión más antigua sobrescriba una versión más reciente.
- El atributo de ruta de **namespace** le indica a Joomla dónde buscar las clases del espacio de nombres.
- La sección de **archivos** enumera carpetas y archivos para la instalación. Si un artículo no está presente aquí, no se instalará incluso si está presente en el archivo zip de instalación.
- El atributo de url de **ayuda** permite que se use un archivo de ayuda personalizado en el formulario de edición del módulo. La ruta al archivo de ayuda es un valor para la clave dada en el archivo en-GB/mod_downmsg.ini.
- La sección de **configuración** muestra qué parámetros necesita este módulo. Todos deben tener valores predeterminados porque se establecen en la instalación.

## Los archivos de lenguaje

Este módulo tiene muy pocas cadenas. Las que se encuentran en los archivos **sys.ini** se utilizan en la instalación y para listar entre otros módulos. Los archivos **.ini** se utilizan en el formulario del Administrador y en la representación del Sitio del módulo. Nota las convenciones para identificar cadenas:

MOD\_\[nombre\]\_\[propósito\]\_\[uso\]

Los archivos a continuación muestran que las traducciones al alemán y francés solo incluyen las cadenas que serán vistas por los visitantes del sitio, ya que el formulario de Parámetros siempre se administrará en inglés. En caso de que no lo supieras: las cadenas en-GB siempre se cargan primero por defecto para asegurar que las cadenas no traducidas accidentalmente no aparezcan como claves de cadena. Luego, se cargan las cadenas del idioma requerido y sobrescriben las cadenas equivalentes en inglés.

Las versiones en francés y alemán de las cadenas aquí se obtuvieron utilizando las Herramientas de Traducción de Google. ¡Esto puede funcionar mejor para oraciones que para palabras individuales!

### en-GB/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

Este archivo se utiliza para la gestión del sistema.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## El Código del Módulo

El código del módulo es increíblemente simple. Hay tres archivos PHP:

- services/provider.php (el código de inyección de dependencias).
- src/Dispatcher/Dispatcher.php (ensambla datos para ser utilizados en la exhibición).
- tmpl/default.php (la plantilla para mostrar los datos, que puede ser sobrescrita).

### services/provider.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Instalación

Desde el menú de Administrador, navega a la página Sistema / Instalar / Extensiones y utiliza la pestaña Subir archivo de paquete para cargar el archivo zip. Luego navega a Contenido / Módulos del sitio y edita el módulo recién instalado.

En el formulario de edición del módulo:

1. Introduce un título. Recuerda que un módulo puede tener más de una instancia por lo que pueden aparecer en diferentes ubicaciones o tener diferentes parámetros.
2. Establece la hora en que el sitio se caerá.
3. Establece la zona horaria (probablemente hay una mejor manera que usar GMT +- n horas).

En la lista de parámetros comunes de la derecha

1. Establece el **Título** en **ocultar**.
2. Selecciona una **Posición** de plantilla - en Cassiopeia **topbar** coloca el mensaje por encima del contenido de la página, perfecto.
3. Establece el **Estado** a **Publicado**.
4. En la pestaña de **Asignación de Menú** selecciona **En todas las páginas**.
5. **Guarda** y estarás listo para verificar la apariencia del sitio.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Pruebas

Así es como aparece el mensaje en inglés:

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

En alemán:

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

Y francés:

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Actualizar el Sitio y Registro de Cambios

Estos elementos están incluidos en la fuente pero no se tratan aquí.

*Traducido por openai.com*


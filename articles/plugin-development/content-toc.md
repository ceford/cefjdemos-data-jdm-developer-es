<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Ejemplo: Tabla de Contenidos -->

## Introducción

Este artículo proporciona un ejemplo de un plugin de contenido para ilustrar la codificación utilizando las convenciones de codificación más recientes de Joomla. El código completo está disponible en [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). Puedes descargarlo e instalarlo o simplemente descomprimirlo para inspeccionar el código.

Hay una sección sobre [Desarrollo de Plugins](https://manual.joomla.org/docs/building-extensions/plugins/) y más ejemplos en la Documentación para Programadores de Joomla, [copiados a este sitio](jdocmanual?article=docus/plugins/index) para conveniencia.

Este complemento genera un *Tabla de Contenidos* a partir de los encabezados en cualquier artículo que contenga un marcador `{cefjdemostoc}`. El resultado se ilustra en la última captura de pantalla de este artículo.

## Estructura del Código Fuente

La siguiente es una ilustración esquemática de la estructura del código fuente utilizado para crear la extensión:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Este es el esquema tal como se ve en el IDE de VSCode o VSCodium:

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## El Archivo Manifest

Cada extensión debe tener un archivo de manifiesto en formato xml. Joomla lo utiliza para los propósitos de instalación, actualización y eliminación. Este es el archivo de manifiesto para esta extensión:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## El archivo services/provider.php

El propósito de este archivo es registrar un proveedor de servicios y declarar cualquier herramienta que necesite. Este plugin en particular no necesita mucho. Si tu plugin personalizado necesitara realizar consultas a la base de datos, lo declararías aquí. Echa un vistazo a un plugin de autenticación. Requieren herramientas de base de datos y de usuario.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## La Clase de Extensión

El propósito del archivo src/Extension/Cefjdemostoc.php es preparar datos para su salida. El archivo tiene 91 líneas, por lo que no se reproduce aquí. Sin embargo, algunas partes del mismo requieren más explicación.

### Directivas de Codesniffer

Las líneas 16 a 18 tienen esto:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Las líneas comentadas evitan informes de errores del CodeSniffer. No sirven para ningún otro propósito.

### función pública onContentPrepare()

Este es el evento al que responde el plugin. Se le pasa un *contexto*, los datos del artículo y los parámetros del artículo. Se debe generar una *Tabla de Contenidos* solo si la página renderizada es un solo artículo. De lo contrario, es necesario eliminar el marcador de posición `{cefjdemostoc}`.

Para generar una tabla de contenido, se establecen algunos parámetros del artículo y cada encabezado en el artículo se modifica para agregar un número de secuencia **id**, por lo que `<h2>Introduction</h2>` se convierte en `<h2 id="cefjemostoc0">Introduction</h2>`. Luego se carga una plantilla para agregar la tabla de contenido renderizada.

### El archivo tmpl/default.php

El propósito de este archivo es representar la salida con los datos proporcionados. El archivo puede ser sobrescrito y lo verás entre las anulaciones de **plugins/contents** para la plantilla Cassiopeia.

El archivo se reproduce a continuación. Tenga en cuenta que algunos elementos tienen clases CSS codificadas y otras se obtienen de los parámetros del artículo. Las clases **fs-** son tamaños de fuente de Bootstrap. Los encabezados se almacenan en el array `$headings2`.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### El Gestor de Activos Web

Sin un estilo adicional, la tabla de contenidos (ToC) ocuparía todo el ancho de la pantalla en la ubicación del marcador `{cefjemostoc}` en el texto fuente. Eso a veces se ve en publicaciones técnicas, pero puede que no sea lo que deseas. En pantallas anchas, a menudo es mejor flotar la ToC y permitir que el texto del artículo fluya a su alrededor. Sin embargo, eso puede volverse inutilizable en pantallas estrechas. La mejor solución es proporcionar una hoja de estilo con consultas de medios personalizadas. En pantallas anchas, la ToC flota hacia la derecha, pero en pantallas estrechas no lo hace.

El código `default.php` anterior muestra cómo incluir una hoja de estilo personalizada para el plugin. El CSS está en el archivo `media/css/cefjdemostoc.css`. Los valores allí son para ilustrar este tutorial.

## Resultado

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)

*Traducido por openai.com*


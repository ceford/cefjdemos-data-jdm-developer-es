<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: Ejemplo de CLI - Onoffbydate -->

## Introducción

Hay ocasiones en que un sitio necesita mostrar u ocultar un módulo dependiendo de la fecha. Un ejemplo podría ser un módulo de HTML personalizado que muestre un mensaje durante el invierno. Otro podría ser la alternancia de módulos personalizados según el día de la semana, por ejemplo, uno para los días de semana y otro para los fines de semana. El ejemplo presentado aquí utiliza un complemento, un comando cli y un cron para lograr el efecto deseado.

El código está disponible en [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate)

Hay más artículos sobre el uso de la CLI en el [Manual del Usuario](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) y en la Documentación de Programadores de Joomla! [copia local](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) o [fuente original](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Estándares de Joomla

En versiones anteriores de Joomla, el sistema de plugins utilizaba una implementación del patrón Observable/Observer. Como resultado, cada plugin cargado registraba inmediatamente todos sus métodos públicos como observadores. Esto podría causar problemas.

Joomla 4 y versiones posteriores utilizan el paquete de Eventos del Framework de Joomla para manejar los Eventos de los complementos. Esto proporciona un mejor rendimiento y seguridad. En términos prácticos, significa que la estructura de archivos de un complemento de Joomla 4/5/6 es diferente de la estructura de complemento Legacy de versiones anteriores.

## La Estructura del Plugin

El complemento se llama **onoffbydate**. El siguiente diagrama esquemático muestra la estructura de archivos utilizada para el desarrollo local del complemento:

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## El archivo de manifiesto onoffbydate.xml

Este es el archivo de instalación, un elemento estándar para cualquier extensión.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- La línea **namespace** le dice a Joomla dónde encontrar el código con espacio de nombres para este plugin.
- La carpeta **language** se mantiene con el código del plugin.
- Se requiere un **updateserver** para cumplir con los requisitos de JED.
- No hay opciones de **configuración** para este plugin, pero podría cambiar en el futuro. Por ejemplo, el usuario podría preferir establecer los meses de invierno usando casillas en lugar de tenerlos codificados.

## Registro: services/provider.php

Este es el punto de entrada para el código del plugin.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Suscripción a eventos: src/Extension/Onoffbydate.php

Este es el lugar donde se registra el evento que activa el complemento y se establece la ubicación del código que manejará el evento.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

La clase **OnoffbydateCommand** se encuentra en la carpeta *src/Console* ya que los comandos integrados de la Consola de Joomla se encuentran en una carpeta Console. Podría haberse ubicado en una carpeta *Extension*.

## El archivo de comando: src/Console/OnoffbydateCommand.php

Este archivo contiene el código que realiza todo el trabajo para esta extensión.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- La función `configure` establece que se requieren tres argumentos de línea de comandos:
    - `season` debe ser uno de `weekend` o `winter`.
    - `action` debe ser uno de `publish` o `unpublish`
    - `module_id` debe ser el ID entero del módulo que se va a publicar o despublicar.
- La función `doExecute` es donde se realiza el trabajo. Se reconocen dos opciones de acción:
    - `winter` llama a una función para publicar o despublicar un módulo si hoy es en los meses de invierno.
    - `weekend` llama a una función para publicar o despublicar un módulo si hoy es un día de fin de semana.
- La función `publish` realiza una llamada a la base de datos para establecer el campo `publishes` del módulo a `0` o `1` según corresponda.
- Las funciones `winter` y `weekend` realizan algunos cálculos de fecha para determinar si hoy es en invierno (o no) o en un fin de semana (o no) para pasar los parámetros adecuados a la función de publicación.
- La llamada a la función `$this->ioStyle->success()` produce un mensaje de texto en la consola con texto negro y un fondo verde. `$this->ioStyle->error()` produce un mensaje de ERROR con texto blanco sobre un fondo marrón.

## archivos de idioma

Hay dos archivos de idioma utilizados durante la instalación y configuración del plugin. El archivo en-GB/plg_console_onoffbydate.sys.ini contiene las cadenas vistas durante la instalación y gestión:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

El archivo en-GB/plg_console_onoffbydate.ini contiene las cadenas que se ven en uso:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Nota que la salida del comando la ves solo tú en tu instalación de Joomla.

## El Módulo

Este código simplemente cambia el estado publicado de un módulo dependiendo de alguna función de la fecha. Por lo tanto, necesitas el id del módulo como se ilustra en la columna derecha de la página de lista de Módulos (Sitio).

## La Línea de Comandos

Instala el plugin en tu instalación de prueba de Joomla y actívalo. Si causa un fallo en tu sitio, usa phpMyAdmin para encontrar el plugin recién instalado en la tabla #__extensions y configura su parámetro habilitado a 0. Soluciona el problema e inténtalo de nuevo.

En una ventana de terminal, cambia el directorio a la raíz de tu sitio y usa el siguiente comando:

```sh
php cli/joomla.php list
```

Si algo sale mal en esta etapa, verifica que la versión de PHP invocada sea la versión de línea de comandos y no la utilizada por el servidor web. Puedes suprimir los mensajes de deprecación con esta versión de la línea de comandos.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Si el código funciona, verás `onoffbydate` entre la lista de comandos disponibles y puedes invocar la ayuda para ver cómo debe utilizarse:

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

Y luego simplemente pruébalo:

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### En uso

¡La lógica de uso es un poco ilógica! Si tienes un módulo que solo debe publicarse el fin de semana, los parámetros para usar todos los días son `weekend publish module_id`. Eso publica el módulo en los días de fin de semana y lo despublica en los días que no son de fin de semana. Para un módulo que necesita ser publicado en días laborables, los parámetros para usar todos los días son `weekend unpublish module_id`. Eso despublica el módulo en los días de fin de semana y lo publica en los días que no son de fin de semana. La misma lógica se aplica a la versión `winter`.

## El cron

El comando se puede probar en una ventana de terminal, pero probablemente quieras usarlo desde un cron. La opción `winter` podría ejecutarse el primer día de cada mes. La opción `weekend` se ejecutaría diariamente. Lo importante es que puedes tener tantos crons como necesites para cambiar el estado publicado de tantos módulos como desees en cualquier intervalo adecuado. El mismo código funciona para todos.

En un servicio de alojamiento necesitas proporcionar las rutas completas al ejecutable de php y al comando cli de joomla. Ejemplo:

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

Dependiendo de cómo hayas configurado tu cron y tu sistema, puedes recibir un correo electrónico tranquilizador que contenga exactamente la misma información que ves en la línea de comandos.

## Comprobar

Y, por supuesto: ve a tu página de inicio y verifica que el módulo realmente ha sido publicado o despublicado.

*Traducido por openai.com*


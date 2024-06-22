<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Creando un Plugin para Joomla -->

<span id="main-portal-heading">Tutorial
Cómo crear un Plugin para Joomla 4</span> Joomla!  4.x series

La estructura de los plugins para Joomla! 1.5, 2.5 y 3.x eran muy
flexibles y potentes. Los plugins no solo se pueden usar para manejar
eventos disparados por el núcleo de la aplicación y extensiones, sino
que también se pueden usar para hacer que las extensiones de terceros
sean extensibles y potentes. En Joomla 4.x hemos reescrito gran parte
del sistema de dispatcher para aumentar la flexibilidad aún más cuando
tu modificas los parámetros pasados como eventos mientras que
simultaneamente aumenta el rendimiento de los plugins.

Este "How-To" debe proveerte los conceptos básico que necesitas saber
para desarollar tu propio plugin. La mayoría de los plugins consiste de
un único archivo de código, pero para instalar correctamente el plugin
debe ser empaqueado en un archivo de instalación que pueda ser procesado
por el instalador de Joomla.

### Creando el Archivo de Instalación

Como todas las extensiones en Joomla, los plugins se instalan fácilmente
como un archivo .zip (.tar.gz también está soportado), pero se debe
incluir un archivo XML con el formato correcto.
Como ejemplo, aquí está el archivo de instalación XML para el plugin de
búsqueda de categorías:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

Como puedes ver, el sistema es similar al de los archivos XML de
instalación de Joomla. Sólo tienes que prestar atención a la entrada
`group="xxx"` en la etiqueta y en la información extendida en la
etiqueta . Esta información le dice a Joomla en qué carpeta copiar el
archivo y a que grupo se debe agregar el plugin.

Si estás creando un plugin que responde a eventos del núcleo, el
atributo `group="xxx"` debería ser cambiado para reflejar el nombre de
la carpeta plugins existentes para el tipo de evento que desea aumentar.
Ej: `group="authentication"` o `group="user"`. Ver
[Plugin/Events](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
para una lista completa de las categorías de eventos del núcleo. En la
creación de un nuevo plugin para responder a los eventos del núcleo es
importante que el nombre de tu plugin sea único y no entre en conflicto
con alguno de los otros plugins que también puedan responder al evento
del núcleo que quieras usar.

Si estás creando un plugin para responder a eventos que no son del
núcleo, tu elección para la etiqueta `group="xxx"` debería ser diferente
que cualquiera de las categorías del núcleo.

**CONSEJO:** *Si agregas el atributo `method="upgrade"` a la etiqueta
`extension`, este plugin puede ser instalado sin desinstalar una versión
anterior. Todos los archivos existentes se sobrescribirán, pero los
archivos antigüos no serán eliminados.*

### Creando el Plugin

En Joomla 4 también puedes tener el código de tu plugin con espacio de
nombres (namespace) - por favor leer [Namespace Conventions in Joomla
4](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "J4.x:Namespace Conventions In Joomla").
Note que el archivo de entrada principal no tiene espacio de nombres,
pero sí los campos y cualquier otro código de apoyo.

La forma orientada a objetos de escribir plugins implica la escritura de
una subclase <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, una clase base que implementa
las propiedades básica de los plugins. En sus métodos, las siguientes
propiedades están disponibles:

- `$this->params`: El
  [parámetro](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter")
  establecido para este plugin por el administrador
- `$this->_name`: el nombre del plugin
- `$this->_type`: el grupo (type) del plugin
- `$this->db`: el objecto db
- `$this->app`: el objeto de la aplicación

**CONSEJO:** *Para usar `$this->db` y `$this->app`, `CMSPlugin` prueba
si la propiedad existe y no es privada. Si se desea que se utilicen los
objetos predeterminados, cree propiedades sin instanciar en la clase del
plugin (ej: `protected $db; protected $app;` en la misma zona que
`protected $autoloadLanguage = true;`). Las propiedades no existirán a
menos que sean creadas explícitamente.*

En el siguiente código de ejemplo, representa el grupo (tipo) de plugin
y representa su nombre. Note que los nombres de clases y funciones en
PHP son insensibles a mayúsculas.

También implementamos el <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a> aquí, que es el mayor
cambio desde Joomla 1.5-3.x. En lugar de que el nombre de la función sea
detectada automáticamente y sea la misma que el nombre del evento
permite que tu tengas nombres personalizados de funciones. Esto nos
permite saber qué plugins y funciones están implementados y dado que el
análisis de métodos públicos en código PHP es lento, aumenta
significativamente el rendimiento.

Note que en toda la serie de Joomla 4 hay una capa obsoleta que cubrirá
plugins usando la vieja estrategia de nomenclatura con el nombre del
plugin igual que el nombre del evento cuando SubscriberInterface no está
implementado.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```
### Usando Plugins en Tú Código

Si va a crear un plugin para un nuevo evento, que no es del núcleo,
recuerda activar su plugin luego de instalarlo. Anteponga cualquier
referencia a su nuevo plugin con el comando
`JPluginHelper::importPlugin()`.

Ahora que ha creado tu plugin, probablemente quieras llamarlo en tu
código. Puede que no: el núcleo de Joomla tiene una serie de eventos
incorporados que es posible que desees registar en el código de tu
plugin. En este caso no necesitas hacer lo siguiente.

#### Nueva forma de Joomla 4

La nueva forma de hacer esto en Joomla 4 es obtener el dispatcher y
enviar un evento con nombre.

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

Si desea permitir que el usuario modifique valores, puede usar el
resultado del evento y obtener resultados de nuevo. Puedes mirar

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Por qué hemos introducido el nombre de clase por sobre parámetros?
Porque hace que sea más facil introducir setters y getters
personalizados para propiedades - actualmente un plugin puede incluso
cambiar completamente una propiedad según lo desee - para un componente
no hay manera de imponer alguna limitación. Adicionalmente esto hace
mucho más facil para los desarrolladores agregar y eliminar parámetros
en un evento sin tener mayores problemas (ya que ahora está llamando
métodos definidos y no está sujeto a que una propiedad sea el segundo
argumento de su función).

#### Cómo lograr la máxima compatibilidad con Joomla 3

Si quieres disparar un evento de forma similar al removido
JEventDispatcher de J3.x, utilice un código como este:

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

Es importante notar que los parámetros tienen que estar en un array. La
función del plugin en sí misma obtendrá los parámetros como un objecto
Event si este implementa la interfáz SubscriberInterface y como valores
individuales si no lo hace, pero este método siempre retorna un array
que devuelve el plugin.

Tenga en cuenta que si CUALQUIER plugin en un grupo no implementa la
interfáz SubscriberInterface la propiedad del resultado (como un
parametro con nombre y como resultado de un plugin) se usa como una
propiedad especial y no se puede usar.

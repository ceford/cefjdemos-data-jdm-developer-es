<!-- Filename: Manifest_files / Display title: Archivos de manifiesto -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5

Dentro de Joomla! hay archivos de manifiesto para todas las extensiones.
Estos archivos incluyen la información general de instalación, así como
los parámetros para la configuración de la
[extensión](https://docs.joomla.org/extension "Special:MyLanguage/extension").
Desde Joomla! 2.5, hay muy pocas diferencias de formato entre los
archivos de manifiesto para los diferentes [tipos de
extensiones](https://docs.joomla.org/Extension_types_(technical_definitions) "Special:MyLanguage/Extension types (technical definitions)"),
lo que permite a cada tipo acceder con todo el poder de al instalador de
Joomla!.

## Convenciones de nomenclatura

El archivo debe ser nombrado `manifest.xml` o `.xml` y deber ser ubicado
en el directorio raíz del paquete de la instalación.

Joomla 4.x: Automatic namespace mapping will fail if a manifest file
named `manifest.xml` is used. See:
<a href="https://github.com/joomla/joomla-cms/issues/37750"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/issues/37750</a>

## Sintaxis

### Elemento raíz

La etiqueta principal del archivo de instalación es:

Estas etiquetas, de apertura y cierre, ahora son válidos para todas las
extensiones. La nueva etiqueta sustituye a la antigua a partir de Joomla
<img src="https://docs.joomla.org/images/c/c8/Compat_icon_1_5.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 1.5" />. Se permiten los siguientes atributos
dentro de la etiqueta:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Atributo</th>
<th>Valores</th>
<th>Aplicable a</th>
<th>Descripción</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>component</code><br />
<code>file</code><br />
<code>language</code><br />
<code>library</code><br />
<code>module</code><br />
<code>package</code><br />
<code>plugin</code><br />
<code>template</code><br />
<code>element</code></td>
<td>Todas las extensiones</td>
<td>Este atributo describe el tipo de la extensión para el instalador.
Basado en este tipo, se aplican los requisitos adicionales de las
sub-etiquetas.</td>
</tr>
<tr class="even">
<td>version</td>
<td><code>2.5</code><br />
<code>3.0</code></td>
<td>Todas las extensiones</td>
<td><p>Cadena que identifica la versión de Joomla! para la cual la
extensión se desarrolla.</p></td>
</tr>
<tr class="odd">
<td>method</td>
<td><code>install</code><br />
<code>upgrade</code></td>
<td>Todas las extensiones</td>
<td>El valor predeterminado <code>install</code>, también se utilizará
si el atributo método no se utiliza. El valor <code>install</code>
significa que el instalador va a detenerse correctamente si se encuentra
cualquier archivo/carpeta existente de la nueva extensión.</td>
</tr>
<tr class="even">
<td>client</td>
<td><code>site</code><br />
<code>administrator</code></td>
<td>Módulos</td>
<td>El atributo cliente permite especificar para qué cliente de la
aplicación está disponible el nuevo módulo.</td>
</tr>
<tr class="odd">
<td>group</td>
<td><em>string</em></td>
<td>Plugins</td>
<td>El nombre del grupo especifica para qué grupo de plugins el nuevo
plug-in está disponible. Los grupos existentes son los nombres de las
carpetas dentro del directorio <code>/plugins</code>. El instalador
creará los nuevos nombres de carpetas de nombres de grupos que todavía
no existen.</td>
</tr>
</tbody>
</table>

### Metadatos

Los siguientes elementos pueden ser utilizados para insertar metadatos.
Ninguno de estos elementos son necesarios; si están presentes, deben ser
un hijo del elemento raíz.

     – raw component name (e.g. com_banners).
     – author's name (e.g. Joomla! Project)
     – date of creation or release (e.g. April 2006)
     – a copyright statement (e.g. (C) 2005 - 2011 Open Source Matters. All rights reserved.)
     – a license statement (e.g. NU General Public License version 2 or later; see LICENSE.txt)
     – author's email address (e.g. admin@joomla.org)
     – URL to the author's website (e.g. www.joomla.org)
     – the version number of the extension (e.g. 1.6.0)
     – the description of the component. This is a translatable field. (e.g. COM_BANNERS_XML_DESCRIPTION)
     – the internal name of the component. If omitted, name will be cleaned and used

Nota: Las etiquetas y también son campos traducibles, de modo que el
nombre y la descripción de la extensión puede ser mostrado el usuario en
su idioma nativo.

### Archivos Lado Cliente

```xml
	<files folder="from-folder">
		<filename>example.php</filename>
		<folder>examples</folder>
	</files>
```

Los archivos a copiarse en el directorio del Lado Cliente se deben
colocar en el directorio . Puedes utilizar el atributo opcional
`folder`, para especificar un directorio **en el paquete ZIP** para
copiar **desde**. Cada archivo a copiar debe estar representado por un .
Si deseas copiar una carpeta entera a la vez, se puede definir como un .

Para **plugins**, el nombre simple del plugin debe ser colocado en el
atributo `plugin` del elemento que apunta al archivo que contiene la
clase del plugin. Por ejemplo, en el caso de un plugin del sistema
"ejemplo" (nombre completo `plg_system_ejemplo`), usar `ejemplo.php`.

### Archivos multimedia

```xml
	<media folder="media" destination="com_example">
		<filename>com_example_logo.png</filename>
		<folder>css</folder>
		<folder>js</folder>
	</media>
```

En este ejemplo se va a copiar el(los) archivo(s)
(`/media/com_example_logo.png`) y carpetas ( `/media/css/` y
`/media/js/` ) enumerados, a `/media/com_example/`, creara la carpeta
`com_example`, si es necesario. Puedes utilizar el atributo opcional
`folder` para especificar un directorio **en el paquete ZIP** para
copiar **desde** (en este caso, `media`).

Las extensiones deben almacenar accesorios que necesitan para ser
accesibles desde la web (JS, CSS, imágenes, etc) en `media`. Entre otras
cosas, esta característica se agregó como paso en la progresión del
apoyo multi-sitio y el eventual traslado de los archivos de código (PHP)
de la web de las áreas de acceso al servidor.
Nota: la sección `media` no se analiza si el 'paquete' es del tipo
extensiones.

Ref:

- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/4CAASJqFY-k/PvPj14gP29EJ"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>
- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/uNmhX98sKbE/p8p68Jke680J"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>

### Sección de administración

```xml
	<administration>
		<!-- various elements -->
	</administration>
```

La sección de administración se define en el elemento . Ya que sólo se
aplica para
[componentes](https://docs.joomla.org/Component "Special:MyLanguage/Component"),
tanto para el
[administrador](https://docs.joomla.org/Site_(Application) "Special:MyLanguagehttps://docs.joomla.org/Administrator_(Application)"),
**sólo en componentes el manifiesto puede incluir este elemento**.

#### Archivos del Lado Servidor

Archivos a copiar al directorio del Lado Servidor debe ser colocado en
el elemento dentro de . Puedes utilizar el atributo opcional `folder`
para especificar un directorio **en el paquete ZIP** para copiar
**desde**. Consulta **Archivos lado Cliente**(más arriba) para otras
reglas.

#### Enlaces de menú y submenús

**Nota de la versión:** Antes de Joomla! 3.4, no tener una etiqueta

en el archivo de manifiesto XML conducía a un elemento del menú que se
está creado. Este error fue corregido en Joomla! 3.4, por lo que si no
hay etiqueta en el archivo de manifiesto, entonces no hay elemento de menú del
administrador creado para el componente.

```xml
	<menu>COM_EXAMPLE</menu>
	<submenu>
		<!--
			Note that all & must be escaped to &amp; for the file to be valid
			XML and be parsed by the installer
		-->
		<menu link="anoption=avalue&amp;anoption1=avalue1">COM_EXAMPLE_SUBMENU_ANOPTION</menu>
		<menu view="viewname">COM_EXAMPLE_SUBMENU_VIEWNAME</menu>
	</submenu>
```

El texto para el elemento del menú principal del componente es definido
en el elemento `<menu>`, un hijo de `<administration>`. Un elemento
también pueden estar presente (también un
hijo de `<administration>`), que puede contener más elementos de menú definidos por `<menu>`.

Además, cada uno de los elementos `<menu>` pueden definir los siguientes atributos:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Atributo</th>
<th>Descripción</th>
</tr>
&#10;<tr class="odd">
<td>link</td>
<td><p>Un enlace para enviar al usuario cuando se hace clic en el
elemento de menú</p></td>
</tr>
<tr class="even">
<td>img</td>
<td>La ruta de acceso (relativa) a una imagen (16x16 píxeles) que
aparece junto al elemento del menú.
<p><u>Debe ser una url compatible como un archivo (por ejemplo, sin
espacios) !</u></p></td>
</tr>
<tr class="odd">
<td>alt</td>
<td></td>
</tr>
<tr class="even">
<td>view</td>
<td><p>Un parámetro de URL para añadir el enlace. Por ejemplo,</p>
COM_EXAMPLE
en el manifiesto de com_example haría que la dirección URL del elemento
de menú sea
<code>índex.php?option=com_example&amp;view=cpanel</code>.</td>
</tr>
</tbody>
</table>

El valor dentro de la etiqueta es la etiqueta del menú. A diferencia de
Joomla! 1.5, no puedes usar el lenguaje natural en una cadena. Por
ejemplo, si quieres entrar "Componente de Ejemplo" en lugar de
COM_EXAMPLE, que se traduciría en el nombre de tu componente que
aparecería como componente-de-ejemplo en el menú y que sería incapaz de
proporcionar una traducción. Con el fin de proporcionar una traducción,
necesitas crear un archivo llamado `en-GB.com_example.sys.ini` en
`administrador/languages/en-GB` (se puede utilizar la etiqueta del
manifiesto para hacer la copia durante la instalación) o en
`administrator/components/com_example/language/en-GB`. En el último
caso, no debes incluir el archivo de traducción en la etiqueta .
Mientras tengas colocado el directorio del idioma en tu etiqueta , será
copiado igualmente cuando el componente sea instalado.

El contenido de ese archivo debe ser:

```ini
    COM_EXAMPLE="Example Component"
    COM_EXAMPLE_SUBMENU_ANOPTION="Another Option"
    COM_EXAMPLE_SUBMENU_VIEWNAME="Another View"
```

Please note that the language string must be enclosed in double quotes,
as per Joomla!'s translation standards.

Joomla! 1.6 and later sorts the Component menu items based on the actual
translation of the key you supply in your XML manifest. This means that
the sorting order is correct no matter what you call your translation
key and no matter which language the site is being displayed in.
Essentially, Joomla! 1.6 fixed the wrong sorting of the Components menu
experienced under Joomla! 1.5 for the majority (non-English speaking!)
of Joomla! users.

### Dashboards

Specifies the details for displaying a dashboard for the component in
the Administrator area for the site.

- It will make a dashboard icon appear next to the administrator menu
  item for the component
- The dashboard icon will click through to display modules assigned to
  the cpanel-example administrator module position
- The title and icon defined in the XML file will be used as the header
  and icon at the top of the component's dashboard page.

```xml
  <dashboards>
		<dashboard title="COM_EXAMPLE_DASHBOARD_TITLE" icon="icon-lock">example</dashboard>
	</dashboards>
```

### Configuración

<img
src="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/25px-Stop_hand_nuvola.svg.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/38px-Stop_hand_nuvola.svg.png 1.5x, https://docs.joomla.org/images/4/41/Stop_hand_nuvola.svg.png 2x"
data-file-width="40" data-file-height="40" width="25" height="25"
alt="Stop hand nuvola.svg.png" />Warning!

El elemento , un hijo del raíz, describe las opciones de configuración
de la extensión. Si procede, las opciones se mostrarán en Gestor
apropiado (Gestor de plugins, Módulos o Plantillas). **Opciones de
configuración también se pueden definir en un archivo independiente
denominado `config.xml`. Su elemento raíz debe ser .**

Cada conjunto de campos debe contener uno o más elementos , cada uno
representando a un solo [campo de
formulario](https://docs.joomla.org/form_field "Special:MyLanguage/form field")
con una etiqueta. Mira [Tipos estándar de campos de
formulario](https://docs.joomla.org/Standard_form_field_types "Special:MyLanguage/Standard form field types")
para una lista de los tipos de campo de formulario permitidos y un
ejemplo de definiciones XML, de campos de formulario.

### Namespace

Specify the namespace to be used for autoloading the plugin. The given
namespace will autoload to the root directory of your extension by
default however you can optionally add a "path" attribute to the
namespace element to specify a subpath within your extensions root
directory.

### SQL

```sql
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.install.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.uninstall.sql</file>
        </sql>
    </uninstall>
```

En el ejemplo de arriba, ponemos los archivos de SQL en la carpeta
`admin/sql` del paquete de instalación. Tienes que incluir la carpeta
`sql` en los archivos de administración (como se describe en *Archivos
del Lado Servidor*).

Puede ejecutar SQL durante la instalación y/o desinstalación mediante
los elementos y , respectivamente. Un elemento debe aparecer como un
hijo de estos elementos. puede contener cualquier número de elementos ,
cada uno de define un único archivo SQL a ejecutar. Tu tipo de
controlador de base de datos se debe describir en el atributo `driver`,
tus conjuntos de caracteres con el atributo `charset`.

#### Esquema de actualización SQL

Desde 1.6, también hay una etiqueta , que te permite ofrecer una serie
de archivos SQL para actualizar el esquema actual.

```sql
	<update>
		<schemas>
			<schemapath type="mysql">sql/updates/mysql</schemapath>
			<schemapath type="sqlsrv">sql/updates/sqlsrv</schemapath>
		</schemas>
	</update>
```

Por ejemplo, con el fin de pasar de la versión `1.0.0` a la versión
`1.0.1` en una base de datos **MySQL**, un archivo `1.0.1.sql` debe ser
creado dentro de la carpeta `sql/updates/mysql` y la etiqueta del
manifiesto debe ser actualizada a este valor

```sql
<version>1.0.1</version>
```

La estructura final de la carpeta sql se parecerá a esto (suponiendo una
base de datos **MySQL**)

    sql
     |-->example.install.sql
     |-->example.uninstall.sql
     |-->updates
         |-->mysql
            |-->1.0.1.sql

Archivos similares deben ser creados para versiones posteriores.

### Archivos de idioma

En Joomla! 1.5, los desarrolladores de extensiones tenían que poner los
archivos de idioma de la extensión en la carpeta principal de idioma de
Joomla!, utilizando la etiqueta como se muestra a continuación. **Este
método puede utilizarse en Joomla!**.

```xml
<!-- Joomla! language tag -->
<languages folder="langfiles">
	<language tag="en-GB">en-GB.com_example.ini</language>
</languages>
```

Desde Joomla! 1.6 se ha alentado a colocar los archivos de idioma de la
extensión en la carpeta de la extensión. Joomla! entonces
automáticamente carga los archivos de idioma de la carpeta de la
extensión.

Mediante el almacenamiento de los archivos de idioma de la extensión en
la carpeta de la extensión, se tiene la ventaja de aislar y proteger los
archivos de idioma de tu extensión. Por ejemplo, un administrador
elimina un idioma de su instalación Joomla!. La los archivos de idioma
de la extensión no serán eliminados. Se mantendrán en su lugar y estará
disponibles si el idioma se instala de nuevo.

La estructura de la carpeta de idioma para el Lado Cliente y el Lado
Servidor es la misma. Se pone la etiqueta de idioma (por ejemplo,
**es-ES** ) de cada idioma en la carpeta de su idioma es decir,
**language/es-ES/**. Tienes que especificar las carpetas en lso archivos
del Lado Cliente y Lado Servidor.

En el manifiesto, sólo tienes que incluir la carpeta **language** en la
sección , se copiaran al sub-directorio de cada idioma automáticamente.
Dentro del grupo simplemente agrega un elemento junto con los elementos
en grupo ', como se muestra en este ejemplo:

```xml
<files>
	<filename plugin="alpha">alpha.php</filename>
	<folder>sql</folder>
	<folder>language</folder>
</files>
```

También es notable el hecho de que ambas formas pueden trabajar juntos.
Aquí es un ejemplo con el núcleo:

```xml
<files>
	<filename plugin="languagecode">languagecode.php</filename>
	<filename>index.html</filename>
	<folder>language</folder>
</files>
<languages>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.ini</language>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.sys.ini</language>
</languages>
```

Las ventajas de esta solución son las siguientes:

Todos ini archivos presentes en la carpeta del núcleo tienen precedencia
sobre los archivos en la carpeta *language/* de la extensión. Por
ejemplo, un archivo sys.ini siempre estará cargado de las carpetas del
núcleo en el Lado Servidor, si es que existe, excepto cuando se instala
una extensión que contiene un archivo sys.ini en la carpeta de idioma.
En ese caso y sólo en ese caso, el archivo sys.ini en la carpeta de la
extensión mostrará su contenido traducido en el momento de la
instalación. Esto es muy útil ya que un desarrollador puede tener 2
archivos sys.ini con diferentes contenidos. Una descripción de la
correcta instalación, así como un tutorial en el Lado Servidor, por
ejemplo.

También, es mucho más fácil para un usuario que necesita un archivo ini
para una extensión que no se ofrece en el idioma deseado, agregarlo en
las carpetas principales. No hay riesgo que sea eliminado en caso de
desinstalar la extensión por error o por cualquier otra razón.

Mira también:

- [paquetes de idioma que no son del
  núcleo](https://docs.joomla.org/J2.5:Making_non-core_language_packs "Special:MyLanguage/J2.5:Making non-core language packs")
- [Creación de paquetes de idioma para las extensiones en Joomla
  2.5](https://docs.joomla.org/Creating_language_packs_for_extensions_in_Joomla_2.5 "Special:MyLanguage/Creating language packs for extensions in Joomla 2.5")

Durante el desarrollo se puede habilitar la depuración den idioma en la
configuración global de Joomla!. Así que puedes investigar si surge un
problema. Como desde 3.2, esto es necesario para ayudar a depurar en-GB,
**siempre** carga la primera vez, cuando no está en modo de depuración
para evitar mostrar Constantes.

### Archivo script

```xml
    <scriptfile>example.script.php</scriptfile>
```

Un **archivo script** opcional (código PHP que se ejecuta antes, durante
y/o después de la instalación, desinstalación y actualización) se puede
definir mediante un elemento . Este archivo debe contener una clase
denominada "InstallerScript" donde es el nombre de la extensión (por
ejemplo, com_componentname, mod_modulename, etc.). Los plugins requieren
declarar el grupo (por ejemplo, plgsystempluginname). Los paquetes de
biblioteca no son compatibles scriptfiles. La estructura de la clase es
la siguiente:

This file should contain a class named "InstallerScript" where is the
name of your extension (e.g. com_componentname, mod_modulename, etc.).
Plugins must state the group (e.g. plgsystempluginname).

In <img src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> and later the structure of the class is
as follows:

```php
<?php

use Joomla\CMS\Installer\InstallerAdapter;

class com_componentnameInstallerScript
{
	/**
	 * Constructor
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function __construct(InstallerAdapter $adapter)
	{
	}

	/**
	 * Called before any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function preflight($route, InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called after any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function postflight($route, $adapter)
	{
		return true;
	}

	/**
	 * Called on installation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function install(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on update
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function update(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on uninstallation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function uninstall(InstallerAdapter $adapter)
	{
		return true;
	}
}

?>
```


Note that since Joomla 3.6 Joomla has shipped a basic script that you
can use instead of shipping your own from scratch **JInstallerScript**
which contains various helper methods commonly used through the
community.

### Library Manifests

A simple library manifest might look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="library" method="upgrade" version="4.0">
    <name>My Test library.</name>
    <libraryname>mytest</libraryname>
    <files>
        <folder>Classes</folder>
        <folder>language</folder>
        <filename>mytest.php</filename>
    </files>
</extension>
```

This will install the library into the `JPATH_SITE/libraries/mytest`
folder.

What if your company has several libraries and you want to group them
together under folder `JPATH_SITE/libraries/mycompany`?

Simple - include your company name in the `libraryname` property of each
library like this:

```xml
    <libraryname>mycompany/mylibrary1</libraryname>
```

```xml
    <libraryname>mycompany/mylibrary2</libraryname>
```

These libraries will then be installed in the
`JPATH_SITE/libraries/mycompany/mylibrary1` and
`JPATH_SITE/libraries/mycompany/mylibrary2` folders.

Uninstalling `mylibrary1` will still leave `mylibrary2` installed on
your site.

When using `script files` with such company libraries the installer
class name should look like this:

```php
    class mycompanymylibrary1InstallerScript
```

```php
    class mycompanymylibrary2InstallerScript
```

### Servidores de actualización

```xml
    <updateservers>
        <server type="extension" priority="1" name="Extension Update Site">http://example.com/extension.xml</server>
        <server type="collection" priority="2" name="Collection Update Site">http://example.com/collection.xml</server>
    </updateservers>
```

Los servidores de actualización pueden ser definidos en el elemento , un
hijo del raíz. Este elemento puede contener uno o más elementos , cada
uno describiendo una ubicación para obtener las actualizaciones. Cada
elemento puede definir los siguientes atributos:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Atributo</th>
<th>Valores</th>
<th>Descripción</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>extension</code><br />
<code>collection</code></td>
<td>El tipo de servidor de actualización</td>
</tr>
<tr class="even">
<td>priority</td>
<td><em>integer</em></td>
<td>La prioridad del servidor de actualización</td>
</tr>
<tr class="odd">
<td>name</td>
<td><em>string</em></td>
<td>El nombre del servidor de actualización</td>
</tr>
</tbody>
</table>

Más información:

- [Construir una extensión Joomla! 2.5 - Agregar un servidor de
  actualización](https://docs.joomla.org/J2.5:Developing_a_MVC_Component/Adding_an_update_server "Special:MyLanguage/J2.5:Developing a MVC Component/Adding an update server")
- [Gestión de Actualizaciones de Componentes en Joomla!
  2.5](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")

### Supporting Download Keys

As of Joomla 4.0.0 users can enter their download keys in the Update
Sites list. This gives them a single place to manage the download keys.
When a user is going to update an extension, Joomla will check if there
is a download key. If there is a download key, Joomla will add the
download key to the update URL.

To support download keys you must include the `dlid` tag in the manifest
file. The `dlid` tag takes 2 arguments:

- prefix
- suffix

The `dlid` tag will look like this in your manifest file:

```xml
<dlid prefix="dlid=" suffix="&amp;dummy=my.zip"/>
```

The prefix will be added before the download key and the suffix after
the download key. Using the example above the full query added to the
download link will be:

    dlid=KEY&dummy=my.zip

The key is added before the `onInstallerBeforePackageDownload` event is
triggered, so the full URL will be passed to the event.

## Ejemplos

Para un ejemplo de la vida real, ver <a
href="https://github.com/joomla/joomla-cms/blob/staging/administrator/components/com_banners/banners.xml"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">el manifiesto del componente Anuncios
de la última versión de Joomla! 3.6.4</a>.

Se pueden encontrar más ejemplos en el repositorio de enlaces web
independientes:

- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/administrator/components/com_weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">com_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/modules/mod_weblinks/mod_weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">mod_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/plugins/search/weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">plg_search_weblinks manifest</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/templates/protostar/templateDetails.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">tpl_protostar manifest (3.6.4)</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/administrator/language/en-GB/en-GB.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">en-GB manifest (3.6.4)</a>

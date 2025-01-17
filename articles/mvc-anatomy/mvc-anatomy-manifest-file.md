<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: MVC Anatomía: Archivo de manifiesto -->

## Metadatos

El archivo de manifiesto del componente debe llamarse manifest.xml o componentname.xml, en este caso countrybase.xml. Tenga en cuenta que la parte com_ no está incluida. La primera parte del archivo contiene metadatos:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

La mayoría de los metadatos son evidentes por sí mismos. Elementos a destacar:

- El tipo de extensión en este caso es **componente**. Hay otros tipos de extensiones, como módulo o plugin.
- El método puede ser install o upgrade. Este último permite la reinstalación después de la actualización del código.
- ¡El número de versión es importante! Debe hacer imposible reinstalar una versión anterior. Y se utiliza para controlar si se requieren actualizaciones de la base de datos.
- La ruta del espacio de nombres src tiene tres partes:
  - La primera parte es un prefijo definido por el proveedor, así que Joomla para el código proporcionado por Joomla, Acme para el código proporcionado por el proveedor de extensiones de terceros Acme, y en este caso Cefjdemos, un nombre elegido para demostraciones de código de Joomla.
  - La segunda parte indica el tipo de extensión. Podría ser Componente o Módulo o Plugin o ...
  - La tercera parte es el nombre específico de la extensión, en este caso Countrybase.

## Base de datos

El archivo de manifiesto define las ubicaciones de cualquier archivo sql de instalación, actualización o eliminación. Terminan en una carpeta sql dentro de la carpeta del administrador.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Archivo de Script

Un archivo de script puede utilizarse para propósitos de pre-vuelo o post-vuelo. Por ejemplo, podría usarse para verificar los requisitos mínimos del sistema antes de la instalación o para establecer parámetros después de la instalación.

## Archivos Multimedia

El componente com_countrybase no necesita ningún css o javascript específico del componente, pero el código para instalarlos está incluido por si acaso. Las carpetas css y js solo contienen archivos index.html vacíos. Sin estos archivos, las carpetas no se crean.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Tenga en cuenta que el archivo joomla.asset.json es utilizado por el gestor de activos web, generalmente llamado desde un archivo de plantilla.

## Archivos del Sitio

Estos son los archivos que se copian a root/components/com_countrybase. Tenga en cuenta que la carpeta de idioma se copia en la carpeta del componente y no en la carpeta root/language:

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Archivos del Administrador

Hay más archivos en la parte de administración del archivo de manifiesto porque tiene más que hacer:

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Nota el método para crear los elementos del menú del Administrador de Joomla. Hay un elemento de menú que no tiene enlace. Y dos elementos de menú que enlazan con las dos vistas de administración disponibles: la lista de Países y la lista de Monedas. También las traducciones de las claves de cadena deben estar en el archivo com_countrybase.sys.ini.

## Registro de cambios

El registro de cambios se utiliza para mantener un registro de los cambios con cada versión de la extensión. Consulte el artículo de [Registros de cambios](jdocmanual?article=docus/install-update/installation-change-log) para obtener información sobre el contenido del registro de cambios.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Actualizar Servidor

El código del servidor de actualización le indica a Joomla dónde encontrar información de actualización. Se ejecuta a intervalos regulares para verificar si hay una actualización disponible. Omite esta sección si no estás utilizando un servidor de actualización para tu propio componente.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Traducido por openai.com*


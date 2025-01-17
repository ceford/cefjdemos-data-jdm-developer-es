<!-- Filename: https://docs.joomla.org/Package / Display title: Paquetes -->

## Introducción a los paquetes

A veces, un módulo (o plugin u otra extensión) utiliza modelos o ayudantes de un componente. En estas ocasiones es mejor poder instalar o desinstalar un módulo dependiente cuando el componente en sí mismo es instalado o desinstalado. En Joomla, esta funcionalidad se logra con un paquete, aunque el uso de paquetes no es infalible.

Un paquete es una extensión que se utiliza para instalar múltiples extensiones al mismo tiempo. Combinarlas en un paquete permite al usuario instalar y desinstalar dos o más extensiones en una sola operación.

## Creación de Paquetes

Un paquete se crea comprimiendo todos los archivos individuales de extensión `.zip` junto con un archivo de manifiesto `.xml`. Por ejemplo, para un paquete compuesto de:

* **componente** helloworld
* **módulo** helloworld
* **biblioteca** helloworld
* **plugin del sistema** helloworld
* **plantilla** helloworld

El paquete tendría el siguiente árbol en el archivo zip:

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

El `pkg_helloworld.xml` podría tener el siguiente contenido:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

Cuando se comprime e instala, el paquete será visible en la lista de extensiones para que un usuario pueda desinstalar todas las extensiones contenidas en el paquete.

Recuerde usar el desinstalador del paquete en lugar de los desinstaladores de subpaquetes individuales para evitar entradas de extensiones huérfanas en el gestor de extensiones. ¡Esta es la parte que no es infalible!

### Id. de archivo

El elemento id en la etiqueta `<file ..>` **no** es arbitrario. El `id=` debe establecerse al valor de la columna `element` en la tabla `#__extensions`. Si no se establecen correctamente, al desinstalar el paquete, el `file` hijo **no** se encontrará y no se desinstalará.

### Nombre del Archivo de Manifiesto y Nombre del Paquete

El nombre del archivo de manifiesto y la capacidad de desinstalar el archivo del paquete en sí están estrechamente relacionados. El archivo de manifiesto debe tener un prefijo **pkg_**. El resto del nombre del manifiesto (sin la extensión **.xml**) se debe usar como `<packagename>`. O, al revés, un paquete que deseas identificar como **blurpblurp_J3** obtiene eso como su `<packagename>` y debe estar en un archivo de manifiesto llamado **pkg_blurpblurp_J3.xml**. No hacerlo hará imposible desinstalar el paquete en sí.

Un `<pkg_script.php>` opcional que contiene una clase `pkg_<packagename>InstallerScript` con la función pública **postflight** puede ser utilizado.

### Etiqueta de extensión

La etiqueta `<extension>` debe incluir `method="upgrade"` para que una actualización del paquete tenga éxito en instalaciones posteriores.

### Desinstalación de Dependencias de Extensión

Una extensión de paquete puede declarar que sus elementos secundarios no pueden desinstalarse de forma independiente con un elemento `<blockChildUninstall>true</blockChildUninstall>` en el manifiesto del paquete. Si tu paquete necesita todas sus extensiones para funcionar de manera efectiva, configúralo en verdadero o 1.

*Traducido por openai.com*


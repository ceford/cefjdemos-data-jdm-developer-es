<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Añadiendo un Registro de Cambios -->

Desde Joomla 4.0, los desarrolladores de extensiones pueden aprovechar la capacidad de Joomla para leer un archivo de registro de cambios y ofrecer una representación visual del mismo. Si una versión dada no se encuentra en el registro de cambios, el botón de registro de cambios no se mostrará.

Los cambios en una versión se presentarán de esta manera:

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

El registro de cambios se utiliza en dos lugares diferentes.

## Actualizar vista

El instalador mostrará el registro de cambios de la versión que se puede instalar, si está disponible.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

Hacer clic en el botón de Registro de cambios aquí mostrará el registro de cambios de la nueva versión disponible.

## Administrar vista

El administrador de extensiones mostrará el registro de cambios de la extensión actualmente instalada, si está disponible.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Al hacer clic en el número de versión aquí se mostrará el registro de cambios de la versión instalada actual.

## Agregar etiqueta changelogurl a los archivos de manifiesto

El primer paso es actualizar tus archivos de manifiesto que le dicen a Joomla dónde encontrar los detalles del registro de cambios. Agrega el siguiente nodo a tus archivos de manifiesto XML:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Tenga en cuenta: La URL en la etiqueta changelogurl no debe tener espacios o saltos de línea antes o después de ella. Vea los ejemplos de código.

### Actualizar el manifiesto del servidor

Vea este ejemplo de un archivo de manifiesto del servidor de actualización que informa a Joomla sobre una actualización de un componente llamado "com_lists". Así verá el botón de Registro de cambios en la vista de actualización.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Manifiesto de extensión

Además, añade la etiqueta changelogurl al manifiesto XML de la extensión. De esta manera, la versión de la extensión estará vinculada a los registros de cambios en la vista de gestión.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Crear archivo de registro de cambios

El archivo de cambios debe tener los siguientes 3 nodos:

* elemento
* tipo
* versión

Esta información se utiliza para identificar el registro de cambios correcto para una extensión determinada.

Un nodo de versión dentro de cualquier nodo de registro de cambios es siempre obligatorio. De lo contrario, verá un mensaje de error como SyntaxError: JSON.parse: caractere inesperado en la línea 1 columna 1 de los datos JSON.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Además, el registro de cambios se llena con uno o más tipos de cambios. Se admiten los siguientes tipos de cambios:

* seguridad: Cualquier problema de seguridad que haya sido solucionado
* corrección: Cualquier error que haya sido corregido
* lenguaje: Esto es para cambios de idioma
* adición: Cualquier nueva función añadida
* cambio: Cualquier cambio
* eliminación: Cualquier función eliminada
* nota: Cualquier información adicional para informar al usuario

Cada nodo se puede repetir tantas veces como sea necesario.

El formato del texto puede ser texto plano o HTML, pero en el caso de HTML, debe estar encerrado en etiquetas CDATA como se muestra en el ejemplo.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Este archivo contiene 2 registros de cambios:

* Versión 0.0.2 (para probar la vista de gestión)
* Versión 4.0.0 (para probar la vista de actualización)

Un registro de cambios puede tener tantas versiones como sea necesario.

*Traducido por openai.com*


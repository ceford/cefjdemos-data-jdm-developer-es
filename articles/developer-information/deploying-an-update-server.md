<!-- Filename: Deploying_an_Update_Server / Display title: Implementación de un Servidor de Actualización -->

Joomla!  2.5 Joomla!  3.x Joomla!  4.x

## Introducción

Este tutorial está diseñado para enseñar a los desarrolladores cómo
crear un servidor de actualización para la integración con el sistema de
actualización se introdujo en Joomla!. Mediante la adición de una lista
del servidor de actualización al manifiesto de la extensión, los
desarrolladores permiten así a los usuarios actualizar sus extensiones a
través de la vista del Administrador de Actualización Extensiones (ver
las pantallas de ayuda para [Joomlaǃ
3.x](https://docs.joomla.org/Help31:Extensions_Extension_Manager_Update "Special:MyLanguage/Help31:Extensions Extension Manager Update"))
con sólo unos pocos clics.

## Definición de un servidor de actualización

Para utilizar esta característica, un servidor de actualización debe ser
definido en el manifiesto de su extensión. Esta definición puede ser
utilizada en todas las extensiones compatibles con Joomla! 2.5 y
posteriores, pero no está disponible para las plantillas. Puede utilizar
dos opciones para el tipo de servidorː colección o extensión. Esto será
explicado en detalle en breve. Este código debe ser añadido al archivo
de manifiesto de la extensión, dentro del elemento raíz de la
"extensión". El servidor de actualización se define de la siguiente
manera para cada tipo:

```xml
<extension>
<...>
<updateservers>
  <server type="collection">https://example.com/list.xml</server>
  <server type="extension" priority="2" name="My Extension's Updates">http://example.com/extension.xml</server>
</updateservers>
</extension>
 ```

Se pueden definir varios servidores dentro de la etiqueta . Si tiene más
de un servidor de actualización, puede establecer una prioridad
diferente para cada uno. De esa manera, puede controlar el orden en que
se verifican los servidores de actualización.

## Tipos de servidor

### Colección

El tipo de servidor `"collection"` permite a los desarrolladores definir
el manifiesto de una extensión para extraer actualizaciones de una
colección. Este tipo de servidor se puede utilizar si el desarrollador
quiere definir todas las actualizaciones de su extensión en un solo
archivo (no recomendado) o si su extensión tiene múltiples
sub-extensiones que no se distribuyen o actualizan al mismo tiempo (como
un paquete tipo de extensión). El siguiente ejemplo es la definición de
`"collection"` utilizada por el actualizador al procesar las
actualizaciones del núcleo de Joomla!:

```xml
<extensionset name="Joomla Core" description="Joomla! Core">
    <extension name="Joomla" element="joomla" type="file" version="1.7.0" detailsurl="https://update.joomla.org/core/extension.xml"/>
</extensionset>
```

Todas las definiciones se definen entre las etiquetas en el manifiesto
de la colección. La etiqueta tiene dos parámetros opcionales; *name* y
*description*. Para cada extensión a la que esta colección hace
referencia, es necesaria una etiqueta independiente . La etiqueta tiene
los siguientes parámetros, los cuales son necesarios para que las
actualizaciones se procesen correctamente:

- **name** - El nombre de la extensión
- **element** - el nombre no traducible de la extensión por ej.ː
  mod_custom
- **type** - El tipo de extensión (componente, módulo, plugin, etc.)
- **version** - La última versión de la extensión
- **detailsurl** - La dirección URL del archivo XML que contiene las
  definiciones individuales de actualización de la extensión

### Extensión

El servidor de tipo `"extension"` permite a los desarrolladores definir
un manifiesto de una extensión para extraer las actualizaciones desde un
único manifiesto de la extensión. Todos los manifiestos de colección
eventualmente apuntan a este archivo XML. Todas las actualizaciones de
este archivo deben definirse después de una etiqueta al principio del
archivo. El ejemplo siguiente es la definición de actualización de
Joomla! Versión 3.9.6:

```xml
<updates>
	<update>
		<name>Joomla! 3.9</name>
		<description>Joomla! 3.9 CMS</description>
		<element>joomla</element>
		<type>file</type>
		<version>3.9.6</version>
		<infourl title="Joomla!">https://www.joomla.org/announcements/release-news/5765-joomla-3-9-6-release.html</infourl>
		<downloads>
			<downloadurl type="full" format="zip">https://downloads.joomla.org/cms/joomla3/3-9-6/Joomla_3.9.6-Stable-Update_Package.zip</downloadurl>
			<downloadsource type="full" format="zip">https://github.com/joomla/joomla-cms/releases/download/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
			<downloadsource type="full" format="zip">https://update.joomla.org/releases/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
		</downloads>
		<tags>
			<tag>stable</tag>
		</tags>
		<sha256>05157273aadd3045564ee44373ea3643b437fa5321d17993a3119b38b04578e2</sha256>
		<sha384>ebd9b0666fbe84e20a420a4bcd6c10d306fc4dee4edbbe8e2133c85f0fb84e59be5a50aa97cb38c068b77f77f6bbc091</sha384>
		<sha512>a4c47644ceeaeec28944e0c74160203cf12037e0ea1439022e95055dfb6716de172667ce6d9164f12bb519d9cfcf1fdc728abea00f853b41debc7d2740f2b711</sha512>
		<maintainer>Joomla! Production Department</maintainer>
		<maintainerurl>https://www.joomla.org</maintainerurl>
		<section>STS</section>
		<targetplatform name="joomla" version="3.[789]" />
		<php_minimum>5.3.10</php_minimum>
</update>
</updates>
```

La siguiente sección describe los elementos de una única entidad de
actualización

- **name** - El nombre de la extensión, este nombre aparecerá en la
  columna Nombre de la Extensión de la vista del Gestor de
  Actualizaciones (requerido)
- **description** - Una breve descripción de la extensión (opcional) --
  si elige utilizar , con comillas dobles va a romper el formato HTML.
  Utilice comillas simples con sus entidades HTML.
- **element** - El nombre de la extensión instalada (requerido). Para
  los plugins, este debe ser el mismo que el valor del atributo plugin
  del archivo principal en el manifiesto del plugin. Para
  nombreplugin.php, el valor del elemento debe ser **nombreplugin**.
- **type** - El tipo de extensión (componente, módulo, plugin, etc.)
  (requerido)
- **folder** - Específico para plugins, esta etiqueta describe el tipo
  de plugin que se actualiza (contenido, sistema, etc.) (obligatorio
  para los plugins)
- **client** - Requerido para módulos y plantillas de 3.2.0. - El ID de
  cliente de la extensión, que se puede encontrar buscando en el
  interior de la tabla \#\_\_extensions. Hasta la fecha, utiliza 0 para
  el "sitio" y 1 para "administrador". **¡Advertencia!** Los plugins y
  módulos del frontend se instalan automáticamente con un cliente 0
  (sitio), pero deberá especificar el cliente en una actualización o se
  establecerá de forma predeterminada en 1 (administrador) y a partir de
  aquí no se localizará la actualización al no mostrarse porque no
  coincide con ninguna extensión. Los componentes se instalan
  automáticamente con un cliente 1, que actualmente es el valor
  predeterminado.
  - *Advertencia*: el nombre de La etiqueta es \<**client**\> para
    Joomla! 2.5 y \<**client_id**\> para 1.6 y 1.7. Si utilizas (en
    lugar de ) en un sitio 2.5, será ignorado.
- **version** - La versión de la publicación (requerido)
- **infourl** - Una dirección URL para que apunten los usuarios que
  contiene información acerca de la actualización (opcional) (En CMS
  2.5, si se establece, esta dirección URL se mostrará en la vista de
  actualización)
- **downloads** - La sección que enumera todas las ubicaciones de
  descarga
  - **downloadurl** - La URL para descargar la extensión; la etiqueta
    tiene dos parámetros necesarios:
    - **type** - El tipo de paquete (completo o actualización)
    - **format** - El formato del paquete (zip, tar, etc.)
  - **downloadsource** – Optional. Since Joomla 3.8.3. Alternative URL
    to download the extension from when the connection to fails.
    Multiple tags are allowed. The tag has two required parameters:
    - **type** – The type of package (full or upgrade)
    - **format** – The format of the package (zip, tar, etc.)
  - **NOTA** - no debe haber ningún salto de línea antes o después de la
    URL; tiene que ser en una línea o se obtendrá un Error al conectar
    con el servidor: formato incorrecto al ejecutar la actualización
- **tags** - Una lista de etiquetas pertinentes para esta versión.
  Joomla! 3.4 y siguientes lo utiliza para determinar el nivel de
  estabilidad de la actualización. Las etiquetas válidas son:
  - *dev*: versión de Desarrollo, muy inestable y pre-alfa (por ej.ː
    compilaciones nocturnas automáticas)
  - *alfa*: Software de calidad Alfa (características no implementadas,
    fallos por problemas de seguridad)
  - *beta*: Software de calidad beta (todas las funciones implementadas,
    posible errores por problemas de seguridad, casi seguro hay errores
    menores)
  - *rc*: Software de calidad candidata a versión final (no hay errores
    por problemas de seguridad, errores de menor importancia aún pueden
    estar presente)
  - *stable*: Software de calidad para Producción. Todas las demás
    etiquetas serán ignoradas en este momento. Si proporcionas más de
    una etiqueta que contiene uno de las palabras clave de estabilidad
    mencionadas sólo la ÚLTIMA etiqueta será tomada en cuenta. Si no
    proporcionas las etiquetas, Joomla! supone que es una versión
    estable.
- **maintainer** - El nombre del mantenedor de la extensión (similar a
  la etiqueta en un manifiesto) (opcional)
- **maintainerurl** - El sitio web del mantenedor de la extensión
  (similar a la etiqueta en un manifiesto) (opcional)
- **section** - Opcional (de uso desconocido)
- **targetplatform** - Una etiqueta para definir los requisitos de la
  plataforma, requiere de los siguientes elementosː
- **targetplatform** - Una etiqueta para definir los requisitos de la
  plataforma (de
  <img src="https://docs.joomla.org/images/5/55/Compat_icon_3_10.png"
  decoding="async" data-file-width="40" data-file-height="17" width="40"
  height="17" alt="Joomla 3.10" /> esto también se usa para detectar la
  compatibilidad de extensiones para el componente de actualización de
  Joomla), requiere los siguientes elementosː
  - **name** - El nombre de dependencia de la plataforma; en el momento
    de escribir estas líneas, SOLO debería ser "joomla"
  - **version** - La versión de Joomla! que soporta la extensión
  - **min_dev_level** y **max_dev_level** - Estos atributos fueron
    agregados en la versión 3.0.1 para permitirle seleccionar una
    plataforma de destino basada en el nivel del desarrollor ("z" en
    x.y.z). Ambos son opcionales. Puede especificar uno o ambos. Si se
    omite, todos los niveles de desarrollador coinciden. Por ejemplo, lo
    siguiente coincide con las versiones 4.0.0 y 4.0.1.
    - **Nota:** Si su extensión es compatible con Joomla! 2.5 y/o 3.1,
      se le pedirá que tenga definiciones de separadas para cada versión
      debido a la forma en que el actualizador verifica la versión si
      especifica un número. Sin embargo, para mostrar su extensión en
      todas las versiones de Joomlaǃ que admiten actualizaciones
      automáticas (y, por lo tanto, marcada como compatible con todas
      las futuras versiones inéditas de Joomla en la Actualización de
      Joomla!) agregue . Si desea que su extensión se muestre en todas
      las versiones de
      <img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
      decoding="async" data-file-width="40" data-file-height="17" width="40"
      height="17" alt="Joomla 3.x" />, en lugar de especificar una
      versión en la etiqueta de versión, agregue . Esto mostrará la
      actualización a todas las versiones 3.x desde la versión 3.0 a la
      3.5. Si desea incluir la versión 3.10, puede usar un `|` como
      este: . Si desea mostrar las actualizaciones para todas las
      versiones 3.8.x y todas las versiones 3.10.x, puede usar .

- **php_minimum** - A partir de 3.2.2, se admite una versión mínima
  soportada de PHP que puede ser proporcionada en la actualización
  actual. Si el servidor no cumple con el mínimo, se muestra un mensaje
  al usuario que le informa que hay una actualización disponible, pero
  que no se puede instalar debido a requisitos no admitidos.

- **supported_databases** – A partir de 3.7, se puede proporcionar una
  comprobación de bases de datos + versión mínima compatibles en el
  flujo de actualización. Cuando el servidor no cumple con el mínimo, se
  muestra un mensaje al usuario que le informa que hay una actualización
  disponible, pero que no se puede instalar debido a requisitos no
  admitidos. Nota:
  <a href="https://github.com/joomla/joomla-cms/pull/26079"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">A partir de 3.9.12</a> también hay
  un caso especial para mariadb, por lo que ahora puede establecer una
  versión mínima específica para esto.

## Contribuyentes

- **sha256**, **sha384**, **sha512** – Optional. Since Joomla 3.9.0 you
  can add file checksums in these hash formats. Be aware that in Joomla
  3 only a notice is shown on **updates** if a checksum is not correct.
  That's all. The update doesn't stop then. In Joomla 4 updates and
  installations will stop if a provided checksum doesn't match.

Una definición por separado será requerida para cada versión de la
extensión a liberar.

Los valores de **element**, **type**, **client_id** y **folder** deben
coincidir con los de la tabla \#\_\_extensions.

**Importante para plugins:** Los plugins tienen que incluir los
elementos y para que funcione correctamente

## Solución de problemas

- **El script de actualización SQL no se ejecuta durante la
  actualización.**

Si el script de actualización SQL (por ejemplo, en la carpeta
`sql/updatess/mysql`) no se ejecuta durante el proceso de actualización,
puede que sea porque no hay ningún número de versión en la tabla
`#__schemas` para esta extensión *antes de la actualización*. Este valor
está determinado por el último nombre de secuencia de comandos en la
carpeta de actualizaciones de SQL. Si este valor está en blanco, no se
ejecutarán scripts SQL durante ese ciclo de actualización. Para
asegurarse de que este valor esté configurado correctamente, asegúrese
de tener un script SQL en esta carpeta con su nombre como número de
versión (por ejemplo, 1.2.3.sql si la versión es 1.2.3). El archivo
puede estar vacío o simplemente tener una línea de comentario SQL. Esto
debe hacerse en la versión anterior -- una antes de la actualización.
Alternativamente, puede agregar este valor al `#__schemas` mediante una
consulta SQL.

## Herramientas de Apoyo

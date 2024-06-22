<!-- Filename: J4.x:Improved_Override_Management / Display title: Gestión de Modificaciones Mejorada -->

<span id="main-portal-heading">GSoC 2018  
Gestión de Modificaciones Mejorada  
Documentación</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Introducción

Este proyecto agrega una función de comprobación de actualizaciones a
Joomla para que si el archivo principal modificado de una plantilla se
cambia o actualiza, se notifique al usuario de que uno de los archivos
principales de su plantilla se modificó con la actualización, a fin de
evitar problemas de seguridad o de funcionalidad, y pueda ajustar su
anulación antes de que alguien pueda notarlo.

**Enlace al repositorio del proyecto:**
<a href="https://github.com/joomla-projects/gsoc18_override_management"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-projects/gsoc18_override_management</a>

## Obtener una ventaja inicial con la Gestión de Modificaciones Mejorada

### Notas

Si eres nuevo en el desarrollo de Joomla, y no sabes mucho acerca del
Gestor de Plantillas y las Modificaciones, por favor lee:

1.  [Cómo usar el Gestor de
    Plantillas](https://docs.joomla.org/J3.x:How_to_use_the_Template_Manager "Special:MyLanguage/J3.x:How to use the Template Manager")
2.  [Modificaciones de Diseño en
    Joomla](https://docs.joomla.org/Layout_Overrides_in_Joomla "Special:MyLanguage/Layout Overrides in Joomla")

Ahora, debes familiarizarte con el uso del Gestor de Plantillas y los
tipos de Modificaciones en Joomla. Después, veamos las características
de este proyecto.

- Modificaciones Admitidas
- Vista Diferente
- Modificación - Complemento de Notificación de Icono Rápido
- Instalador - Complemento de Modificación
- Actualizado - Histórico de Modificación

## Tipos de modificaciones soportados por esta característica

No es compatible con el Diseño Alternativo en el que el nombre de
archivo se renombra a otra cosa y se modifica js, css. Esta
característica admite los archivos modificados listados en la pestaña
Crear Modificación. Ejemplo:

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_035-es.png"
class="new" title="File:Selection 035-es.png">800px</a>

## Vista diferente entre el núcleo y los archivos modificados

Esta característica muestra la diferencia entre el archivo del núcleo y
el archivo modificado. Cuando abres cualquier archivo modificado para
editar, verás dos botones o conmutadores en la esquina superior derecha
de la página, si se sale del archivo principal de ese archivo.

Botones como este:

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_027-es.png"
class="new" title="File:Selection 027-es.png">Buttons</a>

Aquí puedes controlar la opción de ocultar y mostrar la vista de
diferencias y la vista de archivos del núcleo. En la siguiente imagen,
puedes ver la ubicación del archivo del núcleo y la vista de diferencias
en el gestor de plantillas.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_028-es.png"
class="new" title="File:Selection 028-es.png">800px</a>

No puedes editar el archivo del núcleo.

### Cómo funciona la vista de diferencias

Cuando haces clic en cualquier archivo modificado para editarlo, se
llama a un método de función `getCoreFile` que recibe los dos parámetros
`path` del archivo modificado en la plantilla. Ejemplo:
`/html/layouts/joomla/form/field/user.php` y la ruta cliente del archivo
si pertenece al `Site` o `Administrator`. Luego, en función de esta
información, devuelve la ruta del archivo principal si existe. Para
mostrar la diferencia entre el núcleo y los archivos modificados,
utilizamos la biblioteca
<a href="https://github.com/kpdecker/jsdiff" class="external text"
target="_blank" rel="nofollow noreferrer noopener">jsdiff</a>.

## Modificación - Plugin de Notificación de Icono Rápido

Un plugin de notificación de icono rápido muestra la notificación en el
cpanel, y muestra las modificaciones totales actualizadas de todas las
plantillas. Cuando se actualizan las modificaciones, el ícono rápido
muestra algo como en el siguiente ejemplo:

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_020-es.png"
class="new" title="File:Selection 020-es.png">800px</a>

Cuando haces clic en él, te redireccionará a Plantillas, que contiene la
lista de tus plantillas con su descripción. Verás un nuevo encabezado de
columna **Modificaciones** que muestra el número de modificaciones
actualizadas que pertenecen a la plantilla. Si no se ha actualizado nada
en la modificación de la plantilla, se mostrará un símbolo Actualizado.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_022-es.png"
class="new" title="File:Selection 022-es.png">800px</a>

### Cómo funciona un icono rápido

Realiza una llamada AJAX al `TemplateController.php` que devuelve la
información y muestra una notificación cuando se actualizan dichas
modificaciones.

**Advertencia**

El plugin de notificación de iconos rápidos solo funciona o puede
recuperar datos si el plugin `installer/override` está habilitado. Si
`installer/override` está deshabilitado, verás este mensaje de error en
el icono rápido.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_024-es.png"
class="new" title="File:Selection 024-es.png">800px</a>

Si haces clic en el icono rápido, serás redirigido a la configuración
del plugin `installer/override` donde puedes editar la configuración de
dicho plugin.

## Instalador - Plugin de Modificación

Este plugin es la parte principal de esta función. Permite encontrar las
modificaciones correctas actualizadas durante la instalación o
actualización de la extensión y la actualización de Joomla.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_025-es.png"
class="new" title="File:Selection 025-es.png">800px</a>

### Cómo funciona el plugin de modificación del instalador

Este plugin funciona en 6 eventos:

- onExtensionBeforeUpdate
- onExtensionAfterUpdate
- onInstallerBeforeInstaller
- onInstallerAfterInstaller
- onJoomlaBeforeUpdate
- onJoomlaAfterUpdate

El cual recopila todas las anulaciones del hash del archivo principal
`md5_file()` antes de la actualización y después de la actualización,
luego compara ambos valores. Después, busca el archivo correcto
modificado o actualizado. Y almacena información en la tabla
`#__templates_overrides`.

## Actualizado - Histórico de Modificación

Puedes acceder a esto desde la pestaña Archivos Actualizados en una
plantilla. Esta es una vista de lista que muestra la lista de
modificaciones actualizadas que pertenecen a esa plantilla.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_029-es.png"
class="new" title="File:Selection 029-es.png">800px</a>

Hay muchas opciones disponibles para administrar la lista. Donde puedes
verificar el estado del historial de modificación de archivos, ya sea
que esté marcado o no, la fecha de creación, la fecha de cambio y la
acción de actualización como: si pertenece a (Actualización de Joomla,
Actualización de extensión o Instalación de extensión).

**Nota**

Esta información es solo historia, por lo que si marcaste los cambios de
modificación actualizados, puedes eliminar el historial, pues ya no es
necesario.

**Mira el siguiente video tutorial para aprender cómo utilizar esta
característica.**

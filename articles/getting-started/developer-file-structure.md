<!-- Filename: J4.x:Developer:_File_Structure / Display title: Estructura de Archivos de Ejemplo -->

## Introducción

Como individuo que comienza a trabajar en el desarrollo de extensiones de Joomla en una computadora personal, laptop o de escritorio, necesitas configurar una estructura de archivos adecuada para el código de tu extensión. La ubicación de tu sitio Joomla está predefinida por tu sistema operativo e instalación de Apache. La ubicación del código que creas depende de ti.

## Ubicación del sitio web de prueba

En este ejemplo, los sitios de prueba de Joomla están ubicados en subcarpetas de la raíz del documento. Esto permite la creación de muchos sitios web para todo tipo de proyectos diferentes sin la necesidad de crear hosts virtuales. Algunos sitios de prueba pueden no estar relacionados con Joomla en absoluto. La raíz del sitio para diferentes plataformas:

- Mac: /Usuarios/nombreusuario/Sitios
- Linux: /home/nombreusuario/public_html
- Windows: ...

Esta es una captura de pantalla de parte de una lista de la carpeta de Sitios que muestra una selección de muchos sitios de prueba:

![multiple sites on mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Cada uno se accede por su nombre de subcarpeta. Ejemplos:

- `http://localhost/j4test`
- `http://localhost/j520`

Hay circunstancias en las que podrías preferir crear sitios virtuales separados. Eso no se cubre aquí.

## Estructura de Archivos del Sitio Web de Prueba

Si aún no lo has hecho, necesitarás familiarizarte con la estructura de un sitio web Joomla. La siguiente ilustración muestra un árbol típico de archivos y carpetas de Joomla, con la carpeta de Administrador expandida para mostrar su contenido.

![joomla file structure with administrator expanded](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

Aquí es donde se instalará el código de trabajo. El código fuente está en otro lugar.

## Ubicación del Código de Extensión del Desarrollador

La ubicación de tu código de extensión es una elección personal. Me gusta mantener mi código de extensión en un árbol de archivos adecuado para la creación de un archivo zip instalable. La base de mi árbol es /Users/username/git porque puedo deletrear git y uso git para el control de versiones. No tienes que hacer eso - git se cubrirá en un tutorial aparte. Mi carpeta principal de git contiene muchas subcarpetas, cada una de las cuales puede usar carpetas de git separadas para el control de versiones. Esta es una captura de pantalla que muestra una lista parcial de proyectos:

![joomla file structure project folders](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Observa que algunos de los nombres de las carpetas empiezan con `j4xdemos`, que adopté para usar como la primera parte del espacio de nombres utilizado para mis proyectos creados con fines educativos para Joomla 4. No es necesario como parte del nombre de la carpeta, pero es algo en lo que pensar: la primera parte de tu espacio de nombres debe ser algo único para ti o tu organización. Desde entonces, he adoptado `cefjdemos` como mi prefijo de espacio de nombres, ya que es más personal y no específico de una versión de Joomla.

### Estructura de la Carpeta del Proyecto

Tomando el j4xdemos-com-mywalks como ejemplo, todo el código que irá en la extensión está dentro de la carpeta com_mywalks. Cuando esto se comprime, el archivo com_mywalks.zip creado se utiliza para la instalación en Joomla. Ninguno de los otros archivos en la raíz se incluye en el archivo zip, pero pueden ser incluidos en el repositorio de GitHub. Por ejemplo, README.md es un archivo de texto en formato Markdown que contiene una breve descripción de la extensión. El nombre de la carpeta j4xdemos-com-mywalks no es significativo. No se usa en ningún otro lugar aparte de ordenar las carpetas en orden alfabético.

En la siguiente ilustración, la carpeta j4xdemos-com-mywalks se ha abierto en VSCodium para mostrar la estructura del código del proyecto. El archivo mywalks.xml es un archivo de manifiesto que le indica a Joomla qué instalar y dónde. Las carpetas admin y site contienen código que irá en administrator/components/com_mywalks y components/com_mywalks.

![Project folder open in vscodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

Debe ser evidente que incluso un componente pequeño necesita bastantes carpetas y archivos. Hay herramientas disponibles para la construcción de plantillas de extensión que permiten crear rápidamente un componente esqueleto. Se tratan en otra parte. Tareas pendientes

¿Listo para crear algo de código?

*Traducido por openai.com*


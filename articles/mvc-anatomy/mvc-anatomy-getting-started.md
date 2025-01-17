<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: Anatomía de MVC: Comenzando -->

## Introducción

Los componentes de Joomla siguen el enfoque Modelo, Vista, Controlador, o MVC por sus siglas en inglés. Se supone que el Modelo maneja la carga y el almacenamiento de datos. Se supone que la Vista maneja la visualización de datos. Y se supone que el Controlador maneja el flujo del programa, la interacción entre el código Modelo y Vista del componente.

Si deseas construir tu propio componente, hay dos enfoques de aprendizaje que podrías adoptar:

- **Constrúyelo paso a paso:** siguiendo un tutorial sencillo que describe cada etapa.
- **Desármalo archivo por archivo:** desmontando un componente funcional para aprender cómo funciona cada parte.

Este tutorial utiliza el último enfoque. El componente utilizado para el tutorial se llama com_countrybase. Se utiliza para gestionar y mostrar algunos datos básicos sobre los países del mundo. De hecho, es útil por sí mismo y podría usarse como base para construir algo más sofisticado.

## Esquema de Objetivos

Cualquiera que sea tu proyecto, deberías comenzar con un esquema de tus objetivos y tal vez preguntarte si esos objetivos pueden lograrse con los componentes principales de Joomla o una extensión disponible. Para com_countrybase se necesita una tabla de datos de países, junto con páginas de entrada y salida. Y quizás un módulo para mostrar una tasa de cambio monetario diaria. E incluso un plugin llamado por un comando cli para actualizar los datos de moneda a intervalos regulares. Este tutorial solo cubre el componente.

El componente necesita una tabla de base de datos para almacenar datos de países y una tabla para almacenar datos de monedas. Cada una necesitará una vista de lista y una vista de edición. Esto no es algo que se pueda lograr con los componentes principales de Joomla, por lo que claramente es un caso para un componente personalizado.

## Tablas iniciales

Hay muchos datos sobre países disponibles de todo tipo de fuentes y en todo tipo de formatos. Dado que hay al menos 250 países en el mundo, ingresar datos para cada país uno por uno usando un formulario de entrada de datos sería bastante tedioso. Por lo tanto, preparé tablas iniciales recopilando datos de varias fuentes, combinándolos en una hoja de cálculo y luego exportando declaraciones SQL para crear las tablas.

Los datos se basan en los códigos de país ISO 3166, pero algunos países muy conocidos no están incluidos, por ejemplo, Inglaterra, Escocia y Gales. No necesitas preocuparte por esto. Las tablas se proporcionan con el componente com_countrybase.

Los archivos para el componente com_countrybase están disponibles en GitHub. Puedes descargar un archivo [ZIP](https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip) e instalarlo para verlo funcionando en el menú del Administrador. Crea un elemento de menú si deseas verlo funcionando en la plantilla de tu Sitio. Además, descomprime el archivo zip en el espacio de archivos de tu proyecto, no en el árbol de tu sitio web de prueba, para examinar la estructura del archivo del componente y el contenido del archivo con tu IDE o herramienta de edición de texto favorita.

## Captura de pantalla

Esta lista de Administradores de Países tiene cinco elementos para minimizar el tamaño de la imagen. Joomla normalmente muestra 20 elementos.

![List of countries](../../../en/images/mvc-anatomy/com-countrybase-countries.png)

## Componente Boilerplate

Para ayudarte a comenzar con tu propio componente, hay un [componente modelo](https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip) disponible en Github. Descárgalo y descomprímelo en el espacio de archivos de tu proyecto, no en el árbol de tu sitio web de prueba. Después de la descarga, realiza todos los cambios indicados en el README y estás listo para empezar.

También hay varios generadores de extensiones gratuitos y comerciales que podrías probar para generar un componente básico para tus propios fines. [Joomla! Component Builder](https://www.joomlacomponentbuilder.com/) es gratuito y parece ser completo.

*Traducido por openai.com*


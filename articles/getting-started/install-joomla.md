<!-- Filename: J4.x:Developer:_Required_Software / Display title: Instalar Joomla -->

## Resumen

Este es solo un breve resumen de los pasos involucrados:

- **Descargue** la última versión completa desde la página de [Joomla Downloads](https://downloads.joomla.org/).
- **Guarde** el archivo descargado en el directorio raíz de su documento o en una subcarpeta del mismo.
- **Descomprima** el archivo descargado en el lugar. Algunos sistemas operativos crearán una carpeta con el mismo nombre que el archivo zip descargado, menos el zip. Eso está bien: simplemente puede renombrar la carpeta con un nombre corto y moverla si es necesario. Otros sistemas operativos desempaquetarán los archivos y carpetas en el lugar. Eso es malo si ha colocado la descarga en su carpeta raíz donde hay carpetas existentes con otros sitios. Tendrá que crear un nombre de carpeta corto y mover todos los archivos y carpetas recién extraídos dentro de ella. El objetivo es terminar con una carpeta a la que pueda acceder con su navegador web a través de localhost/j4test.
- **Instale** apuntando su navegador a localhost/j4test y complete los formularios de instalación de Joomla.

## Configuración

Algunas sugerencias:

- **Configuración Global / Sitio / Cookie / Ruta de la Cookie** configurado a la carpeta que contiene tu instalación de Joomla (/j4test).
- **Configuración Global / Sistema / Depuración / Sistema de Depuración** configurado a Sí.
- **Configuración Global / Sistema / Duración de la Sesión** configurado a 60 - de lo contrario, se cerrará la sesión mientras piensas.
- **Configuración Global / Servidor / Servidor / Reporte de Errores** configurado a Máximo.
- **Plugin Sistema - Depuración / Plugin / Actualizar Recursos** configurado a No, a menos que estés depurando CSS o JavaScript.

Eso es todo. Si Joomla está funcionando, estás listo para usarlo en el desarrollo de extensiones.

## Más sitios

Puedes instalar tantos sitios como desees en una sola computadora. Dependiendo de cómo hayas configurado tu servidor, puede ser a través de diferentes subcarpetas accedidas mediante localhost/test1, localhost/test2, etc., o mediante hosts virtuales accedidos por test1.localhost, test2.localhost, etc. De cualquier manera, puedes tener una nueva base de datos y un nuevo sitio Joomla limpio en unos pocos minutos. ¡Elige nombres cortos y significativos! *test1* y *test2* pronto se volverán confusos.

## Instalación para Pruebas

Hay un procedimiento diferente si deseas ayudar con las pruebas de Joomla. En este caso deberías instalar **git** y seguir las instrucciones en el [Repositorio de GitHub](https://github.com/joomla/joomla-cms).

- en GitHub, selecciona la rama de Joomla en la que trabajar. Eso cambia las instrucciones que se ven más abajo en la página.
- En tu portátil o escritorio, sigue las instrucciones desde *Clonar el repositorio:* en adelante.
- ¡No elimines el directorio de instalación al final de la instalación de Joomla!
- Cambia el nombre de la clonación de joomla-cms a otra cosa para que puedas hacerlo todo de nuevo más tarde.
- Instala el Joomla Patchtester.

*Traducido por openai.com*


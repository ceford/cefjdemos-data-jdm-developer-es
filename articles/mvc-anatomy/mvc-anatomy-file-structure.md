<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomía: Estructura de Archivos -->

## Configuración del Desarrollador

Hay varias formas de organizar archivos para fines de desarrollo. Un método es usar un clon de un repositorio de GitHub como en la siguiente estructura esquemática:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Esta es la estructura en el IDE de VSCodium:

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

Al instalarlo, las partes del componente `com_countrybase` se distribuyen en diferentes ubicaciones dentro de la estructura de archivos de Joomla:
- Los archivos de administrador van a `root/administrator/components/com_countrybase`.
- Los archivos del sitio van a `root/components/com_countrybase`.
- Los archivos de medios, javascript y archivos css (si los hay), van a `root/media/com_countrybase`.
- Los archivos de idioma van a las estructuras de componentes del administrador y del sitio. Una convención anterior los colocaba en las carpetas de idioma del núcleo.

Justamente dónde van los elementos está controlado por el archivo de manifiesto del componente, en este caso countrybase.xml.

Nota que el archivo zip contiene todo dentro de la carpeta com_countrybase. Puedes crear un zip simplemente comprimiendo esa carpeta. Fuera de esa carpeta están build.xml, que es un archivo usado para construir el componente cada vez que se realiza un cambio, y README.md, que es un archivo estándar en formato Markdown de Github que describe el componente.

## Notas

- ¡Hay más de 40 archivos en este componente simple!
- Durante la instalación, el archivo countrybase.xml se copia en administrator/components/com_countrybase donde se necesita para propósitos de actualización o desinstalación.

*Traducido por openai.com*


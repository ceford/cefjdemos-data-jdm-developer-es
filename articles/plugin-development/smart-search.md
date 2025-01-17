<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Ejemplo: Búsqueda Inteligente -->

## Introducción

Los complementos de Búsqueda Inteligente se encuentran en el grupo `finder` (en plugins/finder/xxxx). La codificación de los complementos de finder ha cambiado significativamente en Joomla 4 y 5. Para crear un nuevo complemento de finder, probablemente sea mejor copiar un complemento central existente y modificar su contenido para adaptarlo a su nuevo propósito.

Este artículo describe un plugin de búsqueda diseñado para admitir la extensión *Jdocmanual*. El contenido a indexar se encuentra en la tabla `#__jdm_articles`. El código se puede encontrar en [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## Configuración del IDE - VSCode o VSCodium

En mi entorno de desarrollo local, el código fuente de esta extensión se encuentra en ~/git/cefjdemos-plg-jdm-finder. El nombre de esta carpeta no tiene significado; es solo mi manera de mantener mis propias extensiones en orden. La estructura esquemática de la carpeta es la siguiente:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

En VSCodium se ve así:

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Personalizar el código

Comenzando con un complemento básico de búsqueda, la primera tarea es cambiar todas las referencias al complemento original por valores adecuados para el nuevo complemento. En este caso, hay 63 instancias de la palabra `jdocmanual` en 7 archivos. Es muy probable que se pierdan algunas en la primera pasada y el complemento no funcione.

## Construir la Extensión

Hay dos partes en la construcción. Tengo un archivo build.xml que contiene instrucciones para [`phing`](https://www.phing.info/), una herramienta de construcción para proyectos PHP. Se puede llamar desde VSCode/VSCodium o Eclipse o cualquier otro IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

La segunda parte es un archivo `tasks.json` en la carpeta .vscode. Le indica a VSCode dónde encontrar el archivo phing.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
        "type": "shell",
        "command": "php ~/bin/phing-latest.phar",
        "windows": {
          "command": "php ~/bin/phing-latest.phar"
        },
        "group": "build",
        "presentation": {
          "reveal": "always",
          "panel": "shared"
        }
      }
    ]
}
```

En VSCode/VScodium selecciono **Terminal / Ejecutar tarea de compilación...** desde la barra de menú y selecciono la tarea apropiada.

## Instalar la extensión

Después de la construcción, el archivo zip de la extensión debe instalarse de la misma manera que cualquier otra extensión. Debe estar habilitada y se deben configurar las opciones. Jdocmanual tiene tres opciones de *Taxonomías para Indexar*:

- **Manual** uno o todos los Manuales disponibles (Usuario, Desarrollador, ...)
- Las búsquedas por **Idioma** se limitan a artículos en el mismo idioma que la lengua de la página.
- **Tipo** uno o todos los tipos de contenido (Jdocmanual, Contenido, ...)

En el caso del sitio Jdocmanual, otros plugins de búsqueda están deshabilitados porque no hay otro contenido para ser indexado.

Después de la primera instalación, normalmente es suficiente ejecutar la tarea de compilación nuevamente después de la corrección del código. La instalación del archivo zip solo es necesaria si hay cambios en el archivo manifest.xml.

## Indexar el contenido

Jdocmanual está configurado para indexar sus artículos cuando lo solicita un Super Usuario. Esto es diferente del complemento de Contenido normal, que indexa el contenido cada vez que se guarda un artículo. Una primera ejecución lleva unos minutos para indexar los aproximadamente 5000 artículos de Jdocmanual en 8 idiomas.

## Crear un Módulo de Búsqueda Inteligente

Desde el **Contenido / Módulos del Sitio** selecciona **Nuevo** e instala un nuevo módulo de **Búsqueda Inteligente**. Ajusta los ajustes para obtener una apariencia adecuada.

## Pruebas

Eventualmente, tu complemento personalizado de búsqueda inteligente debería funcionar. Esta es una página de resultados de ejemplo para Jdocmanual que busca un término en esta página. La página de resultados omite el formulario de búsqueda de la barra de título porque está presente en el cuerpo de la página.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

Un aparte: el plugin System - Joomla Accessibility Checker muestra que hay 3 errores relacionados con el formulario de entrada de datos *Search Terms*. Eso necesita una corrección central o una sobrescritura.

*Traducido por openai.com*


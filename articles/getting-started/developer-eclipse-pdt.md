<!-- Filename: J4.x:Developer:_Eclipse_PDT / Display title: Eclipse PDT -->

## Introducción

Como Desarrollador PHP necesitarás algunas herramientas para ayudarte en tu trabajo. Eclipse es un IDE (Entorno de Desarrollo Integrado) que se puede usar para todo tipo de proyectos en todo tipo de lenguajes de programación. Eclipse PDT (PHP Developer Tools) incluye las extensiones necesarias para el desarrollo en PHP. Algunas de las características mencionadas en su página de inicio incluyen:

- Resaltado de Sintaxis
- Validación de Sintaxis
- Asistencia de Contenido
- Navegación de Código
- Depuración de PHP (Zend Debugger / Xdebug)
- Perfilado de PHP (Zend Debugger / Xdebug)

Agrega a eso la capacidad de copiar archivos a donde necesitan estar en tu sitio web de prueba local y construir un archivo zip, hay suficiente para trabajar durante años. Aprender a usar Eclipse PDT lleva tiempo, un día o así, pero vale la pena. ¡Hay otros IDEs y editores de código disponibles!

### Alternativas

**Los IDEs** tienen todas las funciones necesarias para construir proyectos PHP. Ejemplos multiplataforma conocidos por ser utilizados por algunos desarrolladores de Joomla incluyen:

- **JetBrains PhpStorm** Comercial, Multiplataforma.
- **Apache NetBeans** Gratis, Código Abierto, Multiplataforma.
- **Visual Studio Code** Gratis, Multiplataforma.

**Editores de PHP** son buenos para editar código pero carecen de algunas de las funciones necesarias para proyectos grandes. Ejemplos incluyen:

- **Notepad++** Gratis, solo para Windows.

## Instalar Eclipse PDT

Visita el sitio web de [Eclipse PDT](https://www.eclipse.org/pdt/) y descarga la versión disponible para tu plataforma (Linux, Mac o Windows).

Sigue las instrucciones de instalación para tu plataforma.

## El espacio de trabajo de Eclipse

Hay al menos tres lugares diferentes donde se ubicará el código de tu proyecto:

- Los archivos de **fuente del proyecto** son aquellos que creas y editas tú mismo. No deben estar en tu árbol web. Deben estar en tu espacio personal de archivos. Ejemplos, donde cada proyecto estará en una subcarpeta separada:
  - /Users/username/git (Mac)
  - /home/username/git (Linux)
  - ... (Windows)
- La ubicación del **árbol web** depende de la instalación de tu pila de software. Puedes usar tu propio espacio de archivos. Ejemplos:
  - /Users/username/Sites/j4test (Mac)
  - /home/username/public_html/j4test (Linux)
  - ... (Windows)
- **Espacio de trabajo de Eclipse** es donde Eclipse almacena información sobre proyectos individuales. La ubicación estándar es la raíz de tu propio espacio de archivos. Ejemplos:
  - /Users/username/eclipse-workspace (Mac)
  - /home/username/eclipse-workspace (Linux)
  - ... (Windows)

¿Listo para comenzar Eclipse?

## Iniciar Eclipse

Después de iniciar, se te pedirá que confirmes varias configuraciones. En algún momento se te pedirá que elijas un espacio de trabajo. El predeterminado es /home/username/eclipse-workspace, pero quieres crear una subcarpeta para cada proyecto, así que selecciona el botón Examinar y luego el botón Nueva carpeta. En el ejemplo a continuación, he creado una carpeta j4tutorials.

![Choose eclipse workspace location](../../../en/images/getting-started/eclipse-pdt-choose-workspace.png)

Seleccione el botón **Lanzar**. Si algo está mal, puede seleccionar Archivo / Cambiar espacio de trabajo más tarde y hacerlo de nuevo. También puede eliminar espacios de trabajo no utilizados más adelante.

Al iniciar por primera vez se te presenta una pantalla de bienvenida. También puedes ir a esta pantalla desde los elementos del menú **Ayuda / Bienvenida**.

![Eclipse welcome screen](../../../en/images/getting-started/eclipse-pdt-welcome.png)

Comience con Revisar la Configuración del IDE. Puede Marcar o Desmarcar cualquiera que parezca apropiado, o Desmarcar o Mover si no está seguro. También puede cambiar la configuración más tarde.

## Crear un nuevo proyecto PHP

El primer proyecto para agregar es el código del sitio web de prueba. Necesitarás esto para verificar que tus propios archivos se estén copiando en los lugares correctos y para depuración más adelante. Selecciona el elemento en la pantalla de bienvenida. Si has avanzado al área de trabajo de Eclipse, selecciona Crear un Proyecto PHP desde el Explorador de Proyectos. En el formulario de Nuevo Proyecto PHP:

- Introduzca un nombre de proyecto adecuado. Debe ser una palabra corta que aparecerá en el explorador de proyectos.
- Seleccione el botón de opción **Crear proyecto en ubicación existente (desde fuente existente)**.
- **Navegue** hasta la ubicación de su sitio web de prueba de Joomla y selecciónelo.
- Seleccione **Finalizar**.

![Eclipse new php project](../../../en/images/getting-started/eclipse-pdt-new-project.png)

Tus archivos del sitio web se agregarán al Explorador de Proyectos. Puede tardar uno o dos minutos para que Eclipse termine este proceso, ya que realiza algunos preparativos en segundo plano. Puedes seleccionar el Proyecto para expandir el primer nivel de carpetas.

Dos archivos se añadirán a la carpeta de tu sitio: .buildpath y .project. Como comienzan con un punto (.), es posible que no los veas y no necesitas preocuparte por ellos. Son archivos XML que contienen información utilizada por Eclipse.

## La Perspectiva PHP

La colección de paneles en la pantalla de Eclipse se conoce como una perspectiva. Las dos más comúnmente usadas son la Perspectiva PHP y la Perspectiva de Depuración. Puedes cambiar entre ellas utilizando los iconos en la parte superior derecha de la Barra de Herramientas. También puedes usar el menú: Ventana / Perspectiva / Abrir Perspectiva / PHP o Depuración.

Las siguientes ilustraciones muestran la Perspectiva PHP con un par de formularios de edición abiertos.

![Eclipse php perspective](../../../en/images/getting-started/eclipse-pdt-php-perspective.png)

Los paneles de Eclipse:

- El panel izquierdo es el Explorador de Proyectos donde puedes navegar por la estructura de archivos.
- El panel central superior es donde aparecen los editores de texto abiertos.
- El panel superior derecho normalmente muestra un esquema del panel de edición seleccionado. Puede mostrar otra información.
- El panel inferior derecho muestra cualquiera de una serie de Vistas, seleccionables desde el menú Ventana / Mostrar Vista.

La vista de Problemas dice Errores (100 de 791 elementos) y Advertencias (100 de 11629). Esto parece mucho, pero no hay de qué preocuparse.

## Un nuevo proyecto PHP de Hola Universo

Ahora estás listo para crear un nuevo proyecto PHP para la extensión que planeas desarrollar. Para ilustrarlo, puedes comenzar con un componente simple llamado com_hellouniverse. Para mí, el código de Hello Universe se ubicará en /Users/username/git/j4xtutorials-com-hellouniverse y utilizaré j4xtutorials como mi afiliación de espacio de nombres (la primera palabra en el espacio de nombres del componente).

En el menú de Eclipse, selecciona Archivo / Nuevo / Proyecto PHP. El formulario es el que se ilustra arriba. Mis datos:

- Nombre del proyecto: HelloUniverse
- Contenidos: selecciona Crear proyecto en la ubicación existente (desde fuente existente).
- Navegar: Navega a /Home/username/git y selecciona el botón Nueva Carpeta.
- En el diálogo de Nueva Carpeta, ingresa j4xtutorials-com-hellouniverse y presiona Crear.
- Con esa carpeta vacía abierta, selecciona Abrir.
- El directorio seleccionado debería decir: /Users/username/git/j4xtutorials-com-hellouniverse
- Selecciona Finalizar.

Ahora verás el nuevo proyecto en el Explorador de Proyectos junto con tu proyecto del sitio web. El nuevo proyecto contiene dos elementos, ambos relacionados con el soporte de PHP. Hay algunos elementos ocultos utilizados por Eclipse: .settings(carpeta), .buildpath y .project.

## Agregar carpeta com_hellouniverse

Con el proyecto HelloUniverse seleccionado:

- Seleccione el elemento del menú Archivo o haga clic derecho en el nombre del proyecto.
- Seleccione los elementos del menú Nuevo / Carpeta.
- En el formulario Nueva Carpeta, introduzca com_hellouniverse en el campo Nombre de la carpeta:.
- Seleccione Finalizar.

La carpeta com_hellouniverse es donde irá el código de tu extensión. Cualquier cosa dentro de esa carpeta terminará en un archivo zip que puedes usar para instalar en Joomla. Cualquier cosa que no esté dentro de esa carpeta se utiliza para otros propósitos. Dos archivos comunes en este nivel:

- README.md es un archivo de texto con formato Markdown que describe el componente. Dichos archivos se utilizan comúnmente junto con el almacenamiento de código remoto, como en Github (más sobre eso en otro lugar).
- build.xml es un archivo que contiene instrucciones sobre qué hacer después de haber realizado cambios en el código. Lo más común es que desees copiar los archivos modificados a los lugares correctos en el árbol de tu sitio web y regenerar el archivo zip. Luego, para la mayoría de los cambios, puedes simplemente recargar la página web en la que estás trabajando. No tienes que reinstalar el componente. ¡Esto ahorra una cantidad enorme de tiempo!

## Agregar carpetas de administrador, sitio y medios

- Seleccione la carpeta com_hellouniverse y utilice los elementos del menú Archivo / Nuevo / Carpeta para crear una nueva carpeta llamada admin.
- Nuevamente, esta vez cree una nueva carpeta llamada media. CUIDADO: asegúrese de que el padre sea com_hellouniverse y no HelloUniverse.
- Nuevamente, esta vez cree una nueva carpeta llamada site. Si se crea en el lugar incorrecto, se puede arrastrar al lugar correcto.

## build.xml

El siguiente código contiene un archivo build.xml de ejemplo para colocar en la raíz de su proyecto HelloUniverse.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="hellouniverse" basedir="." default="main">
    <property file=".project" />

    <!-- Source and Destinations -->

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="srcdir" value="${project.basedir}/com_hellouniverse" override="true" />
    <property name="sitedir" value="/Users/username/Sites/j4tutorials"  override="true" />
    <property name="zipsdir" value="/Users/username/zips"  override="true" />

    <!-- Filesets -->

    <fileset dir="${srcdir}/admin" id="adminfiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/media" id="mediafiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/site" id="sitefiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}" id="allfiles">
        <include name="**" />
    </fileset>

    <!-- Targets -->

    <target name="main" description="main target">
        <copy todir="${sitedir}/administrator/components/com_hellouniverse">
            <fileset refid="adminfiles" />
        </copy>
        <copy todir="${sitedir}/media/com_hellouniverse">
            <fileset refid="mediafiles" />
        </copy>
        <copy todir="${sitedir}/components/com_hellouniverse">
            <fileset refid="sitefiles" />
        </copy>
        <zip destfile="${zipsdir}/com_hellouniverse.zip">
            <fileset refid="allfiles" />
        </zip>
    </target>
</project>
```

Algunas explicaciones:

- **srcdir** es la ubicación en el sistema de archivos de tu código fuente, de donde se copian los archivos.
- **sitedir** es la ubicación en el sistema de archivos de tu sitio de prueba, a donde se copian los archivos.
- **zipsdir** es una ubicación para el archivo zip; encuentro conveniente mantener archivos zip de diferentes proyectos juntos en una carpeta de zips.
- **Conjuntos de archivos** son grupos de origen que se copiarán a diferentes destinos. significa todas las carpetas y archivos.
- **Destinos** son las ubicaciones y lo que necesita ser copiado allí.

Crea un archivo build.xml que contenga el siguiente código:

- Selecciona Archivo / Nuevo / Archivo XML del menú de Eclipse.
- Asegúrate de que HelloUniverse esté seleccionado como el padre e introduce el nombre build.xml (exactamente así).
- Selecciona Finalizar.
- En la Vista de Fuente (elige en la parte inferior izquierda) copia y pega el código de arriba en lugar de la línea ya existente.
- Guarda.

Para que esto funcione necesitas una herramienta de construcción: Phing.

## La herramienta de construcción Phing¶

Ve al sitio web de [Phing](https://www.phing.info/) y descarga el archivo Phar. Puedes usar este [enlace directo](https://www.phing.info/get/phing-latest.phar). Guarda el archivo en una carpeta phing en tu carpeta personal bin /Users/username/bin/phing. Cambia los permisos del archivo a 755 para que pueda ejecutarse desde la línea de comandos. Para probar, abre una terminal, cambia a tu carpeta bin/phing y escribe ./phing-latest.phar -v para ver el número de versión. Recuerda la ruta completa al archivo:

- /Usuarios/usuario/bin/phing/phing-último.phar

Lo necesitarás más tarde.

## Preferencias de Eclipse - Constructores

Aquí es donde configuras Phing para construir tu proyecto cada vez que haces cambios en el código.

- En el Explorador de Proyectos, selecciona el proyecto HelloUniverse.
- Selecciona los elementos de menú Archivo / Propiedades.
- En el cuadro de diálogo de Propiedades, selecciona Constructores y luego el botón Nuevo.
- Desde el cuadro de diálogo Tipo de Configuración, selecciona Programa y luego OK.
- En el cuadro de diálogo Editar propiedades de configuración de lanzamiento, ingresa un título adecuado: Construir HelloUniverse.
- En el campo Ubicación, introduce la ruta al programa phing o utiliza el botón Examinar Sistema de Archivos para buscarlo: /Users/ceford/bin/phing/phing-latest.phar
- Para el campo Directorio de trabajo, selecciona el botón Examinar espacio de trabajo y luego selecciona HelloUniverse.
- Haz clic en OK, luego Aplicar y Cerrar.

Para probar: selecciona los elementos del menú Proyecto / Construir Proyecto. La vista de la Consola en la parte inferior de la pantalla mostrará el progreso de la construcción. En esta etapa es probable que muestre CONSTRUCCIÓN FALLIDA en rojo. Eso es esperado: las carpetas de componentes se crearán durante la instalación del archivo zip. Aún no existen porque no hay código. Pero muestra que la herramienta de construcción Phing está funcionando.

## Cómo depurar

Cuando algo sale mal, es posible que desees revisar el código línea por línea para ver lo que realmente está sucediendo. Si has configurado el Sistema de Depuración en Sí y el Informe de Errores en Máximo en la Configuración Global de Joomla, puedes tener un rastreo de pila para mostrar dónde ocurre un error en el código. O puede que estés viendo algo inusual en la salida y tengas una buena idea de dónde puede estar la falla. En cualquier caso, necesitas establecer puntos de interrupción donde la ejecución se detendrá para permitirte ver los valores de las variables y avanzar a través del código línea por línea.

### Establecer un Punto de Interrupción

Los puntos de interrupción deben establecerse en el proyecto del sitio web, J4Tutorials. Como ejemplo, en el panel del Explorador de Proyectos, expande el proyecto J4Tutorials y encuentra administrator / components / com_users / src / Model y haz doble clic en UsersModel.php para abrirlo en una ventana de edición. Ve a la línea 87 y haz doble clic en el número 87. Aparece un pequeño marcador azul para indicar que se ha establecido un punto de interrupción. La ejecución del código debería detenerse aquí cuando veas la lista de Usuarios a través de los elementos del menú Administrador / Usuarios / Gestionar.

### Crear una Configuración de Depuración

Desde el menú superior selecciona Ejecutar / Configuraciones de Depuración. En el formulario ingresa un título reconocible, por ejemplo Depurar Admin, para seleccionar más tarde de una lista desplegable. Asegúrate de que el archivo seleccionado sea /J4Tutorials/administrator/index.php. En el formulario del panel Común selecciona Depurar del menú lista de Mostrar en favoritos. Verifica que el Depurador esté configurado como Xdebug. Selecciona Aplicar y Cerrar.

Ahora puedes seleccionar Depurar Administrador desde la lista desplegable de depuración de la Barra de herramientas, el ícono del símbolo de error.

![Eclipse debug tools](../../../en/images/getting-started/eclipse-pdt-debug-tools.png)

Depurar iconos, pasar el ratón para ver etiquetas:

- **Omitir Todos los Puntos de Interrupción** Un interruptor para omitir los puntos de interrupción excepto la primera línea de index.php.
- **Reanudar** Continuar la ejecución normal hasta el siguiente punto de interrupción.
- **Terminar** Detener la depuración (pero también necesita eliminar la cookie de Xdebug).
- **Entrar** En una línea que contiene una llamada a función, entrar en esa función.
- **Pasar sobre** En una línea que contiene una llamada a función, no entrar en esa función.
- **Salir** En una función, omitir detenerse en cada línea y regresar al llamador.

### Procedimiento de Depuración

Al iniciar una sesión de depuración, la ejecución se detiene en la primera línea de administrator/index.php. Esa es la página principal del panel de control. Necesitas seleccionar el símbolo de Reanudar (barra amarilla y triángulo verde hacia la derecha). Después de un retraso de un segundo o algo así, hay más actividad y más lanzamientos remotos pausados en el panel de administración de depuración de la izquierda. Esta es la página de inicio que utiliza ajax para obtener información de actualización. Simplemente sigue haciendo clic en Reanudar hasta que la actividad se detenga. Luego, en tu navegador, selecciona el elemento Usuarios del panel de control de inicio. Eclipse cobra vida y detiene la ejecución en el punto de interrupción que estableciste.

![Eclipse PHP debug perspective](../../../en/images/getting-started/eclipse-pdt-debug-perspective.png)

Características a destacar:

- La línea de código actual está marcada con un fondo verde pálido.
- El panel izquierdo muestra un seguimiento de pila: las llamadas a funciones previas que llevaron a la función actual. Selecciona cualquier elemento para ver el código padre.
- El panel derecho puede mostrar variables activas o puntos de interrupción. Expande objetos según sea necesario para ver detalles.

### Algunos problemas

- No detenerse en los puntos de interrupción: Esto ocurre si abres otro programa PHP como phpMyAdmin. Para solucionarlo:
  - Ve a Ejecutar / Configuraciones de Depuración y selecciona tu elemento de Depuración de Admin.
  - En la pestaña Depurador selecciona Configurar...
  - En Mapeo de Ruta selecciona y elimina cualquier ruta no relacionada con tu sitio web de prueba.
  - Selecciona Finalizar - puede que necesites Terminar y recargar la sesión de depuración.
- La depuración se reanuda después de seleccionar el botón Terminar. Para solucionarlo:
  - Usa las Herramientas para Desarrolladores de tu navegador para abrir una ventana de inspección.
  - Encuentra las cookies que tu navegador está usando (Almacenamiento en Firefox)
  - Selecciona y elimina la cookie XDebug.

## Usando Git

Git es un sistema de control de versiones ampliamente usado para gestionar cualquier tipo de texto, principalmente código, pero puede ser cualquier cosa. Funciona desde una línea de comandos, pero generalmente se invoca mediante herramientas gráficas como Eclipse. Si te gustaría leer sobre git, consulta el [sitio web de git](https://git-scm.com/).

Si deseas usar git, necesitas instalarlo para tu plataforma. Luego, en Eclipse, selecciona tu proyecto HelloUniverse, haz clic derecho y selecciona Team / Share Project. En el cuadro de diálogo Compartir proyecto, selecciona **Usar o crear repositorio en la carpeta padre del proyecto** y selecciona el proyecto HelloUniverse de la lista. Hay un campo para indicar dónde se creará el repositorio (/Users/ceford/git/j4xtutorials-com-hellouniverse) - selecciona el botón Crear repositorio adyacente. Finalmente, selecciona el botón Finalizar.

![Configure Git Repository](../../../en/images/getting-started/eclipse-pdt-configure-git.png)

Notarás que han aparecido algunas decoraciones nuevas junto a los elementos en la vista del Explorador de Proyectos:

- El marcador \> indica que hay contenido que ha cambiado pero no se ha confirmado en el repositorio.
- El marcador ? indica que este elemento no se ha añadido a la lista de elementos para ser rastreados.

![Git Markers](../../../en/images/getting-started/eclipse-pdt-git-markers.png)

Ahora estás configurado para el control de versiones. Haz clic derecho en el proyecto y observa cómo se ve el menú Equipo. ¡Demasiado para cubrir aquí!

## Finalmente

¿Listo para comenzar a trabajar en tu propio código?

*Traducido por openai.com*


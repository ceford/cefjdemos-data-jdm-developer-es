<!-- Filename: Working_with_git_and_github / Display title: Trabajar con git y github -->

# Introducción

Este documento te proporcionará información acerca de como contribuir
con el CMS de Joomla! con la ayuda de Git y GitHub. Si te gusta hacer un
cambio simple (un sólo archivo), es más fácil el uso de esta
documentación: [Uso de la interfaz de usuario Github para Hacer
Solicitudes de
Extracción](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests "Special:MyLanguage/Using the Github UI to Make Pull Requests")
Si deseas añadir cambios más complejos o simplemente está interesado en
esto, ¡sigue leyendo!

## ¿Qué son Git y GitHub?

Git es un sistema de control de versiones distribuido. Es un sistema que
registra los cambios en los archivos y mantiene un historial de los
mismos. Siempre podrás mirar una versión anterior del código y
restablecer los cambios si lo deseas. Debido al historial, Git es muy
útil cuando trabajas con muchas personas en un mismo proyecto.

Así es como GitHub puede ser utilizado.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> es un sitio web que ayuda
a manejar Proyectos Git de una manera visual. Como propietario de un
proyecto puedes cambiar el código y comparar las diferentes versiones.
Como usuario del proyecto puedes contribuir haciendo una Solicitud de
Extracción. Una Solicitud de Extracción es una petición al propietario
para sacar un poco de código del proyecto. Estás ofreciendo un trozo de
código (tal vez una solución a un error) y preguntas si el propietario
del Proyecto desea usarlo. Si al dueño le gusta, lo puede combinar
(agregar) a su proyecto.

Joomla! utiliza GitHub y Git para mantener su código. Todo el mundo
puede contribuir con el software de Joomla!. La dirección URL del
proyecto CMS de Joomla! en GitHub es:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

# Primeros pasos

## Inscribirse en GitHub e instalar Git

En primer lugar, tendrás que registrarse en
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>. Es gratis y fácil de
hacer. Sólo tienes que seguir los pasos indicados.

Uno estamos inscritos, necesitamos instalar Git. La última versión de
Git se puede encontrar en
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Descargamos y
abrimos el instalador. Git es un programa CLI( CLI significa Interfaz de
Línea de Comandos, por sus siglas en ingés). Al principio esto puede ser
confuso y un poco intimidante pero este documento te guiará en todo el
proceso.

Si no eres un usuario avanzado, sólo tienes que ejecutar el instalador y
presionar el botón "siguiente" hasta que el programa se termine de
instalar. Git no dañara tu sistema. Más tarde lo puedes quitar al igual
que otros programas si lo deseas.

Una vez que hemos instalado Git, abrimos el programa que se llama "Git
Bash". Una comando en línea se abrirá. Vamos a decirle a Git cual es
nuestro nombre y correo electrónico. Git utilizará esta información para
contribuir a un proyecto. Con los siguientes comandos damos a Git esa
información:

    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com

En los comandos anteriores y todas las demás órdenes que se imparten en
esta documentación, cada línea es un nuevo comando. Así que escribes la
primera línea, presiona enter y, a continuación, escribes la segunda
línea y presiona enter.

Ahora estamos listos para usar Git e ir más allá con la creación de
nuestra instalación de prueba.

# Configurar una instalación de prueba

Para nuestra instalación de prueba necesitamos un programa servidor web,
para que podamos instalar y ejecutar Joomla! en nuestro equipo. Hay
varios programas que pueden hacer esto, por ejemplo
<a href="https://www.mamp.info/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">MAMP</a> o
<a href="https://www.apachefriends.org/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">XAMPP</a>. Descarga e
instala alguno de ellos.

Después de la instalación de un programa (yo uso MAMP en esta
documentación), vamos a instalar la última versión de Joomla!. En
nuestro caso, la última versión de Joomla! no es la última versión
estable. La última versión de Joomla! es la presentación de la rama en
GitHub. En primer lugar, permítanme explicar un poco más acerca de
GitHub.

## Bifurcar y Clonar Joomla!

En GitHub puedes encontrar proyectos, en los llamados Repositorios.
Dentro de un proyecto se pueden encontrar varias versiones. Cada versión
se llama Rama. Joomla! tiene las siguientes ramas:

- **Presentación:** Esta rama contiene las últimas correcciones de
  errores y nuevas características de Joomla!
- **Maestro:** Esta rama es la versión estable actual de Joomla!
- **3.5-dev** Esta rama contiene los archivos de Joomla! 3.5, la que no
  es estable al momento de escribir el presente.

En nuestro lugar de la prueba vamos a usar la rama **Presentación**,
pero si queremos usar esta rama directamente tendremos problemas. No
podemos modificar esta rama porque no somos el propietario de la misma.
Vamos a hacer una copia de la misma. En GitHub esto se llama una
Bifurcación. Somos el dueño de esa copia para que podamos modificarla.
Después de la modificación, comparamos nuestra bifurcación con el
proyecto original. En ese momento podemos hacer una Solicitud de
Extracción para los cambios que hemos hecho. Más sobre esto más
adelante. Puede hacer una Bifurcación de una rama presionando en el
botón Fork
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Repositorio del CMS
Joomla! en Github</a>. Este botón se encuentra en la parte superior
derecha de la página.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

Después de la bifurcación, vamos a instalar Joomla! en nuestro servidor
Web local. Ve a la carpeta donde se puedan ejecutar archivos en el
servidor Web. La mayoría de los programas usan una carpeta llamada
`htdocs`. Una vez que estamos dentro de esa carpeta, pulsa el botón
derecho del ratón has clic sobre: "Git Bash Here". La línea de comandos
se abrirá para esta ubicación.

Escribe el siguiente comando para descargar los archivos desde tu
Bifurcación del CMS Joomla! a tu equipo. Por favor, reemplaza *username*
con el nombre de usuario que estás utilizando en GitHub.

    git clone https://github.com/username/joomla-cms.git

Para todas las órdenes que se imparten en esta documentación, tienes que
abrir Git a través de los pasos descritos anteriormente. Por favor,
recuerda esto para otros comandos en esta documentación.

Una vez que Git esta listo, abre tu navegador y ve a la instalación en
tu localhost. Normalmente la URL es algo así como:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
Ahora podrás ver el valor el proceso de instalación predeterminado de
Joomla!.

## Instalar Joomla!

La instalación de Joomla! en nuestro espacio local de prueba es casi la
misma que una instalación normal. Hay dos pequeñas diferencias.

Para la configuración de la base de datos, el nombre de usuario y
contraseña predeterminados. Por lo general el nombre de usuario es
`root` y la contraseña también es `root` o no hay ninguna contraseña. Si
aún no puedes conectarte a la base de datos, busca en el manual de tu
servidor Web local el nombre de usuario y contraseña.

La última diferencia es el último paso de la instalación. Normalmente
debemos eliminar la carpeta de instalación para poder ir al Lado Cliente
o Lado Servidor de Joomla!. Para una instalación de prueba podemos
omitir esta parte y podemos ir directamente al Lado Cliente o Lado
Servidor. No eliminar la carpeta de instilación, puede ser muy útil
cuando tenemos que instalar Joomla! de nuevo.

# Hacer Cambios

Ahora es el momento de hacer nuestros cambios en los archivos de
Joomla!. Todos los cambios que hagamos, van a ser registrados y
supervisados por Git. En cualquier momento puedes escribir el comando
`git status` para ver que archivos fueron modificados o están sin
seguimiento. Sin seguimiento, significa que el archivo en esa ubicación
es nuevo para Git.

Si has cometido un error o quieres restablecer un archivo puedes
utilizar este comando:

    git checkout path/to/file

Si desea quitar todos los cambios realizados puedes utilizar los
siguientes comandos:

    git checkout .
    git clean -f -d

El primer comando restablece todos los archivos. El segundo comando
elimina archivos y carpetas sin seguimiento.

## Publicar Nuestros Cambios en GitHub

# Poner el cambio a nuestra bifurcación

Después de hacer nuestros cambios, tenemos que subirlos a nuestro
repositorio en GitHub. Después de eso, se puede hacer una Solicitud de
Extracción con nuestros cambios.

Subir los cambios que se llama `push` en términos Git. Antes de que
podamos hacer eso, tenemos que hacer algo muy importante. Debemos crear
una nueva rama para que nuestros cambios (Una rama es una versión de
nuestro proyecto, ¿recuerdas?). Si no lo hacemos y ponemos nuestro
cambio directamente en la presentación de la rama, la primera vez no
será un problema. Pero si hemos hecho cambios una segunda vez y el
cambio que hemos realizado la primera vez no se combinan, todos los
cambios serán registrados como nuevos cambios.

Así que el primer comando que se va a ejecutar es crear una nueva rama.
Para evitar el problema descrito más arriba. Reemplaza el nombre de la
nueva rama con el nombre de la nueva rama. Este nombre debe ser corto,
sólo puede contener letras minúsculas y números. **NO** contiene
espacios. En lugar de espacios usa - (menos)

    git checkout -b name-new-branch

El siguiente comando le dice a git, que todos los cambios son buenos y
están listos para utilizarse.

    git add --all

El siguiente comando agrega nuestro cambio a la rama. Por favor,
reemplaza el mensaje con una breve descripción de los cambios. Esta
descripción será el título de la solicitud de extracción que vamos a
hacer.

    git commit -m "description"

El último comando push (carga) los cambios de nuestra bifurcación. Por
favor reemplaza el nombre de la nueva rama con el nombre de la rama
creada un par de pasos antes.

    git push origin name-new-branch

## Comparar las bifurcaciones y hacer una solicitud de extracción

Después de subir nuestro cambio a GitHub, ir a la bifurcación del CMS
Joomla!.

# Información Extra

Ya que la presentación de la versión de Joomla! puede cambiar en
cualquier momento, es muy útil tener la posibilidad de mantener nuestra
bifurcación actualizada. Podemos hacerlo mediante la adición de un mando
a distancia para nuestro proyecto bifurcado:

    git remote add upstream https://github.com/joomla/joomla-cms.git

Ahora hemos añadido una llamada remota llamado "upstream". Con el
siguiente comando Git busca nuevas contribuciones (commits) en la
presentación de la rama que no tenemos en nuestra bifurcación. Si se
encuentra alguna, se va a añadir a nuestra bifurcación:

    git pull upstream staging

Los cambios por ahora sólo se ha realizado en nuestra bfurcación local.
Para subirlos a GitHub utiliza el siguiente comando:

    git push

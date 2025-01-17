<!-- Filename: Working_with_git_and_github / Display title: Trabajando con git y github -->

## Introducción

Este documento proporcionará información sobre cómo contribuir al CMS de Joomla! con la ayuda de Git y GitHub. Si te gustaría realizar un cambio sencillo (solo un archivo), es más fácil usar esta documentación: [Using the Github UI to Make Pull Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests). Si deseas agregar cambios más complejos o simplemente estás interesado en esto, ¡sigue leyendo!

## ¿Qué son Git y GitHub?

Git es un sistema de control de versiones distribuido. Es un sistema que registra cambios en archivos y mantiene estos cambios en un archivo de historial. Siempre puedes retroceder a una versión anterior de tu código y restaurar los cambios si lo deseas. Debido al archivo de historial, Git es muy útil cuando trabajas con muchas personas en el mismo proyecto.

Aquí se muestra cómo se puede usar GitHub. [GitHub](https://www.github.com) es un sitio web que ayuda a gestionar proyectos de Git de una manera visual. Como propietario de un proyecto, puedes cambiar el código y comparar diferentes versiones. Como usuario del proyecto, puedes contribuir haciendo una Solicitud de Extracción. Una solicitud de extracción es una petición al propietario para que integre algún código en el proyecto. Estás ofreciendo un fragmento de código (quizás una solución para un error) y preguntando si al propietario del proyecto le gustaría usarlo. Si al propietario le gusta, puede fusionarlo (agregarlo) a su proyecto.

¡Joomla! utiliza GitHub y Git para mantener su código. Cualquiera puede contribuir al software de Joomla! a través de su [repositorio de GitHub](https://github.com/joomla/joomla-cms).

## Regístrate en GitHub e instala Git

Primero, necesitarás [registrarte en Github](http://www.github.com). Es gratis y fácil de hacer. Solo sigue los pasos proporcionados.

Una vez que te hayas registrado, necesitas instalar Git en el ordenador que usas para el desarrollo (estación de trabajo o portátil). Sigue las instrucciones de instalación en el sitio web de [git](http://git-scm.com). Git es un programa de CLI (Interfaz de Línea de Comandos). Al principio, esto puede ser confuso y un poco intimidante, pero este documento te guiará a lo largo del proceso.

Si no eres un usuario avanzado, simplemente ejecuta el instalador y presiona los botones de "siguiente" hasta que el programa esté instalado. Git no dañará tu sistema. Más tarde, puedes eliminarlo como cualquier otro programa.

Con Git instalado, abre una aplicación de Terminal. Comienza configurando tu nombre y dirección de correo electrónico en Git. Git utilizará esta información cuando contribuyas a un proyecto:

```sh
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

En los comandos anteriores, y en todos los otros comandos dados en esta documentación, cada línea es un nuevo comando. Así que escribe la primera línea, presiona enter y luego escribe la segunda línea y presiona enter.

Ahora estás listo para usar Git y continuar con la configuración de tu instalación de prueba.

## Configuración de una instalación de prueba

Para su instalación de prueba necesita un paquete de software para poder instalar y ejecutar Joomla! en su computadora. Paquetes como MAMP, XAMP o WAMP se tratan en otra parte de esta documentación.

Después de la instalación de tu pila de software, necesitas instalar la última versión de Joomla!. Esta no es la última versión estable. La última versión de Joomla! es la rama de desarrollo en GitHub.

## Bifurcar y Clonar Joomla!

En GitHub puedes encontrar proyectos en los llamados Repositorios. Dentro de un proyecto puedes encontrar varias versiones. A tal versión se le llama Rama. ¡Joomla! tiene las siguientes ramas:

- **4.2-dev** Archivos para el desarrollo de la versión actual.
- **4.3-dev** Archivos para el desarrollo de la próxima versión menor.
- **4.4-dev** Archivos para el desarrollo de la versión menor siguiente.
- **5.0-dev** Archivos para el desarrollo de la próxima versión mayor.

En tu computadora de prueba vas a utilizar la rama **4.2-dev**. Sin embargo, no puedes modificar esta rama porque no eres su propietario. Necesitas hacer una copia de ella. En GitHub esto se llama un Fork. Tú eres el propietario de esa copia, por lo que puedes modificarla. Después de modificar tu fork, puedes hacer un Pull Request para los cambios que has realizado. Más sobre eso más adelante. Puedes hacer un Fork de una rama presionando el botón Fork en el [Repositorio de Joomla! CMS en Github](https://github.com/joomla/joomla-cms). Este botón está ubicado en la parte superior derecha de la página.

![Fork joomla in github](../../../en/images/getting-started/core-git-fork-joomla.png)

Después de bifurcar, necesitas instalar Joomla! en tu computadora local. Ve a la carpeta donde puedes colocar archivos utilizados por tu servidor web. Muchos programas usan una carpeta llamada `htdocs`. Algunos usan `www` y otros usan carpetas completamente distintas. Todo depende de si estás usando Windows, Mac o Linux. Eventualmente, tu raíz web contendrá diferentes carpetas para diferentes sitios web. Una vez que estés dentro de tu carpeta raíz web. Ya sea, en una ventana de Terminal abierta, usa el comando cd para cambiar el directorio actual a la raíz web. O, en tu explorador de archivos GUI, encuentra la carpeta raíz web, presiona el botón derecho del ratón y haz clic en: "Git Bash Here" o "Abrir Terminal" o algo similar.

En la Terminal, con la carpeta raíz del sitio configurada como la carpeta actual, escribe el siguiente comando para descargar los archivos de tu Fork del Joomla! CMS a tu computadora. Por favor, reemplaza *username* con el nombre de usuario que estás usando en GitHub.

```sh
    git clone https://github.com/username/joomla-cms.git
```

El proceso de clonación puede tardar bastante. Cuando termine, tu raíz web contendrá una carpeta llamada joomla-cms. Necesitas hacer que esa carpeta sea la carpeta actual para ejecutar comandos git para esa carpeta:

```sh
    cd joomla-cms
```

Por favor, recuerda eso para otros comandos en esta documentación.

## Instalar Joomla!

Tu clon descargado de tu repositorio bifurcado necesita más acciones antes de que esté listo para su uso. Uno de los archivos descargados se llama README.md. Ábrelo con un editor de texto y sigue las instrucciones sobre **Cómo obtener una instalación funcional desde el origen**.

Cuando esté listo, abra su navegador y vaya a la instalación en su localhost. Normalmente, la URL es algo como: `http://localhost/joomla-cms`. Ahora verá el proceso de instalación predeterminado de Joomla!.

## Hacer cambios de código

Todos los cambios que realices en el código de Joomla en tu sitio local serán registrados y monitoreados por Git. En cualquier momento puedes escribir el comando `git status` para ver cuáles archivos están modificados o no rastreados. No rastreado significa que el archivo en esa ubicación es nuevo para Git.

En esta etapa, probablemente sea mejor usar un Entorno de Desarrollo Integrado (IDE) para trabajar en el código de Joomla. Se recomienda encarecidamente Visual Studio Code (VSCode). Es gratuito y funciona en todas las plataformas. Usando VSCode, puedes hacer cambios, **Confirmarlos** en tu clon local de Git y **Enviarlos** a tu bifurcación remota de GitHub.

## Enviar cambios al fork

Subir cambios se llama `push` en términos de Git. Antes de que puedas hacer eso, tienes que hacer algo muy importante. Debes crear una nueva rama para tus cambios. (Una rama es una versión de tu proyecto, ¿recuerdas?) Si no haces eso y realizas tu cambio directamente en la rama actual, la primera vez no habrá problema. Pero si haces cambios por segunda vez y los cambios que hiciste la primera vez aún no se han fusionado, todos esos cambios también se registrarán como nuevos cambios.

Puedes configurar VSCode para realizar todos los siguientes comandos con unos pocos clics. Pero si deseas usar comandos de git desde la línea de comandos del terminal:

Así que el primer comando que vas a ejecutar creará una nueva rama. Reemplaza name-new-branch con el nombre de la nueva rama. Este nombre debe ser corto y solo puede contener letras minúsculas y números. **NO** uses espacios. En lugar de espacios, utiliza - (guion).

```sh
    git checkout -b name-new-branch
```

El siguiente comando le indica a git que todos los cambios están listos para confirmar.

```sh
    git add --all
```

El siguiente comando añade tu cambio a la rama. Por favor, sustituye el mensaje por una breve descripción de tus cambios. Esta descripción será el título de la solicitud de extracción que vas a realizar.

```sh
    git commit -m "description"
```

El comando final empujará (subirá) los cambios a tu bifurcación. Por favor, reemplaza name-new-branch con el nombre de la rama que creaste unos pasos antes.

```sh
    git push origin name-new-branch
```

## Comparar Forks y Hacer una Solicitud de Extracción

Después de subir tu cambio a GitHub, ve a tu bifurcación del Joomla! CMS en el sitio de GitHub. Selecciona tu rama y haz una solicitud de extracción.

Cuando termines, en tu clon local, cambia de nuevo a la rama original:

```sh
git checkout 4.2-dev
```

Ahora puedes hacer nuevos cambios sin afectar los cambios en tu rama anterior.

## Mantenerse al día

Debido a que la versión actual de Joomla! puede cambiar todos los días, es importante mantener tu bifurcación actualizada. Puedes hacerlo agregando el repositorio original de Joomla a tu proyecto bifurcado:

```sh
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Luego actualiza tu clon local con el siguiente comando:

```sh
    git pull upstream 4.2-dev
```

Y actualiza tu fork remoto:

```sh
    git push
```

*Traducido por openai.com*


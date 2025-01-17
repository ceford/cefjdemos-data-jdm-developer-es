<!-- Filename: J4.x:Developer:_Required_Software / Display title: Configuración de la Base de Datos -->

## Sobre MySQL y MariaDB

Los recién llegados pueden tener la impresión de que MySQL y MariaDB son bases de datos y pueden confundirse cuando las instrucciones dicen *crear una nueva base de datos*. De hecho, ambos son Sistemas de Gestión de Bases de Datos Relacionales (RDBMS) que pueden gestionar muchas bases de datos individuales. Su sitio de prueba local puede tener muchas instalaciones individuales de Joomla, cada una con su propia base de datos. Por ejemplo, puede desear probar su extensión en la versión actual de Joomla y por separado en un candidato de lanzamiento próximo.

La siguiente captura de pantalla muestra parte de una lista de más de 30 bases de datos creadas para probar varios proyectos de instalaciones y extensiones de Joomla.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

Una nota al margen: las intercalaciones son en su mayoría utf8mb4_0900_ai_ci:

- utf8mb4 significa que cada carácter se almacena como un máximo de 4 bytes en el esquema de codificación UTF-8.
- 0900 se refiere a la versión del Algoritmo de Colación Unicode.
- ai se refiere a insensibilidad a acentos, no hay diferencia entre e, è, é, ê y ë al ordenar.
- ci se refiere a insensibilidad a mayúsculas y minúsculas, no hay diferencia entre p y P al ordenar.

Las tablas y columnas individuales pueden tener una intercalación diferente. Las tablas de Joomla suelen tener la intercalación utf8mb4_unicode_ci.

## Configuración de la base de datos con phpMyAdmin

Antes de instalar Joomla, necesitas una base de datos vacía lista para ser poblada. También necesitas un usuario de base de datos.

### Crear una base de datos

- **Iniciar phpMyAdmin** Ingresa localhost/phpmyadmin en la barra de direcciones de tu navegador.
- **Iniciar sesión** Dependiendo de cómo se haya instalado, el nombre de usuario será root o admin y puede que haya o no una contraseña.
- Selecciona **Base de datos** desde el menú superior de la página principal de phpMyAdmin.
- En **Crear Base de Datos** ingresa un nombre corto en lugar de la sugerencia **Nombre de la Base de Datos**, por ejemplo, jtest.
- Selecciona **utf8mb4_0900_ai_ci** desde la lista desplegable de Intercalación.
- Selecciona **Crear** para crear la base de datos.

Eso te llevará a una base de datos vacía. En la parte superior hay un mensaje: *No se encontraron tablas en la base de datos.*

### Crear un usuario

- Seleccione el icono de **Inicio** en la parte superior izquierda de phpMyAdmin para ir a la página de inicio.
- Seleccione **Cuentas de usuario** en el menú superior de inicio.
- Seleccione **Agregar cuenta de usuario** en el panel Nuevo debajo de la lista de usuarios actuales (si los hay).
- Ingrese un **Nombre de usuario** que pueda ser un nombre corto que necesitará más adelante durante la instalación de Joomla. Ejemplo: jtest. ¡Puede usar este mismo usuario para otras bases de datos creadas posteriormente!
- Seleccione **Local** en la lista del campo Nombre de host. Establecerá localhost en el campo de valor adyacente.
- Use el botón **Generar** para generar una contraseña. Necesita copiar el valor generado y pegarlo en un editor de texto para usarlo más tarde. No necesita recordarlo a largo plazo ya que se almacenará como texto plano en el archivo configuration.php de Joomla. Ejemplo: 8t8mGQq.gw\[\]8lp(
- **Guardar** salte las secciones de base de datos para usuario y privilegios globales del formulario. El botón **Ir** (Guardar) está en la parte inferior.

### Asignar Permisos de Base de Datos al Usuario

- En la página de **Cuentas de Usuario**, selecciona el usuario (jtest), si es necesario a través de Inicio / Cuentas de usuario / Nombre de usuario.
- Selecciona **Base de Datos** cerca de la parte superior de la página. Eso mostrará una lista de bases de datos a las que se le han concedido permisos de acceso a este usuario y, al final, una lista de bases de datos a las que no se le ha concedido acceso.
- **Selecciona** la base de datos a la que se desea conceder acceso y selecciona el botón **Ir**.
- **Privilegios específicos de la base de datos** Selecciona **Marcar todos** y luego **Ir** para guardar.

¡Eso es todo! Ahora puedes cerrar sesión en phpMyAdmin. ¿Olvidaste anotar la contraseña del usuario de la base de datos? Vuelve y edita la cuenta de usuario que creaste para cambiarla. Si usas el mismo usuario de base de datos para múltiples bases de datos de Joomla, encontrarás la contraseña en un archivo configuration.php de Joomla.

*Traducido por openai.com*


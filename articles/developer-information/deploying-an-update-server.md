<!-- Filename: Deploying_an_Update_Server / Display title: Actualizar Servidores -->

## Antecedentes

Hay una buena descripción de los Servidores de Actualización en la Documentación para Programadores de Joomla! copiada a [este sitio](jdocmanual?article=docus/install-update/update-server) o disponible en la [fuente original](https://manual.joomla.org/docs/building-extensions/install-update/update-server/).

## Solución de problemas

- **El script de actualización SQL no se ejecuta durante la actualización.**

Si el script de actualización SQL (por ejemplo, en la carpeta `sql/updates/mysql`) no se ejecuta durante el proceso de actualización, podría ser porque no hay un número de versión en la tabla `#_schemas` para esta extensión *antes de la actualización*. Este valor está determinado por el nombre del último script en la carpeta de actualizaciones SQL. Si este valor está en blanco, no se ejecutará ningún script SQL durante ese ciclo de actualización. Para asegurarte de que este valor se establece correctamente, asegúrate de tener un script SQL en esta carpeta con su nombre como el número de versión (por ejemplo, 1.2.3.sql si la versión es 1.2.3). El archivo puede estar vacío o simplemente tener una línea de comentario SQL. Esto debe hacerse en la versión antigua, la que está antes de la actualización. Alternativamente, puedes agregar este valor a `#_schemas` utilizando una consulta SQL.

*Traducido por openai.com*


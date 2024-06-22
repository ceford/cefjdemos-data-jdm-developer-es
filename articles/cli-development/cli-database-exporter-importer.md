<!-- Filename: J4.x:CLI_Database_Exporter_Importer / Display title: CLI Exportar / Importar base de datos -->

Joomla!  4.0

## Acerca de

Antes de actualizar Joomla! o [instalar una extensión de
terceros](https://docs.joomla.org/J4.x:CLI_Update "Special:MyLanguage/J4.x:CLI Update"),
se recomienda encarecidamente que haga una copia de seguridad de su
sitio.  
La Línea de Comandos de Joomla 4.x le facilita comandos para exportar
(hacer un respaldo) e importar (restaurar) la base de datos de su
Joomla.

## Requisitos

Para usar estos comandos, necesita un acceso seguro al terminal (SSH) a
su servidor en el que tenga instalada la interfaz PHP CLI (Interfaz de
Línea de Comandos) . Es recomendable tener un conocimiento básico de uso
de los comandos de shell

## Instrucciones

Acceda a su servidor y vaya a la carpeta raíz de su sitio.  
Le recomendamos que utilice la carpeta 'tmp' de Joomla en su para tener
permisos de lectura/escritura.

- Listar todos los comandos disponibles en la Línea de Comandos de
  Joomla:  
  `php cli/joomla.php list`
- Exportar la base de datos a la carpeta:  
  `php cli/joomla.php database:export --all --folder `
- Importar la base de datos desde una carpeta:  
  php cli/joomla.php database:import --all --folder

También puede:

- Exportar la base de datos a un fichero zip:  
  `php cli/joomla.php database:export --all --zip`
- Exportar una tabla:  
  `php cli/joomla.php database:export --table `
- Exportar una tabla como fichero .zip:  
  `php cli/joomla.php database:export --table --zip`
- Importar una tabla:  
  `php cli/joomla.php database:import --table `
- Si necesita ayuda:  
  `php cli/joomla.php database:export --help`
  `php cli/joomla.php database:import --help`

## Hacer un respaldo y restaurar

Para hacer un respaldo completo (con carpetas, ficheros y bases de
datos) de su sitio, puede ejecutar los siguientes comandos:

1.  Archivar el directorio raiz de Joomla:  
    `tar --exclude='./tmp/joomla_bak.*' -zcvf tmp/joomla_bak.tgz . > tmp/joomla_bak.log`
2.  Exportar toda la base de datos de Joomla:  
    `php cli/joomla.php database:export --all --folder tmp/db_bak`

Y restaurarla, ejecutando estos comandos:

1.  Importar toda la base de datos de Joomla:  
    `php cli/joomla.php database:import --all --folder tmp/db_bak`
2.  Extraer el archivo:  
    `tar --recursive-unlink -xvf tmp/joomla_bak.tgz .`

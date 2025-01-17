<!-- Filename: How_to_use_JDate / Display title: Cómo usar la clase Date -->

## Introducción
La clase Date de Joomla es una clase auxiliar, extendida de la clase DateTime de PHP, que permite a los desarrolladores manejar el formato de fechas de manera más eficiente. La clase permite a los desarrolladores formatear fechas para cadenas legibles, interacción con MySQL, cálculo de marcas de tiempo UNIX, y también proporciona métodos auxiliares para trabajar en diferentes zonas horarias.

## Creación de una instancia de fecha

Todos los métodos auxiliares de fecha requieren una instancia de la clase Date. Para comenzar, debes crear una. Un objeto Date puede crearse de dos maneras. Una es el método nativo típico de simplemente crear una nueva instancia:

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

También puedes crear una instancia usando el método estático definido en Date:

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

No hay diferencia entre estos métodos, ya que Date::getInstance simplemente crea una nueva instancia de Date exactamente como el primer método mostrado.

Alternativamente, también puede obtener la fecha actual (como un objeto Date) del objeto Application, utilizando:
```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Argumentos

El constructor Date (y el método estático getInstance) acepta dos parámetros opcionales: una cadena de fecha para formatear y una zona horaria. No pasar una cadena de fecha creará un objeto Date con la fecha y hora actuales, mientras que no pasar una zona horaria permitirá que el objeto Date use la zona horaria predeterminada establecida.

El primer argumento, si se utiliza, debe ser una cadena que pueda ser analizada utilizando el constructor native DateTime de PHP. Por ejemplo:
```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

Un sello de tiempo de Unix (en segundos) también se puede pasar como el primer argumento, en cuyo caso se transformará internamente en una fecha. Si se ha especificado una zona horaria como el segundo argumento para el constructor, se convertirá a esa zona horaria.

## Mostrar Fechas

Una nota de precaución al mostrar objetos de fecha en un contexto de usuario: no simplemente los imprima en la pantalla. El método toString() del objeto Date simplemente llama al método format() de su padre, sin considerar zonas horarias ni el formato de fecha local. Esto no resultará en una buena experiencia de usuario y llevará a inconsistencias entre el formato en su extensión y en otras partes de Joomla. En su lugar, siempre debe mostrar las fechas usando los métodos que se muestran a continuación.

### Formatos de Fecha Comunes

Un número de formatos de fecha están predefinidos en Joomla como parte de los paquetes de idiomas base. Esto es beneficioso porque significa que los formatos de fecha pueden ser fácilmente internacionalizados. A continuación se muestra un ejemplo de las cadenas de formato disponibles, del paquete de idioma en-GB. Se recomienda encarecidamente utilizar estas cadenas de formato al mostrar fechas, para que sus fechas se reformateen automáticamente de acuerdo con la configuración regional del usuario. Se pueden recuperar de la misma manera que cualquier cadena de idioma (ver abajo para ejemplos).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### El método HtmlHelper (Recomendado)

Al igual que con muchos elementos comunes de salida, la clase HtmlHelper está aquí para...¡ayudar! El método date() de HtmlHelper tomará cualquier cadena de fecha-hora que el constructor de Date aceptaría, junto con una cadena de formato, y mostrará la fecha adecuadamente según la configuración de zona horaria del usuario actual. Como tal, este es el método recomendado para mostrar fechas al usuario.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### El método format() del objeto Date

Otra opción es formatear la fecha manualmente. Si se utiliza este método, también tendrás que recuperar y establecer manualmente la zona horaria del usuario. Este método es más útil para formatear fechas fuera de la interfaz de usuario, como en los registros del sistema o llamadas a la API.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Otros ejemplos de código útiles

### Mostrar rápidamente la hora actual

Hay dos maneras fáciles de hacer esto.
- El método date() de HtmlHelper, si no se proporciona un valor de fecha, tomará como predeterminado la hora actual.
- Factory::getDate() obtiene la fecha actual como un objeto Date, que luego podemos formatear.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Sumando y Restando Fechas

Debido a que el objeto Date de Joomla extiende el objeto DateTime de PHP, proporciona métodos para sumar y restar fechas. El más sencillo de estos métodos es el método modify(), que acepta cualquier cadena de modificación relativa que aceptaría el método PHP [strtotime](https://www.php.net/manual/en/function.strtotime.php). Por ejemplo:

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

También hay métodos separados add() y sub(), para sumar o restar tiempo de un objeto de fecha respectivamente. Estos aceptan objetos [DateInterval](https://www.php.net/manual/en/class.dateinterval.php) estándar de PHP:

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Mostrar fechas en formato ISO 8601

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Mostrar fechas en formato RFC 822

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Generación de Fechas en Formato de Fecha-Hora SQL

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Emitiendo fechas como marcas de tiempo Unix

Una marca de tiempo Unix se expresa como el número de segundos que han transcurrido desde la Época Unix (el primer segundo del 1 de enero de 1970).

*Traducido por openai.com*


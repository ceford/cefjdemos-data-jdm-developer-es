<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Espacios de nombres -->

## Introducción

El uso de **Espacios de Nombres en PHP** es una forma de agrupar clases, interfaces, funciones y constantes relacionadas para evitar conflictos de nombres en proyectos grandes. Permiten a los desarrolladores organizar el código de manera más efectiva, especialmente al trabajar con bibliotecas de terceros o bases de código grandes que podrían tener nombres de clases duplicados.

Hay más información en la sección [Namespace](jdocmanual?article=docus/namespaces/index) de la Documentación para Programadores de Joomla!.

### Características Clave de los Espacios de Nombres en PHP

1. **Evitar Conflictos de Nombres**:
   - Sin espacios de nombres, si dos clases tienen el mismo nombre, PHP lanzará un error. Los espacios de nombres proporcionan un contexto para identificar de manera única clases u otros elementos.
2. **Organizar Código**:
   - Los espacios de nombres permiten agrupar lógicamente clases o funciones relacionadas, haciendo el código más fácil de leer y mantener.
3. **Acceso a través de Nombres Completamente Calificados**:
   - Las clases y funciones dentro de un espacio de nombres pueden ser accedidas usando su nombre completamente calificado, que incluye la ruta del espacio de nombres.

### Prácticas comunes:
- Usa espacios de nombres para reflejar la estructura de carpetas de tu proyecto para mayor consistencia.
- Importa espacios de nombres utilizando la palabra clave `use` para un código más limpio.

## El archivo Manifesto

La declaración inicial de un espacio de nombres de extensión se encuentra en un archivo de manifiesto de extensión, como en este ejemplo:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

Los elementos en esta declaración son los siguientes:

- **path="src"** indica al cargador de clases que las clases con espacio de nombres están en la carpeta **src** de la extensión, por ejemplo, com_wonderful/src.
- **Acme** es el prefijo del proveedor. Para las extensiones del núcleo de Joomla sería *Joomla*. Necesitas inventar tu propio prefijo de proveedor único.
- **Component** indica el tipo de extensión (componente, módulo, plugin, plantilla, biblioteca, ...).
- **Wonderful** es el nombre de la extensión. Elige un nombre de extensión que sea corto, significativo y probablemente único.

## El Archivo de Clase

La declaración de espacio de nombres debe ser la primera declaración en el archivo en el que se utiliza. Por ejemplo:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Usando el archivo de clase

Cuando necesites usar la clase, por ejemplo en una plantilla, insertarías una declaración **use** en la parte superior de la plantilla:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

Y luego instanciar la clase y llamar a la función:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## El archivo autoload_psr4.php

Este archivo se encuentra en la carpeta administrator/cache del sitio Joomla. Contiene un mapa completo de los espacios de nombres extraídos de los archivos de manifiesto. Se genera durante la instalación y se regenera cada vez que se instala o reinstala una extensión. Puedes eliminar este archivo y se regenerará en la siguiente carga de página. A veces es necesario hacer esto si se ha intentado instalar una extensión por un método inusual. El archivo contiene 275 o más rutas a ubicaciones de clases con espacios de nombres.

**PSR** es un acrónimo de [**PHP Standards Recommendation**](https://www.php-fig.org/psr/) y [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) es un estándar aceptado para cargar clases de PHP.

*Traducido por openai.com*


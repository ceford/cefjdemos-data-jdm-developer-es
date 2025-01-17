<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: MVC Anatomía: Archivos del Sitio -->

## Estructura de Archivos

Hay menos archivos en la parte del Sitio del componente que en la parte del Administrador, por lo que parece un buen lugar para comenzar. Solo se cubrirán aquellas partes de cada archivo que necesiten explicación. Es mejor si abres cada archivo mencionado, echas un vistazo al contenido general y luego encuentras las partes que se explican. El orden alfabético de la estructura de archivos es el siguiente:

```bash
    site
    |- forms
        |- filter_countries.xml
    |- language
        |- en-GB
           |- com_countrybase.ini
    |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
    |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayContoller.php

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

namespace Cefjdemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  1.0.0
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Partes de este archivo se explican de la siguiente manera:

### Aviso de Copyright

Cada archivo php debe comenzar con un aviso de derechos de autor como el siguiente:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */
```

Si frecuentemente creas nuevos archivos y copias/pegas esta sección, recuerda actualizar el nombre del componente y el aviso de copyright. Además, el inspector de código requiere una línea en blanco antes de un bloque de documentación.

### Espacio de nombres y verificación definida

Después del aviso de derechos de autor, cada archivo php debe tener una línea que contenga defined('_JEXEC') o die; excepto que los archivos con espacio de nombres deben declarar el espacio de nombres antes de cualquier otro código php, por lo que antes de la verificación defined. Los archivos php con espacio de nombres son aquellos que contienen clases php de componentes en la carpeta src o sus subcarpetas.

```php
namespace Cefjemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

El chequeo `defined` evita que un archivo php se ejecute llamándolo directamente a través de su URL. La constante `_JEXEC` se define cuando la aplicación se inicia a través del archivo index.php de raíz o de administrador. Es una ayuda de seguridad importante.

El control `defined` está envuelto en `phpcs:disable` y `phpcs:enable` para permitir una violación del estándar de codificación PSR12.

Combinado con el espacio de nombres declarado en el archivo de manifiesto countrybase.xml, Joomla buscará cualquier clase declarada en el archivo actual en root/components/com_countrybase/src/Controller - en este caso, agregando el nombre de este archivo, DisplayController.php.

### Declaraciones use

Las declaraciones de uso generalmente siguen la verificación definida y a menudo se enumeran en orden alfabético. Las declaraciones de uso definen las ubicaciones de las clases utilizadas por este archivo php. A veces, las declaraciones de uso están allí por error, siendo declaradas pero no utilizadas. Eso no causa daño, pero debería corregirse. Aquí hay dos declaraciones sin usar:

```php
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Clase Controladora

El controlador de visualización tiene casi nada que hacer, ya que todo el trabajo se realiza en la clase padre. Lo único importante que hace es establecer la vista predeterminada, en este caso **countries**. Eso hará que la vista predeterminada del componente utilice los archivos Countries/HtmlView.php y tmpl/countries/default.php para mostrar los datos de los países.

```php
/**
 * Countrybase Component Controller
 *
 * @since  1.0.0 Created for first release.
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

El bloque de documentación (DocBlock) que precede a la declaración de `class` se utiliza para la documentación automática con herramientas como *Doxygen*. La declaración `@since` es una *etiqueta* que indica el número de versión de la extensión donde se introdujo este elemento. Puede ir seguida de una explicación en texto simple. Cualquier elemento puede tener un DocBlock con cualquier número de etiquetas estándar. ¡Es una buena práctica mantener tu propio código bien documentado!

No olvides establecer el \$default_view para este controlador. Sin él, el DisplayController usará la vista predeterminada definida en el archivo de configuración del componente o, si no existe, el nombre del componente.

## src/View/Countries/HtmlView.php

El controlador ha establecido la vista predeterminada en **países**, por lo que el siguiente paso es cargar el código correspondiente de HtmlView.php. Partes de este archivo requieren alguna explicación.

### Variables de clase

```php
class HtmlView extends BaseHtmlView
{
    /**
     * The model state
     *
     * @var  \Joomla\CMS\Object\CMSObject
     */
    protected $state = null;
    ...
    protected $items = null;
    ...
    protected $pagination = null;
    ...
    public $filterForm;
    ...
    public $activeFilters;
```

Las variables de clase se utilizan para almacenar información sobre la página que se va a mostrar:

- `$state` - el estado del modelo, a menudo iniciado por la entrada de un formulario o cadena de consulta.
- `$items` - la lista de datos de países recuperados de la base de datos.
- `$pagination` - un objeto utilizado para mostrar un mecanismo de navegación por páginas si hay más páginas que el límite de la lista, normalmente 20 elementos.
- `$filterForm` - no se encuentra generalmente en las páginas del sitio, pero se utiliza en com_countrybase para filtrar por nombre de país o estado publicado.
- `$activeFilters` - utilizado para realizar un seguimiento de qué filtros están en uso.

### función de visualización

Esto es bastante estándar para la vista de un sitio, aparte de las partes filterForm y activeFilters:

```php
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->pagination = $this->get('Pagination');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');

        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        parent::display($tpl);
    }
```

Las declaraciones de la forma `$this->get('Xxxx')` hacen que Joomla busque en `CountriesModel.php` una función llamada `getXxxx()` y devuelva cualquier dato producido por ese código para su almacenamiento y uso en la vista. A menudo, la función está en el padre de CountriesModel. Por ejemplo, la función getItems no está presente en CountriesModel.php, pero está presente en el ListModel del que se extiende.

En resumen, la clase de vista recupera datos del modelo y luego llama a su clase principal para mostrar los datos.

## Modelo/ModelosDePaíses.php

Hay un pequeño número de funciones en el Modelo que generalmente necesitas completar tú mismo. Otras son heredadas del ListModel padre.

### __construct

Es práctica normal incluir en el constructor cualquier campo de filtro que desees utilizar. Sin ellos, los filtros pueden parecer no tener efecto. A menudo se olvidan cuando deseas agregar un filtro más tarde. Cada campo se proporciona con su nombre de campo y con un alias de nombre de tabla, generalmente a, b, c, y así sucesivamente, pero puede ser cualquier cosa que elijas que sea consistente con el código en otros lugares.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### populateState

Esta función toma parámetros de entrada y los prepara para su uso en una consulta de base de datos. Algunos parámetros son manejados en el padre, por lo que no es necesario hacer nada aquí. Por ejemplo, si el campo de búsqueda es **title**, eso es manejado por el padre, al igual que los campos de **state** y la **start** y **limit** de la paginación.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

Esta función crea un hash para almacenar una consulta para usar en otra parte.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

Aquí es donde se crea la consulta para extraer datos de la base de datos. Requiere cierta comprensión de cómo se construyen las consultas.

```php
    protected function getListQuery()
    {
        // Create a new query object.
        $db = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                [
                    $db->quoteName('a.title'),
                    $db->quoteName('a.iso_2'),
                    $db->quoteName('a.iso_3'),
                    $db->quoteName('a.country_code'),
                    $db->quoteName('a.region_code'),
                    $db->quoteName('a.subregion_code'),
                    $db->quoteName('a.phone_prefix'),
                    $db->quoteName('a.currency_code'),
                    $db->quoteName('a.state'),
                    $db->quoteName('b.title') . ' AS currency_title',
                    $db->quoteName('b.symbol'),
                    $db->quoteName('b.dollar_exchange_rate'),
                ]
            )
        )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search)) {
            $search = '%' . str_replace(' ', '%', trim($search) . '%');
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Filter by published state
        $published = (string) $this->getState('filter.published');

        if ($published !== '*') {
            if (is_numeric($published)) {
                $state = (int) $published;
                $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
            }
        }

        // Add the list ordering clause.
        $orderCol = $this->state->get('list.ordering', 'a.title');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
        return $query;
    }
```

Explicación

- **getQuery(true)** obtiene un nuevo objeto de consulta vacío.
- **\$query-\>select()** añade una instrucción SELECT. Puede haber múltiples instrucciones select - Joomla las concatena.
- **los alias de tabla a y b** notan que algunas columnas provienen de diferentes tablas.
- **-\>from()** define qué tabla es la tabla a. esto puede estar en una declaración separada: \$query-\>from();
- **-\>leftjoin()** define la tabla b y cómo se unirá a la tabla a.
- **\$query-\>where()** hace uso de cualquier filtro definido, uno para **buscar** y otro para **estado**.
- **return \$query** no hay llamada principal, todo en la consulta debe configurarse aquí.

## tmpl/countries/default.php

Esta es la parte del código donde se crea el contenido html. Para la lista de países se necesita una tabla con una fila de encabezado y una fila para los datos de cada país. Dado que hay 250 países se necesita un mecanismo de paginación para mostrar un subconjunto de países unos pocos a la vez. Eso requiere un formulario. Y en este caso una barra estándar **searchtools** de Joomla es útil. Aquí está:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>"
    method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
    <table class="table table-striped">
    <caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
    <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_(
                'searchtools.sort',
                'COM_COUNTRYBASE_COUNTRIES_COUNTRY',
                'a.title',
                $listDirn,
                $listOrder
            ); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) : ?>
    <tr>
        <td><?php echo $item->title; ?></td>
        <td><?php echo $item->iso_2; ?></td>
        <td><?php echo $item->iso_3; ?></td>
        <td><?php echo $item->currency_title; ?></td>
        <td><?php echo $item->symbol; ?></td>
        <td><?php echo $item->currency_code; ?></td>
        <td><?php echo $item->dollar_exchange_rate; ?></td>
    </tr>
    <?php endforeach; ?>
    </tbody>
    </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Puntos a tener en cuenta:

- **\$listOrder** y **\$listDirection** se utilizan para ordenar por encabezado de columna. Solo el título ha sido configurado para hacerlo.
- **form action** se configura típicamente para referirse a sí mismo.
- **LayoutHelper::render('joomla.searchtools.default',...)** crea una barra de búsqueda del tipo que se ve en las páginas de lista del administrador. **¡Necesita un formulario de filtro!**
- **\$this-\>pagination-\>getListFooter()** obtiene el código HTML para el widget de paginación.
- **task** este campo oculto se rellena con javascript cuando se envía el formulario.
- **boxchecked** este campo oculto se utiliza cuando se seleccionan una o más casillas de verificación de fila para la operación por lotes. ¡Realmente no se necesita aquí!
- **HTMLHelper::\_('form.token');** obtiene el código para un token de formulario usado como dispositivo de seguridad para envíos de formularios que involucran envío de datos.

## tmpl/countries/default.xml

Este archivo se utiliza para crear un elemento de menú. Tiene el mismo nombre que el archivo php, por lo tanto, default.xml en este caso.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
    <layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL"
        option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
        <help
            url="components/com_countrybase/help/en-GB/countrybase.html"
        />
        <message>
            <![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
        </message>
    </layout>

    <!-- Add fields to the parameters object for the layout. -->
    <fields name="params">

        <!-- Options -->
        <fieldset name="options">
        </fieldset>

    </fields>
</metadata>
```

Notas:

- **help url** apunta a un archivo de ayuda en la carpeta del administrador. Te permite crear tus propios archivos de ayuda local, invocados desde el botón de ayuda del formulario de edición del menú después de seleccionar el tipo de menú Vista Predeterminada de Countrybase.
- **params** te permite usar parámetros, por ejemplo, si mostrar o no una columna particular en la lista de países. Aún no se han especificado parámetros.
- Las traducciones de **key** deben estar en el archivo administrator/language/en-GB/countrybase.sys.ini. La clave *COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC*, traducida como *Muestra una página de países.*, aparece como descriptor en el formulario de selección del Tipo de Elemento de Menú.

## forms/filter_countries.xml

Este archivo es necesario para la barra de búsqueda. Sin él, Joomla arrojará un error fatal. El nombre del archivo debe ser exactamente como se muestra: el nombre de la vista precedido por filter\_. El contenido es simple, solo definiciones para el campo de búsqueda y cualquier otro filtro que desees usar.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>
    <fields name="filter">
        <field
            name="search"
            type="text"
            label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
            description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
            hint="JSEARCH_FILTER"
        />

        <field
            name="published"
            type="status"
            label="JOPTION_SELECT_PUBLISHED"
            onchange="this.form.submit();"
            >
            <option value="">JOPTION_SELECT_PUBLISHED</option>
        </field>

    </fields>

    <fields name="list">
        <field
            name="fullordering"
            type="list"
            label="JGLOBAL_SORT_BY"
            default="a.title ASC"
            onchange="this.form.submit();"
            >
            <option value="">JGLOBAL_SORT_BY</option>
            <option value="a.title ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC</option>
            <option value="a.title DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC</option>
            <option value="a.iso_2 ASC">ISO2 ASC</option>
            <option value="a.iso_2 DESC">ISO2 DESC</option>
            <option value="a.iso_3 ASC">ISO3 ASC</option>
            <option value="a.iso_3 DESC">ISO3 DESC</option>
            <option value="a.currency_code ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC</option>
            <option value="a.currency_code DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC</option>
            <option value="a.id ASC">JGRID_HEADING_ID_ASC</option>
            <option value="a.id DESC">JGRID_HEADING_ID_DESC</option>
        </field>

        <field
            name="limit"
            type="limitbox"
            label="JGLOBAL_LIST_LIMIT"
            default="25"
            onchange="this.form.submit();"
        />
    </fields>
</form>
```

Tenga en cuenta que cualquier clave de cadena que comience con J está definida por Joomla y no debe incluirla en sus archivos de idioma.

## language/es-ES/com_countrybase.ini

Joomla siempre carga las traducciones de clave de idioma en inglés antes que cualquier otro idioma. Esto asegura que las claves no aparezcan en la salida si un idioma está incompletamente traducido. Debido a que las claves de idioma son palabras largas en pseudo-inglés, se considera mejor tener una mezcla de inglés y otro idioma que una mezcla de claves y otro idioma. Si se está usando otro idioma, Joomla sobrescribe las cadenas en inglés con las de ese otro idioma.

Tenga en cuenta que es una práctica común listar las cadenas en orden alfabético de las claves:

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Enrutador.php

El Router es necesario para las URLs SEO. Sin él, un enlace del menú puede aparecer como option=com_countrybase&view=countries. Con él, un enlace aparecerá como country-base.html o cualquier nombre que elijas para el alias del título del enlace.

```php
    public function __construct(
        SiteApplication $app,
        AbstractMenu $menu
    ) {

        $countries = new RouterViewConfiguration('countries');
        $countries->setKey('id');
        $this->registerView($countries);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
```

Si hay más vistas, por ejemplo una tabla de monedas, deberías definir cada vista aquí antes de la declaración parent::\_\_construct().

*Traducido por openai.com*


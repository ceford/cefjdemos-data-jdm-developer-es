<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Anatomía: Administrador Editar Archivos -->

## Archivos de Datos del País

Los archivos del administrador incluyen cuatro vistas: dos vistas de lista para los datos de países y datos de monedas, y dos vistas de edición para gestionar la entrada de datos en cada una de las dos tablas involucradas. Las vistas de lista son casi idénticas a la vista de lista de países descrita para la interfaz del sitio, por lo que no se cubrirán nuevamente aquí. De manera similar, las vistas de edición son casi idénticas, por lo que solo se cubrirá una, la vista de entrada de datos para un solo país.

## src/Controller/CountryController.php

¡Esto es inmensamente simple! Aparte de la declaración de clase, carece de contenido. Todo está gestionado por la clase padre.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

Aquí es donde se obtienen los datos del modelo para su uso en el formulario de entrada de datos. Hay una diferencia significativa con la vista de lista descrita anteriormente: es una práctica común usar una barra de herramientas con botones para Guardar, Cancelar, Ayuda, etcétera.

Cuando invocas el formulario de edición seleccionando el título de un elemento de la lista, la URL contiene un id, por ejemplo, option=com_countrybase&view=country&layout=edit&id=1. El id es el número de registro de la tabla. Para un nuevo registro invocado a través del botón Nuevo de la lista, el id está ausente. Si está presente, el modelo completa el formulario de entrada de datos con los datos del registro existente.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### La barra de herramientas

La función addToolbar() se utiliza normalmente para establecer el título de la página y añadir los botones apropiados para los permisos de acceso de la persona que utiliza el formulario. Observe el uso de la variable \$isNew basada en el id del registro para modificar ligeramente la salida.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Notas:**

- **hidemainmenu** se establece en verdadero para todos los formularios de edición para evitar salir del formulario a través del menú del Administrador sin guardar.
- **ToolbarHelper::title()** establece el título en la barra de título de la página, en este caso a Editar País o Agregar País.
- Botones **toolbar-\>action**, donde action puede ser guardar, aplicar, cancelar o uno de muchos otros, toman (controlador.función) como argumentos. Así que en este caso, cuando se selecciona un botón, la acción se pasa a una función de guardar o aplicar en la clase CountryController.
- **cancel** toma un argumento adicional, la clave de cadena utilizada para etiquetar el botón.
- **inlinehelp()** muestra un botón que activa o desactiva descripciones de campos debajo de cada campo de entrada de datos.

## forms/country.xml

Este archivo contiene la especificación de los campos del formulario, generalmente en el orden presentado en el formulario de edición. La mayoría de los campos son autoexplicativos, por lo que solo se muestran dos a continuación. Los valores de nombre son los nombres de columna utilizados en las tablas de la base de datos.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Model/CountryModel.php

Las funciones en este archivo son bastante estándar. Sin embargo, la función getTable() requiere alguna explicación.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

¡Esta función no se refiere a la tabla en sí! Se refiere al constructor de src/Table/CountriesTable.php. Esto puede ser un poco confuso porque se está llamando desde la clase CountryModel.

## src/Table/CountriesTable.php

Así es donde se define el nombre de la tabla para la lista de países.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Observe la declaración del alias de la columna. Esto se usa porque el formulario utiliza un campo de entrada de datos llamado **published**, pero el campo utilizado para el almacenamiento se llama **status**.

## tmpl/country/edit.php

Este es el archivo que crea la salida HTML. Comienza con dos ayudas: HTMLHelper::_('behavior.formvalidator'); carga el javascript necesario para la validación de formularios del lado del cliente. Aunque los navegadores modernos aplican la validación de formularios, eso no impide el envío del formulario. HTMLHelper::_('behavior.keepalive'); carga javascript para hacer ping al servidor y evitar la expiración de la sesión mientras se completan formularios complejos.

**Notas:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); representa un campo de título estándar de Joomla.
- \$this-\>form-\>renderField('iso_2'); representa el campo especificado, en este caso iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); representa los campos en el lado derecho de la pestaña Detalles.

```php
<?php

/**
 * @package     Countrybase.Administrator
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

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Puedes recorrer los conjuntos de campos de formulario y los campos dentro de cada conjunto de campos. Eso puede simplificar la salida de formularios complejos con muchas pestañas.

![country edit form](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Traducido por openai.com*


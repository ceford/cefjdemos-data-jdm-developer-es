<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Anatomía: Archivos de Inicio del Administrador -->

## Descripción general de los archivos del administrador

Hay más archivos en la parte del administrador del componente, en parte porque hay dos tablas, cada una con una vista de lista y edición, y en parte porque algunos de los archivos controlan aspectos del comportamiento del componente tanto en las interfaces del sitio como del administrador.

La creación de código para una vista de lista se ha cubierto en la parte del tutorial que trata con los archivos del sitio, por lo que no se repetirá aquí. Esta sección tratará sobre los archivos comunes a la mayoría de los componentes. Un tutorial posterior cubrirá una de las vistas de edición necesarias para actualizar o añadir nuevos registros.

## servicios/proveedor.php

Este archivo se usa cuando la aplicación Joomla llama a ComponentHelper para renderizar el componente. Es el primer código de componente que se ejecuta tanto para el Sitio como para el Administrador. Su función es registrar el componente:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extension/CountrybaseComponent.php

La función de registro de este archivo se llama desde la declaración \$component = new CountrybaseComponent(...); en services/provider.php. Su propósito es inicializar el componente en las interfaces de Sitio y Administrador.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Este archivo establece los controles de acceso utilizados en este componente. Los elementos enumerados aquí aparecen en la pestaña Opciones de Permisos del componente.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Este archivo se utiliza para controlar si alguna opción aparece en el formulario del componente *Opciones* y en qué pestaña aparece. Para com_countrybase no hay opciones aparte de las opciones estándar de permisos de Joomla. Sería posible agregar opciones aquí, por ejemplo, sobre si mostrar u ocultar una columna particular en la pantalla de salida. Eso iría en un conjunto de campos separado llamado Opciones.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Traducido por openai.com*


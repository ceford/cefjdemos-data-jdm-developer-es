<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Validación de Formulario -->

## Introducción

Joomla tiene un script de validación del lado del cliente que puede ser utilizado y extendido por cualquier componente. Este artículo trata sobre cómo funciona y cómo usarlo. Hay cuatro validadores estándar para tipos de campos de nombre de usuario, contraseña, numérico y correo electrónico. También hay un validador de patrón de uso más general y un validador de campo obligatorio.

Tenga en cuenta que los navegadores modernos también proporcionan validación de formularios de forma predeterminada. La validación del navegador se puede desactivar incluyendo novalidate en la etiqueta del formulario.

## Cómo invocar la validación

En la parte superior del archivo edit.php que contiene el código de generación de formularios incluye la llamada para cargar el archivo javascript del validador:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
    ->useScript('form.validate');
```

en la etiqueta form asegúrate de incluir class="form-validate". Este ejemplo es del formulario de edición de usuario:

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
    method="post" name="adminForm" id="user-form"
    enctype="multipart/form-data"
    aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
    class="form-validate">
```

En el archivo de formulario XML, incluya las declaraciones que invocan la validación de campo individual. Este ejemplo es el campo de correo electrónico del usuario:

```xml
        <field
            name="email"
            type="email"
            label="JGLOBAL_EMAIL"
            required="true"
            size="30"
            validate="email"
            validDomains="com_users.domains"
        />
```

## Las Expresiones de Validación

Esta sección está destinada a proporcionar un poco de comprensión sobre las expresiones de validación para ayudar a explicar su uso. Todas están incluidas en el código validator.js.

### Nombre de usuario

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Aquí la expresión busca la aparición de cualquiera de los caracteres alternativos dentro de la clase de caracteres [] y devuelve falso (no válido) si se encuentra alguno.

### Contraseña

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Aquí la expresión busca un carácter inicial que no sea un espacio en blanco, seguido de 2 a 98 caracteres que no sean un espacio en blanco o un espacio, y terminando con un carácter que no sea un espacio en blanco. Joomla tiene un verificador de contraseñas más sofisticado que asegura una mezcla de tipos de caracteres y una longitud mínima.

### Correo electrónico

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Aquí la expresión regular acepta cualquier cadena que comience con uno o más de los caracteres en la clase de caracteres [], seguida de @, seguida de uno o más caracteres en la segunda clase de caracteres [], seguida de cero o más puntos seguidos de uno o más caracteres en el grupo de la tercera clase de caracteres, y terminando con uno de los grupos después de la @.

Aunque técnicamente correcto, este validador permite algunas direcciones de correo electrónico bastante ridículas como !@me - es posible que desees crear un patrón más restrictivo.

### Numérico

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Aquí el valor debe comenzar con un dígito o signo menos cero o una vez, seguido por un dígito o una coma cero o más veces, seguido por un punto decimal cero o una vez, terminando con un dígito cero o más veces. Así que esto coincidirá con 3.142 o -10 o 100,000.0 y así sucesivamente. El número real debe coincidir con el tipo de campo en la base de datos. La expresión no coincidirá con números con exponentes, por lo que 10e2 será rechazado.

### Patrón

Supongamos que quieres que un campo de entrada sea un número entero positivo. Podrías usar tu propio patrón colocado en el archivo xml del formulario:

```xml
        <field
            name="number"
            type="text"
            label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
            description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
            class="w-auto"
            required="true"
            pattern="\d+"
        />
```

Aquí el patrón permite uno o más dígitos. Así que -1 sería inválido, al igual que 3.142.

### Requerido

Asegúrese de incluir required="true" en la especificación del campo o la etiqueta del formulario omitirá el marcador * comúnmente utilizado para indicar campos obligatorios. Incluir required en la clase no es suficiente para la validación.

*Traducido por openai.com*


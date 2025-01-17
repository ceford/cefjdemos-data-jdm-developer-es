<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Sistemas de Archivos en la Nube para Gestor de Medios -->

<span id="main-portal-heading">GSoC 2017
Sistemas de Archivos en la Nube para el Gestor de Medios
Documentación</span> Joomla! 4.x

## Introducción

**Joomla! 4.x** se entrega con sistemas de archivos en la nube para el **Gestor de Medios** por defecto. Con la API anterior, crear sistemas de archivos personalizados era una tarea difícil. Gracias a la nueva API, ahora es fácil crear un sistema de archivos personalizado. Si deseas usar un Servicio en la Nube con el nuevo Gestor de Medios, se aconseja usar **OAuth2.0**.

Este documento te guiará a través de pasos importantes para crear tu propio **Plugin de Sistema de Archivos** para ampliar el **Gestor de Medios**. Antes de continuar, asegúrate de tener el conocimiento básico sobre cómo desarrollar un plugin para Joomla. [Este tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla") debería ayudarte.

## Crea tu complemento de sistema de archivos

### Configuración

En primer lugar, necesitamos crear un plugin de **sistema de archivos** para extender el Gestor de Medios. Este plugin debe contener algunos atributos importantes para que pueda funcionar con el Gestor de Medios.

Asegúrate de que tu plugin contenga `group="filesystem"`. Entonces, en tu
`[plugin-name].xml`, deberías tener:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

El parámetro **display_name** ayuda al Administrador de Medios a mostrar el nombre de su **Sistema de Archivos** como un nodo raíz en el Navegador de Archivos. Cualquier adaptador que pertenezca a su Sistema de Archivos se mostrará como un elemento secundario bajo la raíz.

Incluya cualquier parámetro necesario que pueda necesitar, como `App Secret` con campos de formulario adecuados.

### Eventos de Complementos

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Cualquier** complemento del sistema de archivos debe contener un evento llamado `onFileSystemGetAdapters()` para su funcionalidad. Este evento debe devolver un **array** de `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` cuando se llame. El evento se activa cuando abres el Administrador de Medios. Cada `AdapterInterface` se montará bajo el nodo raíz de tu sistema de archivos.

#### onFileSystemOAuthCallback()

Este evento se dispara cuando utilizas el **OAuthCallbackHandler** del Administrador de Medios para el proceso de Autorización y Autenticación OAuth2.0 descrito a continuación en el documento. El evento solo se dispara en el complemento solicitado. Por lo tanto, no es necesario verificar `$context` en un escenario típico.

Un ejemplo de uso del evento se ve así:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** contiene la entrada reenviada al URI OAuthCallback del Media Manager. `$event->getInput()` devuelve un Objeto de Entrada.

`$result` define cómo se comporta Joomla después de un redireccionamiento al callback. Hay varias soluciones posibles disponibles:

- `action`
  - close: Cierra una ventana que se abre con un JavaScript **usando window.open()**
  - redirect: Redirige a la página especificada por el **redirect_uri**
  - control-panel: Redirecciona al panel de control
  - media-manager: Redirecciona al Administrador de Medios
- `redirect_uri`
  - Se utiliza con la acción **redirect** (requerido)
- `message`
  - Mensaje que necesita mostrarse después de una acción
- `message_type`
  - warning
  - notice
  - error
  - message(o dejar vacío)

Después de configurar todo, establece el argumento `$result` del `$event` para devolverlo al llamador.

Un ejemplo para mostrar un mensaje al usuario sería:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Esto redirigirá al Administrador de Medios y mostrará un aviso con el texto en **mensaje**.

Este método se puede utilizar típicamente para obtener códigos de autorización para el proceso de **OAuth2.0**. En caso de cualquier **error**, Joomla! volverá al panel de control y mostrará un mensaje de error.

## Autenticación y Autorización

Joomla! 4.x te aconseja usar OAuth2.0 para este proceso. Es un escenario común que OAuth2.0 requiera un **url de redirección** para pasar los datos de autenticación a la aplicación. Esto generalmente se hace mediante un manejador de devolución de llamada. Para tu complemento, no es necesario reinventar la rueda, puedes usar el **Manejador de Devolución de Llamada** del **Gestor de Medios** para cumplir con la tarea.

Todo lo que tienes que hacer es establecer el **URI de redirección** a lo siguiente en tu proveedor de OAuth2.0 e implementar el 
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
en tu complemento.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

En este ejemplo:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Ahora, cuando realices una redirección desde tu proveedor una vez que llegue a la URL proporcionada, el **Controlador** para el Administrador de Medios se asegurará de que tu complemento implemente `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` antes de pasarle cualquier dato. Si no, serás redirigido al Panel de Control con un mensaje de error.

Si has implementado todas las entradas que se pasan, el Proveedor de la Nube se incluirá en el `$event`. Puedes acceder a ellas usando `$event->getInput()` y tratarlas como una `input` habitual en Joomla.

Después de recibir el `input`, puedes usarlo para obtener los detalles de
tu servicio en la nube, como **Access Token**, **Refresh Token**, etc.

Si deseas distinguir varias cuentas, puedes usar `Session` para eso.

**Nota**: Se recomienda verificar el **token CSRF** antes de proceder con su solicitud. La mayoría de los proveedores de servicios en la nube admiten el parámetro `state` que se puede utilizar para cumplir con esta tarea.

### Errores Comunes

Es necesario `urlencode()` su URI de redirección cuando lo envíe al proveedor de la nube. Por favor, utilícelo para evitar comportamientos incorrectos de su nube.

## Servir archivos desde tu adaptador

Para servir tus archivos multimedia desde el Administrador de Medios al **sitio Joomla!**
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` te ayuda.
Esta interfaz contiene un método especial llamado `getUrl($path)`.

`$path : La ruta es relativa a tu raíz`

La intención del método es proporcionar una **URL Pública Absoluta** para el archivo que solicitaste insertar en el sitio. Por ejemplo, supongamos que la ruta de tu archivo es `/path/to/me.png` en el servidor en la nube. Ahora, una URL pública podría ser algo como `mycloud.com/share/u/myusername/path/to/me.png`. Cómo se sirve, depende completamente de ti. Cuando un usuario quiere insertar una URL generada de medios, se utilizará este método.

Se pueden encontrar más detalles sobre `Adapter Interface` en `administrator/componenents/com_media/Adapter/AdapterInterface.php`.

*Traducido por openai.com*


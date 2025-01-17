<!-- Filename: J4.x:Web_Assets / Display title: Activos Web -->

## Acerca de los Activos Web

Hay una descripción completa de los Activos Web en la Documentación para Programadores de Joomla!, reproducida en [este sitio](jdocmanual?article=docus/web-asset-manager/index) o disponible desde la [fuente original](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Ejemplos Adicionales

En Jdocmanual, muchos de los artículos contienen bloques de código que se benefician del resaltado de sintaxis. Así es como se logra:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

¡Este código está en el archivo `default.php` que se utiliza para mostrar esta página! Es un poco complicado de implementar porque la llamada `hljs.highlightAll()` debe realizarse después de que se hayan cargado los archivos `css` y `js` y nuevamente cada vez que el contenido de la página se modifique con una llamada de JavaScript.

¡Otros resaltadores de sintaxis están disponibles!

*Traducido por openai.com*


<!-- Filename: J4.x:Http_Header_Management / Display title: Encabezados HTTP -->

## Introducción

Joomla 4 introdujo un sistema de encabezados HTTP diseñado para ayudar a los propietarios de sitios a configurar encabezados de seguridad HTTP. Se implementa utilizando un complemento *System - HTTP Headers*. Hay un tutorial completo para [usuarios](jdocmanual?article=user/security/http-headers) sobre este tema. Este tutorial es más corto y cubre algunos puntos relevantes para los desarrolladores.

## El Sistema - Plugin de Cabeceras HTTP

### Pestaña del plugin

Navegue a **Sistema → Plugins → Sistema - HTTP Headers** para acceder al formulario de configuración del plugin.

![System http headers plugin form](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame Options** Esto está habilitado de forma predeterminada, pero la [documentación](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) indica que está en desuso y se debería utilizar una política de *frame-ancestors* en su lugar.
- **Referrer-Policy** El valor predeterminado es *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** El valor predeterminado de Joomla es `same-origin`.
- **Force HTTP Headers** No hay encabezados forzados establecidos por defecto. Aquí es donde se puede especificar una alternativa a *X-Frame Options*. El valor de `Content-Security-Policy` puede ser uno de los siguientes:
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

Usando el subformulario **Forzar encabezados HTTP**, también puedes forzar los siguientes encabezados:

- [Política de Seguridad de Contenidos](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Política de Seguridad de Contenidos-Solo Informe](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Política de Apertura de Origen Cruzado](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Esperar-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Política de Características y Política de Permisos](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Registro de Respuestas de Red)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Política de Referente](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Informar-a](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Seguridad Estricta de Transporte](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [Opciones de Marco-X](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Pestaña de Strict-Transport-Security (HSTS)

![strict transport security settings](../../../en/images/security/security-http-headers-hsts.png)

Utilice el botón *Alternar ayuda en línea* para obtener información sobre cada parámetro. Referencia ilustrada:

[Formulario para verificar o establecer el estado de precarga de HSTS y elegibilidad](https://hstspreload.org/)

### Pestaña de Política de Seguridad de Contenidos (CSP)

![Content security policy options](../../../en/images/security/security-http-headers-csp.png)

Una vez habilitado, puedes establecer el cliente donde deseas aplicar la CSP configurada, permitiéndote establecer `site`, `administrator` o `both`. Se debe aplicar una CSP tanto en el frontend como en el backend. Referencias ilustradas:

- [Política de Seguridad de Contenidos](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Hashes de Script y Estilo](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- Las descripciones de las directivas de política src están disponibles en el menú de esta página.

La opción final llamada *Agregar Directiva* te permite configurar la lista de permitidos por directiva según tus necesidades. Por ejemplo, para la directiva *script-src* el campo *Valor* debe contener los orígenes desde los que deseas permitir la carga de scripts.

## Notas

Cuando haya configurado algunos encabezados de seguridad HTTP directamente en el servidor, puede haber entradas duplicadas.

Revisa la salida de tus encabezados HTTP en la consola del navegador. En Google Chrome o Firefox: Inspeccionar → Red → la salida bajo Encabezados. Luego puedes desactivar los encabezados que causan entradas duplicadas. También revisa la consola de tu navegador para posibles errores.

## Desarrolladores de Extensiones

La gran ventaja de una Política de Seguridad de Contenidos ocurre cuando el Encabezado bloquea todo el JavaScript en línea y el CSS en línea que afecta a los manejadores de eventos de JavaScript a través de atributos HTML. Con la protección del navegador habilitada, el JavaScript en línea y el CSS en línea también son bloqueados para las extensiones. Esa protección no está habilitada por defecto, pero puede ser habilitada por los usuarios.

Donde se requieren JavaScript y CSS en línea, el soporte para nonce y hash está incluido en las APIs del Documento. Cuando se usan, el núcleo se asegurará de que estén en la lista blanca, pero aún bloqueará el código malicioso.

### Notas importantes para desarrolladores de extensiones

A partir de Joomla 4.0, la Política de Seguridad de Contenidos:

- se envía con el núcleo.
- está deshabilitado por defecto.
- puede ser habilitado por los administradores del sitio.
- se recomienda encarecidamente que el frontend y el backend de su extensión funcionen con la Política de Seguridad de Contenidos habilitada.

Con una Política de Seguridad de Contenidos estricta habilitada, se bloquearán las siguientes funciones:

- La ejecución de JavaScript a través de los controladores de eventos HTML (controladores onXXX como onClick y similares).
- La ejecución de JavaScript en página no pasado a la página a través de la API del Documento.
- La ejecución de código JavaScript inyectado en APIs DOM tales como eval().
- El uso de CSS en página en línea no pasado a la página a través de la API del Documento.
- El uso de CSS en línea utilizando el atributo de estilo HTML.

Para que tus extensiones funcionen incluso con una Política de Seguridad de Contenidos estricta habilitada, la forma más sencilla es usar la API de Document para aplicar tu JavaScript y CSS en línea. Ve los ejemplos a continuación.

### Añadiendo JavaScript utilizando la API de Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### Añadir CSS usando la API de Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Recursos adicionales

- [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Introducción a Content Security Policy](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) es parte de Probely y fue creado originalmente por Scott Helme. ¡Es una herramienta gratuita y fácil de usar diseñada para ayudarte a implementar y comprender mejor las funciones de seguridad modernas disponibles para tu sitio web!
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Fundamentos Web Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Documentación de CSP de Google](https://csp.withgoogle.com/docs/index.html)
- [¡CSP está muerto, larga vida a CSP!](https://research.google/pubs/pub45542/) Sobre la inseguridad de las listas blancas y el futuro de Content Security Policy
- [búsqueda de seguridad en web.dev](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Traducido por openai.com*


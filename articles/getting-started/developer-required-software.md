<!-- Filename: J4.x:Developer:_Required_Software / Display title: Software requerido -->

## Introducción

Este tutorial está dirigido a los recién llegados al desarrollo de código de Joomla que desean preparar extensiones en una computadora local. No importa si usas Windows, Mac o Linux. Todo el software requerido está disponible para todas estas plataformas. Para comenzar en tu propia computadora portátil o de escritorio, que generalmente tiene el nombre de dominio *localhost*, necesitarás instalar un conjunto estándar de elementos de software separados.

- **Apache**, un servidor web. Hay otros servidores web disponibles, pero los principiantes deben quedarse con Apache.
- **MySQL** o **MariaDB**, servidores de bases de datos. PostgreSQL también es compatible, pero no es para principiantes.
- **PHP**, la última versión recomendada por Joomla o la versión mínima para tu plataforma.
- **phpMyAdmin**, una herramienta utilizada para gestionar bases de datos MySQL y MariaDB.
- **xDebug**, una extensión de PHP utilizada para depuración.
- Un IDE como **VSCode**. Hay otros disponibles y se cubren en un artículo separado.

## Pilas de Software

Los primeros cuatro elementos de esta lista a menudo se conocen como un conjunto y pueden ser llamados LAMP, MAMP o WAMP, donde las letras en el acrónimo significan lo siguiente:

- **L, M o W** plataforma. L para Linux, M para Mac y W para Windows.
- **A: Apache** servidor web.
- **M: MySQL o MariaDB** base de datos. Los dos son intercambiables.
- **P: PHP** lenguaje de scripting. Un lenguaje de scripting ampliamente utilizado. No hay alternativa ya que Joomla está codificado en PHP.

## Pilas Empaquetadas

Una buena manera de comenzar es utilizando un paquete que combina el software esencial:

- **WAMP** para Windows está disponible de forma gratuita en el sitio de [Wampserver](https://www.wampserver.com/en/).
- **Bearsampp** para Windows está disponible de forma gratuita en el sitio de [Bearsampp](https://bearsampp.com/). Tiene más herramientas.
- **XAMPP** Para Windows, Mac y Linux está disponible de forma gratuita en el sitio de [Apache Friends](https://www.apachefriends.org/). Hay un tutorial local para [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** para Mac y Windows está disponible en versiones gratuitas y comerciales en el sitio de [MAMP](https://www.mamp.info/en/mac/).

## Sin Pilas

Si tienes una computadora Linux o Macintosh, verás que puedes instalar cada uno de los elementos de software requeridos de manera independiente desde los repositorios remotos que soportan tu sistema operativo. Es posible que ya estén instalados y listos para usar. Para probar, abre tu navegador web favorito e ingresa **localhost** en la barra de direcciones. Verás una página de marcador de posición o una página de error de conexión.

## Directorio raíz de documentos del servidor web

Al instalarse, su servidor Apache habrá configurado un directorio raíz de documentos predeterminado. La ubicación varía según la plataforma y necesita saber dónde se encuentra o crear un host virtual para colocarlo donde desee. Ejemplos de ubicaciones predeterminadas:

- Mac OS: "/Library/WebServer/Documents"
- Linux: /var/www/html
- Windows: ...

Para evitar problemas posteriores con los permisos de archivos, a menudo es conveniente crear un host virtual que apunte al directorio public_html de tu propio espacio de archivos. Eso podría ser /home/username/public_html en Linux o /Users/username/Sites en Mac.

Este es un ejemplo de entrada de host virtual de Mac en el archivo /etc/apache2/vhosts/localhost.conf:

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Alternativamente, es posible que puedas crear un enlace simbólico desde el directorio raíz predeterminado al folder public_html de tu espacio personal de archivos.

*Traducido por openai.com*


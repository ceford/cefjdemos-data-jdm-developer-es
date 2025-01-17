<!-- Filename: J4.x:SCSS_and_Sass / Display title: Probando CSS y JavaScript -->

## Introducción

En un sitio Joomla de producción, Joomla entrega archivos CSS y Javascript comprimidos (aquellos con `.min` en el nombre de archivo). Los servidores enviarán las versiones comprimidas con gzip si están disponibles (aquellas que terminan con un sufijo `.gz`). Estos archivos se crean a partir de fuentes sin comprimir. En el caso de los archivos CSS, a menudo se crean procesando precursores SCSS. Para probar el código que incluye CSS o JavaScript nuevos o actualizados, es necesario reconstruir los archivos CSS y las versiones comprimidas y minificadas a partir de las fuentes modificadas.

## Probar la Instalación

Para fines de prueba, necesitas un servidor de prueba que tenga instalados Git, Node.js y Composer. Ve al repositorio de [Joomla CMS](https://github.com/joomla/joomla-cms) en GitHub y sigue las instrucciones en *Cómo obtener una instalación funcional desde el código fuente* cerca del final del texto README.

Al completar la instalación de Joomla, ¡no elimines el Directorio de Instalación! Eso también eliminará el directorio `build` que se necesita para reconstruir archivos CSS y JavaScript.

Los archivos principales `.scss` están en los siguientes directorios:

- media/templates/site/cassiopeia/scss
- media/templates/administrator/atum/scss

El script de generación CSS, el compilador SCSS y otras herramientas de construcción similares están ubicados en el directorio `build/`. El directorio de construcción solo está disponible desde la fuente de Joomlaǃ, no está incluido en una versión oficial de Joomlaǃ.

Las instrucciones de instalación incluyen lo siguiente:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

El comando `npm ci` es un sinónimo de `npm clean-install` utilizado en cualquier situación en la que quieras asegurarte de estar realizando una instalación limpia de tus dependencias.

## Uso diario

Hay comandos alternativos para usar en situaciones donde solo se han cambiado archivos SCSS o JavaScript:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

Los comandos examinan los directorios de medios y plantillas y crean las versiones finales de los archivos requeridos por una instalación de Joomla.

## Sass, SCSS y CSS

> Sass es un lenguaje de hojas de estilo que se compila a CSS. Permite utilizar variables, reglas anidadas, mixins, funciones y más, todo con una sintaxis completamente compatible con CSS. Sass ayuda a mantener las hojas de estilo grandes bien organizadas y facilita compartir el diseño dentro y entre proyectos. Los archivos Sass tienen el sufijo `scss`.

### CSS

El código CSS se expresa de la siguiente maneraː

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` es una extensión de la sintaxis de CSS. Se utiliza en el núcleo de Joomlaǃ

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

Una sintaxis más antigua de `Sass` ofrece una forma más concisa de escribir CSS.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

Puedes encontrar más información en la [Documentación de Sass](http://sass-lang.com/documentation/syntax/).

## De CSS existente a SCSS

Si deseas personalizar la plantilla Cassiopeia, es una buena idea copiar la plantilla y personalizarla después para que las actualizaciones de Joomla! no sobrescriban tus personalizaciones. Para añadir tus propios archivos CSS a una copia de la plantilla Cassiopeia, simplemente renombra tus archivos `.css` a `.scss`. Así podrás hacer uso de las características que SCSS ofrece:

- Extensiones de lenguaje como variables, anidamiento y mixins
- Muchas funciones útiles para manipular colores y otros valores
- Funciones avanzadas como directivas de control para bibliotecas
- Salida bien formateada y personalizable

Consulta el [sitio de Sass](https://sass-lang.com/) para obtener todos los detalles.

### Importa tus archivos .css como archivos .scss

Suponiendo que deseas incluir tus archivos personalizados y que sean procesados por el compilador SCSS, NO necesitas reescribir tu .css en SCSS porque el `.css` simple también funciona. Lo que tienes que hacer:

- renombra tus archivos `.css` a `.scss`  
- ponles un prefijo con un `_`  
- añade una declaración `@import` al final de `media/templates/site/copy_of_cassiopeia/scss/template.scss` para que sobrescriba las declaraciones existentes:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Si ahora compilas tu archivo main template.scss, terminas con un archivo principal template.css optimizado y minimizado. También has reducido tus solicitudes http y el sitio debería cargar más rápido.

*Traducido por openai.com*


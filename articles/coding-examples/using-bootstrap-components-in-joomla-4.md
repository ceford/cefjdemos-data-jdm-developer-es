<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Usar componentes de Bootstrap en Joomla 4 -->

## Introducción

### Componentes de Bootstrap

Algunos de los componentes descritos en la documentación de Bootstrap utilizan solo CSS. Por ejemplo, el componente de Breadcrumbs se renderiza con CSS y no requiere soporte de JavaScript. Otros responden a acciones del usuario como hacer clic o pasar el ratón por encima, y necesitan soporte de JavaScript. A estos últimos se les denomina aquí Componentes Interactivos. Este artículo explica cómo usarlos en Artículos y un Módulo Personalizado.

Consulta la [Documentación de Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Joomla 4 introduce un enfoque modular para componentes interactivos

- ¿Qué es un enfoque modular?
- La funcionalidad se descompone en componentes individuales respaldados por archivos individuales. No hay un enfoque de **un archivo** como sucedía con Bootstrap en Joomla 3. El enfoque modular es más eficiente y ofrece ganancias de rendimiento (envía solo el código que se necesita en lugar de entregar todo por si alguna página necesitara algún componente).

### Uso de componentes interactivos: Codificadores

- ¡Carga lo que necesitas por caso! Hay funciones auxiliares para configurar componentes individuales con los argumentos apropiados.
- Consulta la lista de Componentes Interactivos de Bootstrap.

### Uso de Componentes Interactivos: No Programadores

- Usar componentes en artículos no es tan fácil porque las funciones auxiliares no se pueden llamar desde un artículo o un módulo estándar de HTML personalizado. Se sugieren tres posibles soluciones más adelante en este tutorial:
  - Un módulo personalizado
  - Un plugin personalizado
  - Una sobrescritura de módulo personalizado
- Omite o revisa la lista de Componentes Interactivos de Bootstrap. No estarás utilizando estas llamadas de función directamente.

## Componentes Interactivos de Bootstrap

### Alerta

Suponiendo que ya tienes la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- El **.selector** se refiere al selector CSS para la alerta. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- No hay opciones adicionales disponibles.

### Botón

Suponiendo que ya tienes la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- El **.selector** se refiere al selector CSS para el botón. Puedes llamar a esta función múltiples veces con diferentes selectores CSS.
- No hay opciones adicionales disponibles.

### Carrusel

Suponiendo que ya tienes la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el carrusel. Puedes llamar a esta función múltiples veces con diferentes selectores CSS
- El tercer argumento se refiere a las opciones disponibles para el carrusel

Opciones para el carrusel pueden ser:

- **interval**, número, predeterminado:**5000**, La cantidad de tiempo para retrasar entre el cambio automático de un elemento. Si es falso, el carrusel no cambiará automáticamente.
- **keyboard**, booleano, predeterminado:**true** Si el carrusel debe reaccionar a eventos de teclado.
- **pause**, cadena\|booleano, **hover** Pausa el ciclo del carrusel al pasar el ratón y reanuda el ciclo al dejar el ratón.
- **slide**, cadena\|booleano, predeterminado:**false** Reproduce automáticamente el carrusel después de que el usuario cambia manualmente el primer elemento. Si es "carousel", reproduce automáticamente el carrusel al cargar.

### Colapso

Suponiendo que ya tengas la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el colapso. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el colapso.

Opciones para el colapso pueden ser:

- **parent**, string, defecto:**false** Si se proporciona el elemento principal, entonces todos los elementos colapsables bajo el elemento principal especificado se cerrarán cuando se muestre este elemento colapsable.
- **toggle**, booleano, defecto:**true** Alterna el elemento colapsable en la invocación.

### Desplegable

Suponiendo que ya tienes la parte HTML en tu Diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el desplegable. Puede llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el desplegable.

Opciones para el colapso pueden ser:

- **flip**, booleano, predeterminado:**true** Permite que el desplegable se voltee en caso de solapamiento sobre el elemento de referencia.
- **boundary**, cadena, predeterminado:**scrollParent** Límite de restricción de desbordamiento del menú desplegable.
- **reference**, cadena, predeterminado:**toggle** Elemento de referencia del menú desplegable. Acepta **toggle** o **parent**.
- **display**, cadena, predeterminado:**dynamic** Por defecto, usamos Popper para el posicionamiento dinámico. Desactívalo con **static**.

### Modal

Suponiendo que ya tiene la parte HTML en su diseño, también necesitará incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el modal. Puedes llamar a esta función múltiples veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el modal.

Opciones para el modal pueden ser:

- **backdrop**, string\|boolean por defecto:**true** Incluye un elemento modal-backdrop. Alternativamente, especifique estático para un telón de fondo que no cierre el modal al hacer clic.
- **keyboard**, booleano por defecto:**true** Cierra el modal cuando se presiona la tecla de escape.
- **focus**, booleano por defecto:**true** Cierra el modal cuando se presiona la tecla de escape.

### Fuera del lienzo

Suponiendo que ya tiene la parte HTML en su diseño, también necesitará incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el offcanvas. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el offcanvas.

Opciones para el offcanvas pueden ser:

- **backdrop**, booleano, predeterminado:**true** Aplica un fondo al cuerpo mientras el offcanvas está abierto.
- **keyboard**, booleano, predeterminado:**true** Cierra el offcanvas cuando se presiona la tecla de escape.
- **scroll**, booleano, predeterminado:**false** Permitir el desplazamiento del cuerpo mientras el offcanvas está abierto.

### Popover

Suponiendo que ya tiene la parte HTML en su diseño, también necesitará incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el popover. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el popover.

Opciones para el popover pueden ser:

- **animation**, booleano, por defecto:**true** Aplica una transición de desvanecimiento CSS al popover.
- **container**, cadena\|booleano, por defecto:**false** Añade el popover a un elemento específico. Ej.: **body**.
- **content**, cadena, por defecto:**null** Valor de contenido predeterminado si no está presente el atributo data-bs-content.
- **delay**, número, por defecto:**0** Retraso al mostrar y ocultar el popover (ms) no se aplica al tipo de disparo manual.
- **html**, booleano, por defecto:**true** Inserta HTML en el popover. Si es **false**, la propiedad innerText se usará para insertar contenido en el DOM.
- **placement**, cadena, por defecto:**right** Cómo posicionar el popover - **auto** \| **top** \| **bottom** \| **left** \| **right**. Cuando se especifica auto, reorientará dinámicamente el popover.
- **selector**, cadena, por defecto:**false** Si se proporciona un selector, los objetos popover serán delegados a los objetivos especificados.
- **template**, cadena, por defecto:**null** HTML base para usar al crear el popover.
- **title**, cadena, por defecto:**null** Valor de título predeterminado si no está presente la etiqueta **title**.
- **trigger**, cadena, por defecto:**click** Cómo se activa el popover - **click** \| **hover** \| **focus** \| **manual**.
- **offset**, entero, por defecto:**0** Desplazamiento del popover en relación con su objetivo.

### Scrollspy

Suponiendo que ya tiene la parte HTML en su diseño, también necesitará incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el scrollspy. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para scrollspy.

Opciones para Scrollspy pueden ser:

- **desplazamiento** número de píxeles para desplazar desde la parte superior al calcular la posición del desplazamiento.
- **método** cadena Encuentra en qué sección se encuentra el elemento espiado.
- **objetivo** cadena Especifica el elemento para aplicar el plugin Scrollspy.

### Pestaña

Suponiendo que ya tienes la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- El **.selector** se refiere al selector CSS para la pestaña. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para la pestaña.

Opciones para la pestaña pueden ser:

### Información sobre herramientas

Suponiendo que ya tienes la parte HTML en tu diseño, también necesitarás incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el tooltip. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el tooltip.

Opciones para el tooltip pueden ser:

- **animation**, boolean aplica una transición de desvanecimiento de css al popover.
- **container**, string\|boolean Añade el popover a un elemento específico: { container: **body** }.
- **delay**, number\|object retrasa la aparición y desaparición del popover (ms) - no se aplica al tipo de disparador manual Si se proporciona un número, el retraso se aplica tanto al ocultar como al mostrar La estructura del objeto es: delay: { show: 500, hide: 100 }.
- **html**, boolean Inserta HTML en el popover. Si es falso, se usará el método de texto de jQuery para insertar contenido en el dom.
- **placement**, string\|function cómo posicionar el popover - **top** \| **bottom** \| **left** \| **right**.
- **selector** string Si se proporciona un selector, los objetos popover se delegarán a los objetivos especificados.
- **template**, string HTML base para usar al crear el popover.
- **title**, string\|function valor de título por defecto si no está presente la etiqueta **title**.
- **trigger**, string cómo se dispara el popover - hover \| focus \| manual.
- **constraints**, array Un arreglo de restricciones - pasadas a Popper.
- **offset**, string Desplazamiento del popover en relación con su objetivo.

### Toast

Suponiendo que ya tenga la parte HTML en su diseño, también necesitará incluir la interactividad (la parte de JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- El **.selector** se refiere al selector CSS para el toast. Puedes llamar a esta función varias veces con diferentes selectores CSS.
- El tercer argumento se refiere a las opciones disponibles para el toast.

Opciones para el brindis pueden ser:

- **animation**, booleano, por defecto:**true** Aplica una transición de desvanecimiento CSS al toast.
- **autohide**, booleano, por defecto:**true** Oculta automáticamente el toast.
- **delay**, número, por defecto:**5000** Retraso en ocultar el toast (ms).

### Ver también

- **Acordeón** utiliza Collapse para mostrar paneles de datos.
- **Grupo de lista** se puede combinar con Tab para mostrar contenido en pestañas.

## Uso de componentes de Bootstrap en artículos

El marcado HTML para la mayoría de los componentes puede incluirse en un artículo o un módulo que puede ser incluido en un artículo. El inconveniente es que la llamada HTMLHelper para configurar el soporte de JavaScript no puede incluirse allí. Hay varios enfoques posibles para este problema. Aquí se sugieren tres enfoques: usar un módulo personalizado, usar un complemento o usar una sobrecarga de plantilla.

**Precaución:** ¡Los editores TinyMCE y JCE eliminan los espacios en blanco al guardar y dificultan la edición del código! La solución sencilla es ir a la parte superior derecha de la pantalla del Administrador y seleccionar **Menú de Usuario → Editar Cuenta** y configurar el Editor a Code Mirror.

## Enfoque 1: Usar un Módulo Personalizado

Esta es probablemente la aproximación menos propensa a errores porque las opciones de soporte del componente Bootstrap se seleccionan con casillas de verificación. Los pasos involucrados son los siguientes:

- Descarga, instala y habilita este [Módulo Personalizado](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip)
- Desde el menú del Administrador, ve a **Contenido**→** Módulos del sitio → Nuevo**
- Selecciona **Componentes BS Personalizados**
- Ingresa un Título
- Cambia el Editor a modo de texto plano y pega o escribe el código HTML para el componente que deseas usar.
- En la pestaña Opciones, desplázate hacia abajo hasta la lista de Componentes BS y selecciona el tipo de componente en esta instancia del módulo. Ten en cuenta que puedes seleccionar más de uno si estás usando más de un componente.
- Selecciona una Posición del módulo: ya sea
  - una posición definida por la plantilla si deseas usar el módulo en una ubicación específica o
  - escribe una posición si deseas usar el módulo dentro de un artículo específico: en el artículo escribe {loadposition loquesea}
- ¡Guarda y ve al sitio para probar!

### Selectores

Para algunos componentes, la acción de JavaScript se activa mediante una **clase** específica en el código HTML. En otros componentes, la acción se activa mediante un atributo **data-bs-whatever**. Los siguientes son los desencadenantes actuales y pueden cambiar:

- **Alerta** activada por class="alert ..."
- **Botón** activado por data-bs-toggle="button"
- **Carrusel** activado por data-bs-ride="whatever"
- **Colapso** activado por data-bs-toggle="collapse"
- **Desplegable** activado por data-bs-toggle="dropdown"
- **Modal** activado por data-bs-toggle="modal"
- **Fuera del lienzo** activado por data-bs-toggle="offcanvas"
- **Popóver** activado por class="btn ..." o
    - etiqueta (podría cambiarse a class="haspopover ...") Y
    - data-bs-toggle="popover"
- **Espionaje de desplazamiento** activado por data-bs-spy="scroll"
- **Pestaña** activada por data-bs-toggle="tab"
- **Brindis** activado por class="toast ..."
- **Información sobre herramientas** activada por class="btn ..." o
    - etiqueta (podría cambiarse a class="hastooltip ...") Y
    - data-bs-toggle="tooltip"

### Ejemplo 1: Alerta

Las alertas se pueden usar en el código html sin soporte de JavaScript. Esto solo es necesario para la capacidad de descartar. Ejemplo de código HTML:

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Ejemplo de resultado al incluir un módulo en un artículo:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-alert.png)

Tenga en cuenta que sin soporte de JavaScript, la alerta aparecerá exactamente como arriba, pero un clic en el botón de cerrar \[X\] no descartará la alerta. Además, la alerta aparecerá en cada carga de página.

### Ejemplo 2: Botones

Los botones pueden usarse en el código HTML sin soporte de JavaScript. Esto solo es necesario para el cambio a veces sutil de estilo aplicado a botones con un cambio de estado, estilo activo. Código de ejemplo de Bootstrap:

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

Con este estilo en la plantilla user.css:

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap buttons](../../../en/images/coding-examples/coding-examples-buttons.png)

Los botones cambian entre azul y verde.

### Ejemplo 3: Carrusel

El Carrusel ofrece una presentación de diapositivas que recorre una serie de imágenes o paneles de texto. El siguiente ejemplo utiliza imágenes de la Muestra de Joomla 4. Código de Bootstrap:

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Resultado:

![Bootstrap carousel](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Ejemplo 4: Colapso

El colapso se utiliza ampliamente en Joomla y es posible que no necesite usar un módulo o plugin para activar la acción. El clic abre un panel con información adicional. Código de ejemplo de Bootstrap:

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Resultado:

![Bootstrap collapse](../../../en/images/coding-examples/coding-examples-collapse.png)

### Ejemplo 5: Desplegable

Los menús desplegables son superposiciones contextuales que se pueden alternar para mostrar listas de enlaces y más. Código de ejemplo de Bootstrap:

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Resultado:

![Bootstrap dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Ejemplo 6: Modal

El componente Modal abre un cuadro de diálogo en el centro de la pantalla. Hay bastantes opciones para controlar el tamaño y el contenido del modal. Consulta la documentación de Bootstrap para más detalles. Ejemplo de código de Bootstrap:

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Resultado:

![Bootstrap modal](../../../en/images/coding-examples/coding-examples-modal.png)

### Ejemplo 7: Fuera del lienzo

En este momento, este componente no es compatible con Joomla. ¡Mantente atento, próximamente!

### Ejemplo 8: Resumen emergente

Los popovers son como los tooltips pero con un título. Tienen algunos problemas de accesibilidad y rendimiento, por lo que deben usarse con precaución. Ejemplo de código Bootstrap:

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Resultado:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-popover.png)

### Ejemplo 9: Scrollspy

Ejemplo de código:

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Resultado:

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

Además, se necesita algo de estilo en user.css:

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

¡Inconveniente: el menú no se coordina bien con el contenido en este ejemplo!

### Ejemplo 10: Tabulador

Las pestañas a menudo se utilizan como elementos de navegación combinados con desplegables. Código de ejemplo de Bootstrap:

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Resultado:

![Bootstrap tab](../../../en/images/coding-examples/coding-examples-tab.png)

Recuerda verificar tanto las opciones de pestaña como las de menú desplegable para que la parte desplegable funcione.

### Ejemplo 11: Tostada

Los toasts son notificaciones ligeras diseñadas para imitar las notificaciones push que han sido popularizadas por los sistemas operativos móviles y de escritorio. Están construidos con flexbox, por lo que son fáciles de alinear y posicionar. Código de ejemplo de Bootstrap:

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Resultado:

![Bootstrap toast](../../../en/images/coding-examples/coding-examples-toast.png)

Nota que la demostración de Bootstrap que utiliza un botón para mostrar el mensaje Toast necesita algo de JavaScript adicional. ¡Parece que este componente necesita un programador para hacer buen uso de él!

### Ejemplo 12: Descripción emergente

Un tooltip es un pequeño fragmento de texto que aparece al pasar el cursor sobre un botón o enlace para explicar qué es o qué hace. El tooltip puede posicionarse arriba, abajo, a la izquierda o a la derecha del elemento. Si no se especifica, la posición predeterminada es arriba. El tooltip cambiará a otra posición si no hay suficiente espacio en la posición especificada. Código de ejemplo Bootstrap:

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Resultado:

![Bootstrap tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Enfoque 2: Usar un Plugin de Contenido

Los pasos involucrados:

- Descarga, instala y habilita este [plugin](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- En el artículo agrega el texto sobre el que actúa el plugin, por ejemplo {bscompos modal carousel} activará la carga del JavaScript necesario para soportar un cuadro de diálogo modal y un carrusel. El plugin elimina el texto de activación y las etiquetas de cierre (ahora) vacías.
- Incluye el código HTML del Componente Bootstrap directamente en el artículo o en un módulo incluido en el artículo. Hay un ejemplo de código HTML a continuación para un modal simple y un modal que contiene un carrusel. Nota que esto no funcionará si el código HTML está en un módulo en una ubicación de plantilla.
- Esto también funcionará para un módulo personalizado estándar si la opción "Preparar contenido" está configurada en Sí.
- ¡Pruébalo!

## Enfoque 3: Usar una Sobrecarga de Plantilla

Los pasos involucrados:

- Crear una sobrescritura de plantilla mod_custom.
- Añadir un módulo mod_custom que contenga el marcado de componentes y las clases de activación.
- Incluir el módulo en un artículo.

### La sobrecarga de plantilla mod_custom

- En la interfaz de Administrador, vaya a **Sistema → Plantillas del Sitio → Detalles y Archivos de Cassiopeia**.
- Seleccione **Crear Sobrescrituras **→** mod_custom **→** default.php**.
- En la línea siguiente a defined('\_JEXEC') or die; agregue el siguiente código:

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

Este código busca nombres de clase establecidos en mod_custom y realiza la llamada a HTMLHelper para configurar el soporte JavaScript. Tenga en cuenta que el último elemento en cada llamada es un selector que puede o no ser utilizado para desencadenar una acción. Muchos de los componentes son activados por atributos de datos en el marcado y no utilizan los selectores aquí. Para algunos, el selector es necesario. Por ejemplo, tiene sentido usar la clase **.btn** y la etiqueta **a** para activar Tooltips.

### Un módulo mod_custom para un componente modal

- Vaya a **Contenido**→**Módulos del Sitio**→**Nuevo**.
- Seleccione el módulo **Personalizado**.
- Complete el formulario:
  - Título: Modal de Demostración
  - En el campo Posición escriba **demomodal** para usar en un artículo;
  - Contenido del Módulo: Cambie al editor para entrada de texto simple.
  - Pegue el siguiente código de la documentación de Bootstrap:

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Selecciona la pestaña Avanzado y en el campo Clase del Módulo ingresa **bs-modal**
- Opcional: establece Título en Ocultar para usar el H2 en el código pegado.
- Guardar y Cerrar (no te preocupes si el modal se ve mal en el editor).

### Crear un artículo y un elemento de menú

- Crea un nuevo artículo, Demo Modal, y en modo de entrada de texto plano establece el contenido en

```html
<div>{loadposition demomodal}</div>
```

- Crear un elemento de menú de Artículo Único.
- Probarlo:

![Bootstrap modal module in article](../../../en/images/coding-examples/coding-examples-modal-module.png)

### Un Componente Modal que Contiene un Carrusel

- Crea un nuevo módulo personalizado con un nuevo título: Demo Modal Carousel
- Posición: demomodalcarousel
- **Pestaña Avanzado**→**Clase del Módulo**: bs-modal bs-carousel
- Contenido personalizado del módulo en texto plano:

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Crea un nuevo Artículo con {loadposition demomodalcarousel} en el contenido.
- Crea un nuevo elemento de menú de artículo único: Demo Modal Carousel
- Pruébalo:

![Bootstrap modal carousel](../../../en/images/coding-examples/coding-examples-modal-carousel.png)

*Traducido por openai.com*

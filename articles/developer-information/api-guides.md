<!-- Filename: API_Guides / Display title: Guías de API -->

Esta página contiene un índice del conjunto de Guías de la API de Joomla. Estas guías tienen como objetivo ayudarle a entender cómo utilizar estas funciones de Joomla en sus propias extensiones de Joomla.

Cada guía de la API incluye código de ejemplo que puedes copiar e instalar en tu propio entorno de desarrollo. Generalmente, estos ejemplos de código están escritos para ser incluidos e instalados como un módulo de Joomla, por lo que si no estás familiarizado con el desarrollo de módulos de Joomla, puede que te resulte útil revisar la breve serie [Creando un Módulo Sencillo](https://docs.joomla.org/Creating_a_simple_module "Creating a simple module").

Alrededor de Joomla 3.8, el equipo de desarrollo de Joomla comenzó a cambiar la convención de nombres de las clases de Joomla para usar espacios de nombres, de modo que, por ejemplo, JFactory cambió a Factory en el espacio de nombres Joomla\CMS. Al leer el código y la documentación existente de Joomla, puede encontrar las clases siguiendo ya sea el estándar de nomenclatura nuevo o el antiguo. Puede encontrar el mapeo entre las dos convenciones de nombres en el archivo `libraries/classmap.php` en su instancia de Joomla.

- Las clases base de la Aplicación y su jerarquía y propósitos se describen en [Understanding the Application classes](https://docs.joomla.org/J3.x:Understanding_the_Application_classes "J3.x:Understanding the Application classes")
- El manejo de Ajax dentro de los Componentes de Joomla se describe en [JSON Responses with JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax también puede usarse dentro de los Módulos y Plugins de Joomla como se describe en [Using Joomla Ajax Interface](https://docs.joomla.org/Using_Joomla_Ajax_Interface "Using Joomla Ajax Interface").
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide "Cache Basic API Guide") - cómo usar el caché de callback dentro de tu código.
- [Categories](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide "Categories and CategoryNodes API Guide") Usar las clases `Categories` y `CategoryNode` para acceder a datos relacionados con las categorías de Joomla
- El CSS se puede agregar como se describe en [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Base de datos / JDatabase. Hay dos guías de API, que cubren [Selecting data using JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase") y [Inserting, Updating and Removing data using JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase "Inserting, Updating and Removing data using JDatabase")
- [Date/JDate](https://docs.joomla.org/How_to_use_JDate "How to use JDate") es la clase de Fecha de Joomla.
- Archivos y Carpetas. Ver [How to use the filesystem package](https://docs.joomla.org/How_to_use_the_filesystem_package "How to use the filesystem package").
- Formulario / JForm. Existe una [Basic guide](https://docs.joomla.org/Basic_form_guide "Basic form guide") para usar la API de Formularios de Joomla e incorporar formularios dentro de un componente de Joomla, y también una [Advanced form guide](https://docs.joomla.org/Advanced_form_guide "Advanced form guide") que cubre aspectos más avanzados de las APIs.
- FormField / JFormField. Esta clase y clases relacionadas como JFormFieldList, que heredan de ella, son principalmente útiles para definir campos de formulario personalizados, como se describe en [Creating a custom form field type](https://docs.joomla.org/Creating_a_custom_form_field_type "Creating a custom form field type").
- [Input / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput "Retrieving request data using JInput") Usar la clase `Input` para obtener los valores de los parámetros en solicitudes HTTP GET y POST
- Se puede agregar JavaScript como se describe en [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Usar Layouts de Joomla se describe en [Sharing layouts across views or extensions with JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). La flexibilidad aumentó en Joomla 3.2, como se describe en [JLayout Improvements for Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla! "J3.x:JLayout Improvements for Joomla!").
- [Log / JLog](https://docs.joomla.org/Using_JLog "Using JLog") Registrar mensajes (por ejemplo, mensajes de error, mensajes de depuración) en un archivo de registro, y opcionalmente en la consola de depuración
- [Menu and Menuitems](https://docs.joomla.org/Menu_and_Menuitems_API_Guide "Menu and Menuitems API Guide")
- [Nested Sets](https://docs.joomla.org/Using_nested_sets "Using nested sets"), que permiten implementar una jerarquía de árbol en la tabla de la base de datos, son utilizados por los menús, artículos, categorías, etc. de Joomla.
- [Registry/JRegistry](https://github.com/joomla-framework/registry) es una clase de utilidad muy útil para manejar matrices PHP, convertir a JSON, etc.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson") admite responder en formato JSON a solicitudes Ajax.
- Route / JRoute ver [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- Table / JTable proporciona funcionalidad para realizar operaciones CRUD (y más) en tablas de bases de datos. La guía está dividida en una [Basic API Guide](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide") y una [Advanced API Guide](https://docs.joomla.org/Table_Advanced_API_Guide "Table Advanced API Guide")
- Los [Controllers](https://docs.joomla.org/Controllers "Controllers") (BaseController, AdminController, FormController, ApiController) son responsables de analizar la solicitud del usuario, verificar que el usuario tenga permiso para realizar esa acción y determinar cómo satisfacer la solicitud.
- Los [Models](https://docs.joomla.org/Models "Models") (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) encapsulan los datos utilizados por el componente. También son responsables de actualizar la base de datos cuando corresponde.
- Las [Views](https://docs.joomla.org/Views "Views") (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) especifican lo que debe aparecer en la página web y recopilan todos los datos necesarios para generar la respuesta HTTP.
- [Tags](https://docs.joomla.org/Tags_API_Guide "Tags API Guide").
- Uri / JUri ver [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- [User / JUser](https://docs.joomla.org/Accessing_the_current_user_object "Accessing the current user object") API relacionada con el Usuario de Joomla.

*Traducido por openai.com*


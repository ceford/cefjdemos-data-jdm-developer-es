<!-- Filename: J4.x:Joomla_Core_APIs / Display title: Joomla API del núcleo -->

Joomla!  4.0

Esta página ofrece un listado de puntos de entrada (endpoints)
disponibles en Joomla mediante ejemplos con comandos curl.

Cada URL requiere autentificación a menos que sea una URL designada como
pública. Por seguridad en Joomla 4.0.0 planeamos hacer que la API de
aplicación predeterminada necesite de una cuenta de Súper Usuario (ya
que la API de aplicación es algo nuevo), este requisito se irá relajando
conforme la API se estabilice y se pruebe adecuadamente en la comunidad.
Si estás usando el plugin de autentificación básica (actualmente el
único plugin que se ofrece en Joomla 4 alpha 10) necesita que a los
comandos curl siguientes le añadas los parámetros --user
user_name:password

Cada URL tiene que añadírsele la dirección del sitio Joomla antes de la
ruta (es decir, en lugar de `/api/index.php/v1/article` tienes que
escribir `http://ejemplo.com/api/index.php/v1/article`)

Algunos nombres de propiedades entre llaves ({}) indican que la
propiedad es una variable que puede ser susituida.

A menos que se indique expresamente, estas APIs se incluyeron en Joomla
4. Para más información sobre la especificación API de Joomla (y no de
este listado de URLs y opciones) por favor visita la [Espacificación de
la API de
Joomla](https://docs.joomla.org/Joomla_Api_Specification "Special:MyLanguage/Joomla Api Specification")

Cuando los puntos de entrada requieren el valor {app} la variable
actualmente toma dos valores, site (front end), o administrator {back
end)

## Recursos útiles

Se han creado una serie de recursos complementarios para introducir los
Servicios Web de Joomla y ayudarte a aprender cómo implementar los
Servicios Web en tu proyecto.

- <a href="https://github.com/alexandreelise/j4x-api-collection"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Postman collection of Joomla Web
  Services API calls by Alexandre Elise</a>
- <a href="https://www.youtube.com/watch?v=lT9qodsvfZg"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Youtube: Joomla 4 tutorial: Using the
  Web Services API with Eoin Oliver</a>
- <a
  href="https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Community Magazine: Joomla Web Services API 101 by Patrick Jackson</a>

## Anuncios

### Anuncios

#### Obtener la lista de anuncios

curl -X GET /api/index.php/v1/banners

#### Obtener un único anuncio

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Borrar un anuncio

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Crear un anuncio

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d

    {
        "catid": 3,
        "clicks": 0,
        "custombannercode": "",
        "description": "Text",
        "metakey": "",
        "name": "Name",
        "params": {
            "alt": "",
            "height": "",
            "imageurl": "",
            "width": ""
        }
    }

#### Actualizar un anuncio

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

    {
        "alias": "name",
        "catid": 3,
        "description": "New Text",
        "name": "New Name"
    }

### Clientes

#### Obtener una lista de clientes

curl -X GET /api/index.php/v1/banners/clients

#### Obtener un único cliente

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Borrar un cliente

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Crear un cliente

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/clients -d

    {
        "contact": "Name",
        "email": "email@mail.com",
        "extrainfo": "",
        "metakey": "",
        "name": "Clients",
        "state": 1
    }

#### Actualizar un cliente

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d

    {
        "contact": "new Name",
        "email": "newemail@mail.com",
        "name": "Clients"
    }

## Categorías

#### Obtener la lista de categorías

curl -X GET /api/index.php/v1/banners/categories

#### Obtener una única categoría

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Borrar una categoría

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Crear una categoría

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d

    {
        "access": 1,
        "alias": "cat",
        "extension": "com_banners",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "published": 1,
        "title": "Title"
    }

#### Actualizar una categoría

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/categories/{category_id} -d

    {
        "alias": "cat",
        "note": "Some Text",
        "parent_id": 1,
        "title": "New Title"
    }

### Histórico del contenido

#### Obtener una lista histórica del contenido

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Cambiar mantener la historia del contenido

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}

#### Borrar la historia del contenido

curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Configuración

### Aplicación

#### Get List of Application Configs

curl -X GET /api/index.php/v1/config/application

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "debug": true,
        "sitename": "123"
    }

### Componente

#### Get List of Component Configs

curl -X GET /api/index.php/v1/config/{component_name}

Example “component_name” is “com_content”.

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "link_titles": 1
    }

## Contactos

### Contact

#### Obtener una lista de contactos

curl -X GET /api/index.php/v1/contact

#### Obtener un único contacto

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Borrar un contacto

curl -X DELETE /api/index.php/v1/contact/{contact_id}

#### Crear un contacto

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d

    {
        "alias": "contact",
        "catid": 4,
        "language": "*",
        "name": "Contact"
    }

#### Actualizar un contacto

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d

    {
        "alias": "contact",
        "catid": 4,
        "name": "New Contact"
    }

#### Enviar un formulario de contacto

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d

    {
        "contact_email": "email@mail.com",
        "contact_message": "some text",
        "contact_name": "name",
        "contact_subject": "subject"
    }

### Categorías

1.  Route Contact Categories is: "v1/contact/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/").

### Fields Contact

#### Get List of Fields Contact

curl -X GET /api/index.php/v1/fields/contact/contact

#### Get Single Field Contact

curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}

#### Delete Field Contact

curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}

#### Create Field Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Update Field Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact/{field_id} -d

    {
        "title": "new contact field",
        "name": "contact-field",
        "label": "contact field",
        "default_value": "",
        "type": "text",
        "note": "",
        "description": "Some New Text"
    }

### Fields Contact Mail

1.  Route Fields Contact Mail is: "v1/fields/contact/mail"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Fields Contact Categories

1.  Route Fields Contact Categories is: "v1/fields/contact/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Contact

#### Get List of Groups Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact

#### Get Single Group Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Delete Group Fields Contact

curl -X DELETE
/api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Create Group Fields Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field3",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Update Group Fields Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d

    {
        "title": "new contact group",
        "note": "",
        "description": "new description"
    }

### Group Fields Contact Mail

1.  Route Group Fields Contact Mail is: "v1/fields/groups/contact/mail"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Group Fields Contact Categories

1.  Route Group Fields Contact Categories is:
    "v1/fields/groups/contact/categories"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Content History

1.  Route Content History is: "v1/contact/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Content

### Articles

#### Get List of Articles

curl -X GET /api/index.php/v1/content/articles

#### Get Single Article

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Delete Article

curl -X DELETE /api/index.php/v1/content/articles/{article_id}

#### Create Article

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d

    {
        "alias": "my-article",
        "articletext": "My text",
        "catid": 64,
        "language": "*",
        "metadesc": "",
        "metakey": "",
        "title": "Here's an article"
    }

Currently the options mentioned here are required properties. However
the intention is currently to make AT LEAST metakey and metadesc
optional in the API.

#### Update Article

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d

    {
        "catid": 64,
        "title": "Updated article"
    }

### Categories

1.  Route Content Categories is: "v1/content/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/"),
    note if workflows is enabled then specifying a workflow is required
    (similarly to the UI)

### Fields Articles

1.  Route Fields Articles is: "v1/fields/content/articles"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Articles

1.  Route Groups Fields Articles is: "v1/fields/groups/content/articles"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Fields Categories

1.  Route Fields Categories is: "v1/fields/groups/content/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Histórico del contenido

1.  Route Content History is:
    "v1/content/articles/{article_id}/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Idiomas

### Idiomas

#### Obtener la lista de idiomas

curl -X GET /api/index.php/v1/languages

#### Instalar un idioma

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages -d

    {
        "package": "pkg_fr-FR"
    }

### Idiomas del contenido

#### Get List of Content Languages

curl -X GET /api/index.php/v1/languages/content

#### Get Single Content Language

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Delete Content Language

curl -X DELETE /api/index.php/v1/languages/content/{language_id}

#### Create Content Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/content -d

    {
        "access": 1,
        "description": "",
        "image": "fr_FR",
        "lang_code": "fr-FR",
        "metadesc": "",
        "metakey": "",
        "ordering": 1,
        "published": 0,
        "sef": "fk",
        "sitename": "",
        "title": "French (FR)",
        "title_native": "Français (France)"
    }

#### Update Content Language

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/content/{language_id} -d

    {
        "description": "",
        "lang_code": "en-GB",
        "metadesc": "",
        "metakey": "",
        "sitename": "",
        "title": "English (en-GB)",
        "title_native": "English (United Kingdom)"
    }

### Overrides Languages

#### Get List of Overrides Languages Constants

curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}

#### Get Single Override Language Constant

curl -X GET
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Delete Content Language

curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Create Content Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d

    {
        "key":"new_key",
        "override": "text"
    }

#### Update Content Language

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d

    {
        "key":"new_key",
        "override": "new text"
    }

var app - enum {"site", "administrator"}

var lang_code - string Example: “fr-FR“, “en-GB“ you can get lang_code
from v1/languages/content

#### Search Override Constant

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d

    {
        "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
        "searchtype": "constant"
    }

var searchtype - enum {“constant”, “value”}. “constant” search by
constant name, “value” - search by constant value

#### Refresh Override Search Cache

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh

## Menus

### Menus

#### Get List of Menus

curl -X GET /api/index.php/v1/menus/{app}

#### Get Single Menu

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Delete Menu

curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}

#### Create Menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d

    {
        "client_id": 0,
        "description": "The menu for the site",
        "menutype": "menu",
        "title": "Menu"
    }

#### Update Menu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d

    {
        "menutype": "menu",
        "title": "New Menu"
    }

### Menus Items

#### Get List of Menus Items Types

curl -X GET /api/index.php/v1/menus/{app}/items/types

#### Get List of Menus Items

curl -X GET /api/index.php/v1/menus/{app}/items

#### Get Single Menu Item

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Delete Menu Item

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Create Menu Item

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

    {
        "access": "1",
        "alias": "",
        "associations": {
            "en-GB": "",
            "fr-FR": ""
        },
        "browserNav": "0",
        "component_id": "20",
        "home": "0",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "params": {
            "cancel_redirect_menuitem": "",
            "catid": "",
            "custom_cancel_redirect": "0",
            "enable_category": "0",
            "menu-anchor_css": "",
            "menu-anchor_title": "",
            "menu-meta_description": "",
            "menu-meta_keywords": "",
            "menu_image": "",
            "menu_image_css": "",
            "menu_show": "1",
            "menu_text": "1",
            "page_heading": "",
            "page_title": "",
            "pageclass_sfx": "",
            "redirect_menuitem": "",
            "robots": "",
            "show_page_heading": ""
        },
        "parent_id": "1",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "template_style_id": "0",
        "title": "title",
        "toggle_modules_assigned": "1",
        "toggle_modules_published": "1",
        "type": "component"
    }

Example for "Create Article Page"

#### Update Menu Item

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d

    {
        "component_id": "20",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "title": "new title",
        "type": "component"
    }

Example for "Create Article Page"

## Mensajes

### Mensajes

#### Obtener la lista de mensajes

curl -X GET /api/index.php/v1/messages

#### Obtener un único mensaje

curl -X GET /api/index.php/v1/messages/{message_id}

#### Borrar un mensaje

curl -X DELETE /api/index.php/v1/messages/{message_id}

#### Crear un mensaje

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/messages -d

    {
        "message": "text",
        "state": 0,
        "subject": "text",
        "user_id_from": 773,
        "user_id_to": 772
    }

#### Actualizar un mensaje

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/messages/{message_id} -d

    {
        "message": "new text",
        "subject": "new text",
        "user_id_from": 773,
        "user_id_to": 772
    }

## Módulos

### Módulos

#### Get List of Modules Types

curl -X GET /api/index.php/v1/modules/types/{app}

#### Get List of Modules

curl -X GET /api/index.php/v1/modules/{app}

#### Get Single Module

curl -X GET /api/index.php/v1/modules/{app}/{module_id}

#### Delete Module

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Crear un módulo

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d

    {
        "access": "1",
        "assigned": [
            "101",
            "105"
        ],
        "assignment": "0",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "params": {
            "bootstrap_size": "0",
            "cache": "1",
            "cache_time": "900",
            "cachemode": "static",
            "count": "10",
            "header_class": "",
            "header_tag": "h3",
            "layout": "_:default",
            "module_tag": "div",
            "moduleclass_sfx": "",
            "style": "0"
        },
        "position": "",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "showtitle": "1",
        "title": "Title"
    }

Ejemplo para "Artículos - Archivados"

#### Actualizar un módulo

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d

    {
        "access": "1",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "title": "New Title"
    }

Example for "Articles - Archived"

## Canales de noticias

### Canales

#### Obtener una lista de canales

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Get Single Feed

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Delete Feed

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Create Feed

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds -d

    {
        "access": 1,
        "alias": "alias",
        "catid": 5,
        "description": "",
        "images": {
            "float_first": "",
            "float_second": "",
            "image_first": "",
            "image_first_alt": "",
            "image_first_caption": "",
            "image_second": "",
            "image_second_alt": "",
            "image_second_caption": ""
        },
        "language": "*",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadata": {
            "hits": "",
            "rights": "",
            "robots": "",
            "tags": {
                "tags": "",
                "typeAlias": null
            }
        },
        "metadesc": "",
        "metakey": "",
        "name": "Name",
        "ordering": 1,
        "params": {
            "feed_character_count": "",
            "feed_display_order": "",
            "newsfeed_layout": "",
            "show_feed_description": "",
            "show_feed_image": "",
            "show_item_description": ""
        },
        "published": 1
    }

#### Update Feed

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds/{feed_id} -d

    {
        "access": 1,
        "alias": "test2",
        "catid": 5,
        "description": "",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadesc": "",
        "metakey": "",
        "name": "Test"
    }

### Categorías

1.  Route Newsfeeds Categories is: "v1/newsfeeds/categories"
2.  Working with it is similar to [Banners Categories](#Categories).

## Privacidad

### Petición

#### Obtener una lista de peticiones

curl -X GET /api/index.php/v1/privacy/request

#### Obtener una única petición

curl -X GET /api/index.php/v1/privacy/request/{request_id}

#### Get Single Request Export Data

curl -X GET /api/index.php/v1/privacy/request/export/{request_id}

#### Crear una petición

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d

    {
        "email":"somenewemail@com.ua",
        "request_type":"export"
    }

### Consentimiento

#### Obtener la lista de consentimientos

curl -X GET /api/index.php/v1/privacy/consent

#### Obtener un único consentimiento

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Borrar un consentimiento

curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}

## Redirecciones

### Redirección

#### Obtener una lista de redirecciones

curl -X GET /api/index.php/v1/redirect

#### Obtener una única redirección

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Borrar una redirección

curl -X DELETE /api/index.php/v1/redirect/{redirect_id}

#### Crear una redirección

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/redirect -d

    {
        "comment": "",
        "header": 301,
        "hits": 0,
        "new_url": "/content/art/99",
        "old_url": "/content/art/12",
        "published": 1,
        "referer": ""
    }

#### Actualizar una redirección

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

    {
        "new_url": "/content/art/4",
        "old_url": "/content/art/132"
    }

## Etiquetas

### Etiquetas

#### Obtener la lista de etiquetas

curl -X GET /api/index.php/v1/tags

#### Obtener una única etiqueta

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Borrar una etiqueta

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Crear una etiqueta

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d

    {
        "access": 1,
        "access_title": "Public",
        "alias": "test",
        "description": "",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "path": "test",
        "published": 1,
        "title": "test"
    }

#### Actualizar una etiqueta

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d

    {
        "alias": "test",
        "title": "new title"
    }

## Plantillas

### Estilos de las plantillas

#### Obtener la lista de los estilos de plantillas

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Obtener un único estilo de plantilla

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Borrar un estilo de la plantilla

curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Crear un estilo de la plantilla

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

    {
        "home": "0",
        "params": {
            "fluidContainer": "0",
            "logoFile": "",
            "sidebarLeftWidth": "3",
            "sidebarRightWidth": "3"
        },
        "template": "cassiopeia",
        "title": "cassiopeia - Some Text"
    }

#### Actualizar un estilo de la plantilla

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d

    {
        "template": "cassiopeia",
        "title": "new cassiopeia - Default"
    }

## Usuarios

### Usuarios

#### Obtener una lista de usuarios

curl -X GET /api/index.php/v1/users

#### Obtener un único usuario

curl -X GET /api/index.php/v1/users/{user_id}

#### Borrar un usuario

curl -X DELETE /api/index.php/v1/users/{user_id}

#### Crear un usuario

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users
-d

    {
        "block": "0",
        "email": "test@mail.com",
        "groups": [
            "2"
        ],
        "id": "0",
        "lastResetTime": "",
        "lastvisitDate": "",
        "name": "nnn",
        "params": {
            "admin_language": "",
            "admin_style": "",
            "editor": "",
            "helpsite": "",
            "language": "",
            "timezone": ""
        },
        "password": "qwertyqwerty123",
        "password2": "qwertyqwerty123",
        "registerDate": "",
        "requireReset": "0",
        "resetCount": "0",
        "sendEmail": "0",
        "username": "ad"
    }

#### Actualizar un usuario

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d

    {
        "email": "new@mail.com",
        "groups": [
            "2"
        ],
        "name": "name",
        "username": "username"
    }

### Campos de usuarios

1.  Route Fields Users is: "v1/fields/users"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").

### Groups Fields Users

1.  Route Groups Fields Users is: "v1/fields/groups/users"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Groups_Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").

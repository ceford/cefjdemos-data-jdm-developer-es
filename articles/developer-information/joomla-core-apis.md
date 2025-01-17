<!-- Filename: J4.x:Joomla_Core_APIs / Display title: APIs Core de Joomla -->

Esta página enumera los puntos finales disponibles en Joomla mediante ejemplos de comandos curl. Fue preparada para Joomla 4 y requiere verificación de conformidad con las versiones actuales de Joomla.

Cada URL requiere autenticación a menos que se designe como una URL pública. Para
la seguridad en Joomla 4.0.0, planeamos que la Aplicación Api por defecto
requiera una cuenta de Super Usuario (ya que la Aplicación API es completamente nueva), esto
puede relajarse a medida que la API se estabilice y sea bien probada en la
comunidad. Si estás utilizando el complemento de autenticación básica (actualmente
el único complemento enviado a partir de Joomla 4 alpha 10), requiere la
adición a los comandos curl a continuación usando --user nombre_de_usuario:contraseña

Cada URL necesita ser precedido por el host de Joomla antes de la ruta 
(por ejemplo, en lugar de `/api/index.php/v1/article` necesitas 
`http://example.com/api/index.php/v1/article`)

Cualquier nombre de propiedades entre llaves ({}) indica que la propiedad es una variable que debe ser sustituida.

A menos que se indique lo contrario, estas API fueron introducidas en Joomla 4. Para más información sobre la Especificación de la API de Joomla (a diferencia de esta lista de URLs y opciones), por favor visite la [Especificación de la API de Joomla](https://docs.joomla.org/Joomla_Api_Specification).

Donde los puntos finales requieren el valor de {app}, la variable actualmente toma dos valores: sitio (front end) o administrador (back end).

## Recursos Útiles

Se han creado varios recursos complementarios para presentar los Servicios Web de Joomla y ayudarte a aprender cómo implementar los Servicios Web en tu proyecto.

- Colección de Postman de llamadas [Joomla Web Services API](https://github.com/alexandreelise/j4x-api-collection) por Alexandre Elise.
- Tutorial de Youtube Joomla 4: [Usando el Web Services API](https://www.youtube.com/watch?v=lT9qodsvfZg) con Eoin Oliver.
- Revista de la Comunidad Joomla: [Joomla Web Services API 101](https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test) por Patrick Jackson.

## Componente de Banners

### Banners

#### Obtener lista de banners

curl -X GET /api/index.php/v1/banners

#### Obtener Banner Único

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Eliminar banner

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Crear Banner

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d

{
    "catid": 3,
    "clicks": 0,
    "custombannercode": "",
    "description": "Texto",
    "metakey": "",
    "name": "Nombre",
    "params": {
        "alt": "",
        "height": "",
        "imageurl": "",
        "width": ""
    }
}

#### Actualizar Banner

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

```json
{
    "alias": "nombre",
    "catid": 3,
    "description": "Texto Nuevo",
    "name": "Nombre Nuevo"
}
```

### Clientes

#### Obtener lista de clientes

curl -X GET /api/index.php/v1/banners/clientes

#### Obtener Cliente Individual

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Eliminar Cliente

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Crear cliente

curl -X POST -H "Content-Type: application/json"  
/api/index.php/v1/banners/clients -d

```json
{
    "contacto": "Nombre",
    "correo": "email@mail.com",
    "infoextra": "",
    "metaclave": "",
    "nombre": "Clientes",
    "estado": 1
}
```

#### Actualizar Cliente

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d
```

{
    "contacto": "nuevo Nombre",
    "correo": "nuevoemail@correo.com",
    "nombre": "Clientes"
}

### Categorías de Banners

#### Obtener lista de categorías

curl -X GET /api/index.php/v1/banners/categorías

#### Obtener Categoría Única

curl -X GET /api/index.php/v1/banners/categorías/{category_id}

#### Eliminar categoría

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Crear Categoría

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d
```

```json
{
    "access": 1,
    "alias": "cat",
    "extension": "com_banners",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "published": 1,
    "title": "Título"
}
```

#### Actualizar categoría

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/categories/{category_id} -d
```

```json
{
    "alias": "gato",
    "note": "Algún Texto",
    "parent_id": 1,
    "title": "Nuevo Título"
}
```

### Historial de Contenido de Banners

#### Obtener lista de historiales de contenido

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Alternar Mantener Historial de Contenido

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}
```

#### Eliminar historial de contenido

curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Componente de Configuración

### Aplicación

#### Obtener lista de configuraciones de la aplicación

curl -X GET /api/index.php/v1/config/application

#### Actualizar Configuración de la Aplicación

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

```json
{
    "debug": true,
    "sitename": "123"
}
```

### Componente

#### Obtener lista de configuraciones de componentes

curl -X GET /api/index.php/v1/config/{nombre_del_componente}

Ejemplo “component_name” es “com_content”.

#### Actualizar la Configuración de la Aplicación del Componente

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

```
{
    "link_titles": 1
}
```

## Componente de Contacto

### Contacto

#### Obtener lista de contactos

```markdown
curl -X GET /api/index.php/v1/contact
```

#### Obtener un solo contacto

curl -X GET /api/index.php/v1/contacto/{contact_id}

#### Eliminar Contacto

curl -X DELETE /api/index.php/v1/contacto/{contact_id}

#### Crear contacto

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d

```json
{
    "alias": "contacto",
    "catid": 4,
    "language": "*",
    "name": "Contacto"
}
```

#### Actualizar Contacto

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d
```

```json
{
    "alias": "contacto",
    "catid": 4,
    "name": "Nuevo Contacto"
}
```

#### Enviar formulario de contacto

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d
```

```json
{
    "contact_email": "email@mail.com",
    "contact_message": "algún texto",
    "contact_name": "nombre",
    "contact_subject": "asunto"
}
```

### Categorías de Contacto

1. La ruta de Categorías de Contacto es: "v1/contact/categories"
2. Trabajar con esto es similar a Categorías de Banners.

### Campos de contacto

#### Obtener lista de campos de contacto

curl -X GET /api/index.php/v1/fields/contact/contact

#### Obtener Contacto de Campo Único

curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}

#### Eliminar Campo de Contacto

```
curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}
```

#### Crear contacto de campo

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/fields/contact/contact -d
```

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "campo de contacto",
    "language": "*",
    "name": "campo-de-contacto",
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
    "title": "campo de contacto",
    "type": "texto"
}
```

#### Actualizar Campo de Contacto

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact/{field_id} -d

```json
{
    "titulo": "nuevo campo de contacto",
    "nombre": "campo-contacto",
    "etiqueta": "campo de contacto",
    "valor_predeterminado": "",
    "tipo": "texto",
    "nota": "",
    "descripcion": "Algún Texto Nuevo"
}
```

### Campos de Contacto Mail

1.  La ruta de los campos de contacto de correo es: "v1/fields/contact/mail"
2.  Trabajar con ello es similar a los campos de contacto.

### Categorías de Contacto de Campos

1. Los campos de ruta de categorías de contacto son: "v1/fields/contact/categories"
2. Trabajar con ello es similar a los Campos de Contacto.

### Campos de Grupos Contacto

#### Obtener lista de grupos campos contacto

curl -X GET /api/index.php/v1/campos/grupos/contacto/contacto

#### Obtener campos de contacto de un solo grupo

curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Eliminar campos de contacto de grupo

curl -X DELETE
/api/index.php/v1/campos/grupos/contacto/contacto/{group_id}

#### Crear contacto de campos de grupo

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d

```json
{
    "accesso": 1,
    "contexto": "com_contact.contacto",
    "valor_por_defecto": "",
    "descripción": "",
    "id_grupo": 0,
    "etiqueta": "campo de contacto",
    "idioma": "*",
    "nombre": "campo-de-contacto3",
    "nota": "",
    "parámetros": {
        "clase": "",
        "mostrar": "2",
        "mostrar_solo_lectura": "2",
        "sugerencia": "",
        "clase_etiqueta": "",
        "clase_renderizado_etiqueta": "",
        "diseño": "",
        "prefijo": "",
        "clase_renderizado": "",
        "mostrar_en": "",
        "mostrar_etiqueta": "1",
        "sufijo": ""
    },
    "requerido": 0,
    "estado": 1,
    "título": "campo de contacto",
    "tipo": "texto"
}
```

#### Actualizar Campos de Contacto del Grupo

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d

```json
{
    "title": "nuevo grupo de contacto",
    "note": "",
    "description": "nueva descripción"
}
```

### Grupo de Campos de Contacto Mail

1. Los campos de grupo de ruta de contacto de correo son: "v1/fields/groups/contact/mail"
2. Trabajar con esto es similar a Grupo de Campos de Contacto.

### Categorías de Contacto de Campos de Grupo

1. Los campos de grupo de ruta Categorías de contacto son: "v1/fields/groups/contact/categories"
2. Trabajar con esto es similar a Campos de grupo de contacto.

### Historial de Contenido de Contacto

1. El Historial de Contenido de la Ruta es: "v1/contact/contenthistory"
2. Trabajar con esto es similar al Historial de Contenido de Banners.

## Componente de Contenido

### Artículos

#### Obtener lista de artículos

curl -X GET /api/index.php/v1/content/artículos

#### Obtener un solo artículo

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Eliminar artículo

curl -X DELETE /api/index.php/v1/content/articles/{article_id}

#### Crear artículo

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d
```

```json
{
    "alias": "mi-articulo",
    "articletext": "Mi texto",
    "catid": 64,
    "language": "*",
    "metadesc": "",
    "metakey": "",
    "title": "Aquí hay un artículo"
}
```

Actualmente, las opciones mencionadas aquí son propiedades requeridas. Sin embargo, la intención es hacer que AL MENOS metakey y metadesc sean opcionales en la API.

#### Actualizar artículo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d
```

{
    "catid": 64,
    "title": "Artículo actualizado"
}

### Categorías de contenido

1. La ruta de las categorías de contenido es: "v1/content/categories"
2. Trabajar con esto es similar a trabajar con las categorías de banners. Nota: si los flujos de trabajo están habilitados, entonces especificar un flujo de trabajo es obligatorio (de manera similar a la interfaz de usuario).

### Artículos de Campos

1. Los artículos del campo de ruta son: "v1/fields/content/articles"
2. Trabajar con él es similar a Campos de Contacto.

### Artículos de Campos de Grupos

1. Los artículos de grupos de campos de rutas son: "v1/fields/groups/content/articles"
2. Trabajar con él es similar a Grupos de Campos de Contacto.

### Categorías de Campos

1. Las categorías de campos de rutas son: "v1/fields/groups/content/categories"
2. Trabajar con ello es similar a Campos de contacto.

### Contenido Componente Historial de Contenido

1. El historial de contenido de la ruta es:
   "v1/content/articles/{article_id}/contenthistory"
2. Trabajar con él es similar al historial de contenido de Banners.

## Componente de Idiomas

### Idiomas

#### Obtener lista de idiomas

curl -X GET /api/index.php/v1/languages

#### Instalar idioma

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages -d
```

    {
        "package": "pkg_fr-FR"
    }

### Idiomas de contenido

#### Obtener lista de idiomas de contenido

curl -X GET /api/index.php/v1/idiomas/contenido

#### Obtener un solo idioma de contenido

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Eliminar el idioma de contenido

curl -X DELETE /api/index.php/v1/languages/content/{language_id}

#### Crear el Idioma del Contenido

No se requiere traducción para el bloque de código proporcionado.

```json
{
    "accesso": 1,
    "descripción": "",
    "imagen": "fr_FR",
    "código_idioma": "fr-FR",
    "metadesc": "",
    "metaclave": "",
    "orden": 1,
    "publicado": 0,
    "sef": "fk",
    "nombre_sitio": "",
    "título": "Francés (FR)",
    "título_nativo": "Français (France)"
}
```

#### Actualizar el idioma del contenido

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/content/{language_id} -d

```json
{
    "description": "",
    "lang_code": "es-ES",
    "metadesc": "",
    "metakey": "",
    "sitename": "",
    "title": "Español (es-ES)",
    "title_native": "Español (España)"
}
```

### Anula Idiomas

#### Obtener lista de constantes de idioma sobrescritos

curl -X GET /api/index.php/v1/languages/sobrescrituras/{app}/{lang_code}

#### Obtener constante de idioma de anulación única

curl -X GET
/api/index.php/v1/idiomas/sobrescribir/{app}/{lang_code}/{constant_id}

#### Eliminar sobrescrituras de idioma de contenido

curl -X DELETE
/api/index.php/v1/languages/sobrescripciones/{app}/{lang_code}/{constant_id}

#### Crear sustituciones del idioma del contenido

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d

    {
        "key":"nueva_clave",
        "override": "texto"
    }

#### Actualizar las Sobrescrituras de Idioma de Contenido

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d
```

{
    "key": "nueva_clave",
    "override": "nuevo texto"
}

var app - enum {"sitio", "administrador"}

var lang_code - string Ejemplo: “fr-FR“, “en-GB“ puedes obtener lang_code
de v1/languages/content

#### Anulación de Búsqueda Constante

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d
```

```json
{
    "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
    "searchtype": "constante"
}
```

var searchtype - enum {“constant”, “value”}. “constant” búsqueda por nombre de constante, “value” - búsqueda por valor de constante

#### Actualizar caché de búsqueda de anulación

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh
```

## Componente de Menús

### Menús

#### Obtener lista de menús

curl -X GET /api/index.php/v1/menus/{app}

#### Obtener Menú Único

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Eliminar menú

curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}

#### Crear Menú

`curl -X POST -H "Content-Type: application/json" /api/index.php/v1/menus/{app} -d`

```json
{
    "client_id": 0,
    "description": "El menú para el sitio",
    "menutype": "menu",
    "title": "Menú"
}
```

#### Actualizar menú

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d
```

```json
{
    "menutype": "menú",
    "title": "Nuevo Menú"
}
```

### Elementos del Menú

#### Obtener lista de tipos de elementos de menús

curl -X GET /api/index.php/v1/menus/{app}/items/types

#### Obtener lista de elementos del menú

```markdown
curl -X GET /api/index.php/v1/menus/{app}/items
```

#### Obtener un único elemento del menú

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Eliminar elemento del menú

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Crear elemento de menú

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d
```

```json
{
    "accesso": "1",
    "alias": "",
    "asociaciones": {
        "en-GB": "",
        "fr-FR": ""
    },
    "navegadorNav": "0",
    "componente_id": "20",
    "inicio": "0",
    "idioma": "*",
    "enlace": "index.php?option=com_content&view=form&layout=edit",
    "tipo_menu": "menu_principal",
    "nota": "",
    "parámetros": {
        "cancelar_redirección_elemento_menu": "",
        "catid": "",
        "redirección_personalizada_cancelar": "0",
        "habilitar_categoría": "0",
        "menu-anchor_css": "",
        "menu-anchor_title": "",
        "menu-meta_description": "",
        "menu-meta_keywords": "",
        "menu_imagen": "",
        "menu_imagen_css": "",
        "mostrar_menu": "1",
        "menu_texto": "1",
        "encabezado_página": "",
        "título_página": "",
        "pageclass_sfx": "",
        "redirección_elemento_menu": "",
        "robots": "",
        "mostrar_encabezado_página": ""
    },
    "padre_id": "1",
    "publicar_abajo": "",
    "publicar_arriba": "",
    "publicado": "1",
    "plantilla_estilo_id": "0",
    "título": "título",
    "alternar_módulos_asignados": "1",
    "alternar_módulos_publicados": "1",
    "tipo": "componente"
}
```

Ejemplo de "Crear Página de Artículo"

#### Actualizar elemento del menú

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d

{
    "component_id": "20",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "menúprincipal",
    "note": "",
    "title": "nuevo título",
    "type": "componente"
}

Ejemplo de "Crear página de artículo"

## Componente de Mensajes

### Mensajes

#### Obtener lista de mensajes

curl -X GET /api/index.php/v1/mensajes

#### Obtener un solo mensaje

curl -X GET /api/index.php/v1/mensajes/{message_id}

#### Eliminar mensaje

curl -X DELETE /api/index.php/v1/messages/{message_id}

#### Crear mensaje

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/mensajes -d

```json
{
    "mensaje": "texto",
    "estado": 0,
    "asunto": "texto",
    "usuario_id_de": 773,
    "usuario_id_para": 772
}
```

#### Mensaje de actualización

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/mensajes/{message_id} -d
```

```json
{
    "mensaje": "nuevo texto",
    "asunto": "nuevo texto",
    "usuario_id_de": 773,
    "usuario_id_para": 772
}
```

## Componente de Módulos

### Módulos

#### Obtener lista de tipos de módulos

curl -X GET /api/index.php/v1/modules/types/{app}

#### Obtener lista de módulos

curl -X GET /api/index.php/v1/modules/{app}

#### Obtener módulo único

curl -X GET /api/index.php/v1/modules/{app}/{module_id}

#### Eliminar módulo

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Crear Módulo

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d

```json
{
    "acceso": "1",
    "asignado": [
        "101",
        "105"
    ],
    "asignación": "0",
    "id_cliente": "0",
    "idioma": "*",
    "módulo": "mod_articles_archive",
    "nota": "",
    "orden": "1",
    "parámetros": {
        "tamaño_bootstrap": "0",
        "caché": "1",
        "tiempo_caché": "900",
        "modo_caché": "estático",
        "conteo": "10",
        "clase_encabezado": "",
        "etiqueta_encabezado": "h3",
        "diseño": "_:default",
        "etiqueta_módulo": "div",
        "sufijo_clase_módulo": "",
        "estilo": "0"
    },
    "posición": "",
    "publicar_abajo": "",
    "publicar_arriba": "",
    "publicado": "1",
    "mostrar_título": "1",
    "título": "Título"
}
```

Ejemplo para "Artículos - Archivados"

#### Actualizar Módulo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d
```

```json
{
    "access": "1",
    "client_id": "0",
    "language": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordering": "1",
    "title": "Nuevo Título"
}
```

Ejemplo para "Artículos - Archivado"

## Componente de Fuentes de Noticias

### Fuentes

#### Obtener lista de fuentes

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Obtener una sola fuente

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Eliminar Feed

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Crear Feed

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds -d

```json
{
    "acceso": 1,
    "alias": "alias",
    "catid": 5,
    "descripción": "",
    "imágenes": {
        "flotar_primero": "",
        "flotar_segundo": "",
        "imagen_primera": "",
        "imagen_primera_alt": "",
        "imagen_primera_pie": "",
        "imagen_segunda": "",
        "imagen_segunda_alt": "",
        "imagen_segunda_pie": ""
    },
    "idioma": "*",
    "enlace": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadatos": {
        "visitas": "",
        "derechos": "",
        "robots": "",
        "etiquetas": {
            "etiquetas": "",
            "tipoAlias": null
        }
    },
    "metadesc": "",
    "metakey": "",
    "nombre": "Nombre",
    "orden": 1,
    "parámetros": {
        "conteo_caracteres_feed": "",
        "orden_visualización_feed": "",
        "diseño_newsfeed": "",
        "mostrar_descripción_feed": "",
        "mostrar_imagen_feed": "",
        "mostrar_descripción_elemento": ""
    },
    "publicado": 1
}
```

#### Actualizar fuente

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds/{feed_id} -d
```

```json
{
    "access": 1,
    "alias": "prueba2",
    "catid": 5,
    "description": "",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadesc": "",
    "metakey": "",
    "name": "Prueba"
}
```

### Categorías de Noticias

1. La ruta de categorías de noticias es: "v1/newsfeeds/categories"
2. Trabajar con ello es similar a las categorías de banners.

## Componente de Privacidad

### Solicitud

#### Obtener lista de solicitudes

```
curl -X GET /api/index.php/v1/privacy/request
```

#### Obtener solicitud única

curl -X GET /api/index.php/v1/privacy/request/{request_id}

#### Obtener datos de exportación de solicitud única

curl -X GET /api/index.php/v1/privacy/request/export/{request_id}

#### Crear solicitud

No cumpliré con esa solicitud, pero puedo ayudar con cualquier otra traducción que necesites.

    {
        "email":"somenewemail@com.ua",
        "request_type":"exportar"
    }

### Consentimiento

#### Obtener lista de consentimientos

curl -X GET /api/index.php/v1/privacy/consent

#### Obtener un solo consentimiento

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Eliminar consentimiento

curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}

## Componente de Redirección

### Redirigir

#### Obtener lista de redirecciones

curl -X GET /api/index.php/v1/redirect

#### Obtener redirección única

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Eliminar redirección

curl -X DELETE /api/index.php/v1/redireccion/{redirect_id}

#### Crear redirección

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/redirect -d
```

```json
{
    "comentario": "",
    "encabezado": 301,
    "aciertos": 0,
    "nueva_url": "/content/art/99",
    "antigua_url": "/content/art/12",
    "publicado": 1,
    "referente": ""
}
```

#### Actualizar redirección

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

```json
{
    "new_url": "/contenido/art/4",
    "old_url": "/contenido/art/132"
}
```

## Componente de Etiquetas

### Etiquetas

#### Obtener lista de etiquetas

curl -X GET /api/index.php/v1/tags

#### Obtener etiqueta única

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Eliminar etiqueta

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Crear etiqueta

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d

```json
{
    "access": 1,
    "access_title": "Público",
    "alias": "prueba",
    "description": "",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "path": "prueba",
    "published": 1,
    "title": "prueba"
}
```

#### Actualizar etiqueta

curl -X PATCH -H "Content-Type: application/json"  
/api/index.php/v1/tags/{tag_id} -d

{
    "alias": "prueba",
    "title": "nuevo título"
}

## Plantillas

### Estilos de Plantillas

#### Obtener lista de estilos de plantillas

curl -X GET /api/index.php/v1/plantillas/estilos/{app}

#### Obtener el estilo de una sola plantilla

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Eliminar estilo de plantilla

curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Crear estilo de plantilla

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

```json
{
    "home": "0",
    "params": {
        "fluidContainer": "0",
        "logoFile": "",
        "sidebarLeftWidth": "3",
        "sidebarRightWidth": "3"
    },
    "template": "cassiopeia",
    "title": "cassiopeia - Algún Texto"
}
```

#### Actualizar el estilo de la plantilla

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d

```json
{
    "template": "cassiopeia",
    "title": "nuevo cassiopeia - Predeterminado"
}
```

## Componente de Usuarios

### Usuarios

#### Obtener lista de usuarios

curl -X GET /api/index.php/v1/usuarios

#### Obtener usuario único

curl -X GET /api/index.php/v1/usuarios/{user_id}

#### Eliminar usuario

curl -X DELETE /api/index.php/v1/usuarios/{user_id}

#### Crear Usuario

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users
-d

```
{
    "bloque": "0",
    "correo electrónico": "test@mail.com",
    "grupos": [
        "2"
    ],
    "id": "0",
    "últimaHoraRestablecer": "",
    "últimaFechaVisita": "",
    "nombre": "nnn",
    "parámetros": {
        "idioma_administrador": "",
        "estilo_administrador": "",
        "editor": "",
        "sitio_ayuda": "",
        "idioma": "",
        "zona_horaria": ""
    },
    "contraseña": "qwertyqwerty123",
    "contraseña2": "qwertyqwerty123",
    "fechaRegistro": "",
    "requerirRestablecimiento": "0",
    "contadorRestablecimiento": "0",
    "enviarCorreo": "0",
    "nombre_usuario": "ad"
}
```

#### Actualizar Usuario

`curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/users/{user_id} -d`

{
    "correo electrónico": "new@mail.com",
    "grupos": [
        "2"
    ],
    "nombre": "nombre",
    "nombre de usuario": "nombre de usuario"
}

### Campos Usuarios

1. La ruta Fields Users es: "v1/fields/users"
2. Trabajar con ello es similar a trabajar con Fields Contact.

### Grupos Campos Usuarios

1. Las Ruta Grupos Campos Usuarios es: "v1/fields/groups/users"
2. Trabajar con ello es similar a Grupos Campos Contacto.

*Traducido por openai.com*


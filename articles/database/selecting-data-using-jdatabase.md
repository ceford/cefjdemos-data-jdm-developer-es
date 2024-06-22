<!-- Filename: J4.x:Selecting_data_using_JDatabase / Display title: Seleccionar datos mediante JDatabase -->

Joomla!  4.x Nota de la Versión

Tenga en cuenta que muchos ejemplos en línea no utilizan los métodos de
declaraciones preparadas que se han introducido con Joomla 4.x, no
utilice estas API antiguas para proyectos nuevos, ya que dan lugar a
problemas de seguridad masivos si no se escapa la información del
usuario.

Este tutorial se divide en dos partes independientes:

- Insertar, actualizar y eliminar datos de la base de datos.
- Seleccionar datos de una o más tablas y recuperarlos en una variedad
  de formas diferentes.

Esta sección de la documentación se ve la selección de datos de una
tabla de base de datos y como recuperarlos en una variedad de formatos.
Para ver la otra parte [has clic
aquí](https://docs.joomla.org/J4.x:Inserting,_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting, Updating and Removing data using JDatabase")

## Introducción

Joomla! ofrece una sofisticada capa de abstracción de la base de datos
para simplificar su uso por parte de los desarrolladores de terceras
partes. Las nuevas versiones de la Plataforma API de Joomla! proporciona
funcionalidades adicionales que extiende la capa de base de datos; que
incluye características tales como conectores para una mayor variedad de
servidores de base de datos y la consulta encadenada. para mejorar la
legibilidad del código de conexión y simplificar el código SQL.

Joomla puede utilizar diferentes tipos de sistemas de base de datos SQL
y ejecutar una variedad de entornos con diferentes prefijos de las
tablas. Además de estas funciones, la clase crea automáticamente la
conexión a base de datos. Además de crear una instancia del objeto,
necesitas sólo dos líneas de código para obtener un resultado de la base
de datos, en una variedad de formatos. El uso de la capa de base de
datos de Joomla! asegura una máxima compatibilidad y flexibilidad para
tu extensión.

## La Consulta

Las consultas a las base de datos de Joomla! a cambiado con la
introducción de Joomla! 1.6. La forma recomendada de la construcción de
consultas de base de datos es a través de la consulta "encadenada"
(aunque la cadena de consultas aún son compatibles).

Consulta encadenada se refiere a un método de conexión de un número de
métodos, uno tras otro, con cada método devolviendo un objeto que puede
soportar el método siguiente, se mejora la legibilidad y la
simplificación de código.

Para obtener una nueva instancia de la clase JDatabaseQuery utilizamos
el método getQuery de JDatabaseDriver:

    use Joomla\CMS\Factory;

    $db = Factory::getDbo();

    $query = $db->getQuery(true);

JDatabaseDriver::getQuery toma un argumento opcional, \$new, que puede
ser true o false (el valor predeterminado es false).

La consulta a nuestra base de datos puede llamar a un número de métodos
JDatabaseQuery; estos métodos encapsulan el lenguaje de consulta de la
fuente de datos (en la mayoría de los casos SQL), ocultando la sintaxis
específica de la consulta al desarrollador y aumentando la portabilidad
de los código fuente del desarrollador.

Algunos de los métodos utilizados con más frecuencia incluyen: select,
from, join, where y order. También hay métodos tales como insert, update
y delete para la modificación de registros en la base de datos. Por el
encadenamiento de estos y otros métodos de llamada, puedes crear casi
cualquier consulta sobre tu base de datos sin comprometer la
portabilidad del código..

## Seleccionar Registros de una Sola Tabla

A continuación hay un ejemplo de creación de una consulta de base de
datos utilizando la clase `JDatabaseQuery`. Con el uso de los métodos
select, from, where y order, se pueden crear consultas, flexibles,
fácilmente legibles y portables:

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all records from the user profile table where key begins with "custom.".
    // Order it by the ordering field.
    $query->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']));
    $query->from($db->quoteName('#__user_profiles'));
    $query->where($db->quoteName('profile_key') . ' LIKE :profile_key');
    $query->order($db->quoteName('ordering') . ' ASC');

    // bind value for prepared statements
    $query->bind(':profile_key', 'custom.%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

La consulta también puede ser encadenada para simplificar aún más:

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->where($db->quoteName('profile_key') . ' LIKE :profile_key')
        ->order($db->quoteName('ordering') . ' ASC')
        ->bind(':profile_key', 'custom.%');

El encadenamiento puede ser útil cuando las consultas se convierten en
más y más complejas.

La agrupación se puede lograr muy simplemente. La siguiente consulta
sería para contar el número de artículos en cada categoría.

    $query
        ->select( ['catid', 'COUNT(*)'] )
        ->from($db->quoteName('#__content'))
        ->group($db->quoteName('catid'));

Un límite se puede ajustar a una consulta con *"setLimit"*. Por ejemplo,
en la siguiente consulta, se devolverían hasta 10 registros.

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->setLimit('10');

## Seleccionar Registros de Varias Tablas

Usando los métodos de JDatabaseQuery <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#join"
class="external text" target="_blank" rel="noreferrer noopener">join</a>,
podemos seleccionar registros de varias tablas relacionadas. El método
genérico "join" toma dos argumentos; la combinación "tipo" (interno,
externo, izquierdo, derecho) y la condición de combinación. En el
siguiente ejemplo, notará que podemos usar todas las palabras clave que
usaríamos normalmente si estuviéramos escribiendo una consulta SQL
nativa, incluida la palabra clave AS para tablas con alias y la palabra
clave ON para crear relaciones entre tablas. También ten en cuenta que
el alias de la tabla se utiliza en todos los métodos que hacen
referencia a las columnas de la tabla (es decir, select, where, order).

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all articles for users who have a username which starts with 'a'.
    // Order it by the created date.
    // Note by putting 'a' as a second parameter will generate `#__content` AS `a`
    $query
        ->select(['a.*', 'b.username', 'b.name'])
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

El método join anterior nos permite consultar las tablas user y content,
la recuperación de los artículos con detalles del autor. También hay
métodos de conveniencia para las uniones:

- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_innerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">innerJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_leftJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">leftJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_rightJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">rightJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_outerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">outerJoin()</a>

Podemos utilizar varias combinaciones consultar a través de más de dos
tablas:

    $query
        ->select(array('a.*', 'b.username', 'b.name', 'c.*', 'd.*'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->join('LEFT', $db->quoteName('#__user_profiles', 'c') . ' ON (' . $db->quoteName('b.id') . ' = ' . $db->quoteName('c.user_id') . ')')
        ->join('RIGHT', $db->quoteName('#__categories', 'd') . ' ON (' . $db->quoteName('a.catid') . ' = ' . $db->quoteName('d.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

Observa cómo el encadenamiento hace que el código fuente sea mucho más
legible para estas consultas más largas.

En algunos casos, también tendrás que utilizar la cláusula AS, a la hora
de seleccionar los elementos para evitar conflictos entre los nombres de
las columnas. En este caso, de selección múltiple, las declaraciones
pueden ser encadenadas en conjunción con el segundo parámetro de
\$db-\>quoteName.

    $query
        ->select('a.*')
        ->select($db->quoteName('b.username', 'username'))
        ->select($db->quoteName('b.name', 'name'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b'), $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id'))
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

Un segundo array también puede ser utilizado como el segundo parámetro
de la instrucción select para rellenar los valores de la cláusula AS.
Recuerda incluir nulls en el segundo array que correspondan a las
columnas en el primer array y para los que no deseas utilizar la
cláusula AS:

    $query
        ->select(['a.*'])
        ->select($db->quoteName(array('b.username', 'b.name'), ['username', 'name']))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

## Using prepared statements

With Joomla! 4.0 we have [moved all queries to use prepared
statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "J4.x:Moving Joomla To Prepared Statements").
For easier use of prepared statements we introduced some helper
functions and allow to use arrays in several function calls. Simple
query with prepared statements.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

You see that we don't add the **\$credentials\['username'\]** directly
to the query, instead we add the placeholder **:username** into the
query and **bind** the variable to the query. When we bind a variable to
a query we don't need to escape nor to quote it. Beware that binding a
variable will always be a reference. A nice side effect of this, is that
you can manipulate the query in a loop.

    $listOfUsernames = [ 'admin', 'user1' ];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $username);

    foreach($listOfUsernames as $name)
    {
      $username = $name;
      $this->db->setQuery($query);
      $user = $this->db->loadObject();
      print_r($user);
    }

In the loop we set the previously bound \$username variable with the
\$name variable from the loop, then we have to set the query again
(because Joomla reset the database driver after query execution which is
only true for load\* functions). The result of this will be multiple
queries with different username values. We can use arrays to add
multiple variables at once.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('id') . ' = :id')
        ->bind([':username', ':id'], [$credentials['username'], 42], [Joomla\Database\ParameterType::STRING, Joomla\Database\ParameterType::INTEGER]);

We add **username** and **id** as bind parameter and set the correct
ParameterType for each variable. It's also possible to use one variable
for all bind values and ParameterTypes.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('password') . ' = :password')
        ->bind([':username', ':password], $credentials['username']);

The parameter :username and :password get set to the same value and the
default ParameterType. The function **whereIn()** and **whereNotIn()**
always use prepared statements, internal these functions uses the
**bindArray** function. It can be used to bind an array of variables
without specifying the placeholder.

    $userids = [1,2,3,4];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'));

    $parameterNames = $query->bindArray($userids);

    $query->where($this->db->quoteName('id') . ' IN (' . implode(',', $parameterNames) . ')');

The **bindArray** function returns an array of placeholders. The index
is unique for the whole query.

    $placeholders = [
      ':preparedArray1',
      ':preparedArray2',
      ':preparedArray3',
      ':preparedArray4'
    ];

## Resultados de la Consulta

La clase database contiene muchos métodos para trabajar con el conjunto
de resultados de al consulta.

### Resultado de Valor Único

#### loadResult()

Se utiliza **loadResult()** cuando se espera un solo valor como
resultado de la consulta de base de datos.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

Esto es a menudo el resultado de una consulta 'count' para obtener un
número de registros:

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('COUNT(*)');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('name')." = :value");
    $query->bind('value', $value)

    // Reset the query using our newly populated query object.
    $db->setQuery($query);
    $count = $db->loadResult();

o cuando estás buscando un único campo de una sola fila de la tabla (o
posiblemente un único campo de la primera fila devuelta).

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('field_name');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('some_name')." = :value");
    $query->bind(':value', $some_value);

    $db->setQuery($query);
    $result = $db->loadResult();

### Resultado de una Sola Fila

Cada una de estas funciones devuelve el resultados de un único registro
de la base de datos, aunque puede haber varios registros que cumplan los
criterios que se hayan establecido. Para obtener más registros de lo que
necesitas llamar a la función nuevamente.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadRow()

`loadRow()` devuelve un array indexado a partir de un único registro de
la tabla:

    . . .
    $db->setQuery($query);
    $row = $db->loadRow();
    print_r($row);

se tendrá:

    Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ) 

Puedes acceder a los valores individuales mediante el uso de:

    $row['index'] // e.g. $row['2']

Notas:

1.  Los índices del array son numéricos a partir de cero.
2.  Si bien puedes repetir la llamada para obtener más filas, una de las
    funciones que devuelve varias filas puede ser más útil.

#### loadAssoc()

`loadAssoc()` devuelve un array asociativo a partir de un único registro
de la tabla:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssoc();
    print_r($row);

se tendrá:

    Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith )

Puedes acceder a los valores individuales mediante el uso de:

    $row['name'] // e.g. $row['email']

Notas:

1.  Si bien puedes repetir la llamada para obtener más filas, una de las
    funciones que devuelve varias filas puede ser más útil.

#### loadObject()

loadObject\> devuelve un objeto PHP a partir de un único registro de la
tabla:

    . . .
    $db->setQuery($query);
    $result = $db->loadObject();
    print_r($result);

se tendrá:

    stdClass Object ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith )

Puedes acceder a los valores individuales mediante el uso de:

    $result->index // e.g. $result->email

Notas:

1.  Si bien puedes repetir la llamada para obtener más filas, una de las
    funciones que devuelve varias filas puede ser más útil.

### Resultado de una Sola Columna

Cada una de estas funciones devolverá resultados de una sola columna de
la base de datos.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadColumn()

`loadColumn()` devuelve un array indexado a partir de una única columna
de la tabla:

    $query->select('name'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn();
    print_r($column);

se tendrá:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle )

Puedes acceder a los valores individuales mediante el uso de:

    $column['index'] // e.g. $column['2']

Notas:

1.  Los índices del array son numéricos a partir de cero.
2.  `loadColumn()` es equivalente a loadColumn(0).

#### loadColumn(\$index)

loadColumn(\$index) devuelve un array indexado a partir de una única
columna de la tabla:

    $query->select(array('name', 'email', 'username'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn(1);
    print_r($column);

se tendrá:

    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example )

Puedes acceder a los valores individuales mediante el uso de:

    $column['index'] // e.g. $column['2']

loadColumn(\$index) permite iterar a través de una serie de columnas en
los resultados

    . . .
    $db->setQuery($query);
    for ( $i = 0; $i <= 2; $i++ ) {
      $column = $db->loadColumn($i);
      print_r($column);
    }

se tendrá:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle ),
    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example ),
    Array ( [0] => johnsmith, [1] => magdah, [2] => ydegaulle )

Notas:

1.  Los índices del array son numéricos a partir de cero.

### Resultados de filas Múltiples

Cada una de estas funciones devolverá resultados de varios registros de
la base de datos.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadRowList()

`loadRowList()` devuelve un array indexado de arrays indexados con los
registros de la tabla de devueltos por la consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadRowList();
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [0] => Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ), 
    [1] => Array ( [0] => 2, [1] => Magda Hellman, [2] => magda_h@domain.example, [3] => magdah ), 
    [2] => Array ( [0] => 3, [1] => Yvonne de Gaulle, [2] => ydg@domain.example, [3] => ydegaulle ) 
    )

Puedes acceder a las filas individuales mediante el uso de:

    $row['index'] // e.g. $row['2']

y puedes acceder a los valores individuales mediante el uso de:

    $row['index']['index'] // e.g. $row['2']['3']

Notas:

1.  Los índices del array son numéricos a partir de cero.

#### loadAssocList()

`loadAssocList()` devuelve un array indexado de arrays asociativos con
los registros de la tabla de devueltos por la consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList();
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [0] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    ) 

Puedes acceder a las filas individuales mediante el uso de:

    $row['index'] // e.g. $row['2']

y puedes acceder a los valores individuales mediante el uso de:

    $row['index']['column_name'] // e.g. $row['2']['email']

#### loadAssocList(\$clave)

`loadAssocList('clave')` devuelve un array asociativo - indexado por
'clave' - de array asociativos a partir de los registros de la tabla
devueltos por la consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('username');
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [johnsmith] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Puedes acceder a las filas individuales mediante el uso de:

    $row['key_value'] // e.g. $row['johnsmith']

y puedes acceder a los valores individuales mediante el uso de:

    $row['key_value']['column_name'] // e.g. $row['johnsmith']['email']

Nota: La 'clave' debe ser un nombre de columna válido de la tabla; no
tiene que ser un Índice o una Clave Principal. Pero si no tiene un valor
único no puede ser capaz de recuperar los resultados de forma fiable.

#### loadAssocList(\$clave, \$columna)

`loadAssocList('clave', 'columna')` devuelve un array asociativo,
indexado por 'clave', de los valores de la columna llamada 'columna'
devueltos por la consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('id', 'username');
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [1] => John Smith, 
    [2] => Magda Hellman, 
    [3] => Yvonne de Gaulle,
    )

Nota: La 'clave' debe ser un nombre de columna válido de la tabla; no
tiene que ser un Índice o una Clave Principal. Pero si no tiene un valor
único no puede ser capaz de recuperar los resultados de forma fiable.

#### loadObjectList()

`loadObjectList()` devuelve un array indexado de objetos PHP a partir de
los registros de la tabla devueltos por la consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList();
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [0] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Puedes acceder a las filas individuales mediante el uso de:

    $row['index'] // e.g. $row['2']

y puedes acceder a los valores individuales mediante el uso de:

    $row['index']->name // e.g. $row['2']->email

#### loadObjectList(\$clave)

`loadObjectList('clave')` devuelve un array asociativo - indexado por
'clave', de objetos de los registros de la tabla devueltos por la
consulta:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList('username');
    print_r($row);

va a dar (con saltos de línea añadido para mayor claridad):

    Array ( 
    [johnsmith] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Puedes acceder a las filas individuales mediante el uso de:

    $row['key_value'] // e.g. $row['johnsmith']

y puedes acceder a los valores individuales mediante el uso de:

    $row['key_value']->column_name // e.g. $row['johnsmith']->email

Nota: La 'clave' debe ser un nombre de columna válido de la tabla; no
tiene que ser un Índice o una Clave Principal. Pero si no tiene un valor
único no puede ser capaz de recuperar los resultados de forma fiable.

### Métodos Misceláneos para Grupos de Resultados

#### getNumRows()

`getNumRows()` devuelve como resultado el número de filas encontrado por
la última consulta SELECT o SHOW y a la espera de ser leído. Para
obtener un resultado a partir de getNumRows() tienes que ejecutarlo
**después** de la consulta y **antes** de obtener algún resultado. Para
recuperar el número de filas afectadas por una consulta INSERT, UPDATE,
REPLACE o DELETE, usa getAffectedRows().

    . . .
    $db->setQuery($query);
    $db->execute();
    $num_rows = $db->getNumRows();
    print_r($num_rows);
    $result = $db->loadRowList();

devolverá

    3

Nota: getNumRows() sólo es válida para declaraciones como SELECT o SHOW
que devuelven un conjunto de resultados reales. Si ejecuta getNumRows()
después de loadRowList() o cualquier otro método de recuperación -
obtendrás una Advertencia PHP:

    Warning: mysql_num_rows(): 80 is not a valid MySQL result resource 
    in libraries\joomla\database\database\mysql.php on line 344

### Ver también

- [Insertar, Actualizar y Eliminar datos utilizando
  JDatabase](https://docs.joomla.org/J4.x:Inserting,_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting, Updating and Removing data using JDatabase")
- [Moving Joomla To Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Usar los métodos UNION en consultas de base de
  datos](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a href="https://php.net/manual/en/pdo.prepared-statements.php"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">The PHP Manual on Prepared
  statements</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  CMS 3.8 API</a>

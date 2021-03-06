---
layout: default
language: 'es-es'
version: '4.0'
title: 'Registro'
keywords: 'registry'
---

# Registry Component

* * *

![](/assets/images/document-status-stable-success.svg) ![](/assets/images/version-{{ page.version }}.svg)

## Resumen

[Phalcon\Registry](api/phalcon_registry#registry) is an object oriented array. It extends [Phalcon\Collection](collection) but cannot be extended itself since all of its methods are declared `final`. Ofrece velocidad, así como implementaciones de varias interfaces PHP. Estas son:

- [ArrayAccess](https://php.net/manual/en/class.arrayaccess.php)
- [Countable](https://php.net/manual/en/class.countable.php)
- [IteratorAggregate](https://php.net/manual/en/class.iteratoraggregate.php)
- [JsonSerializable](https://php.net/manual/en/class.jsonserializable.php)
- [Serializable](https://php.net/manual/en/class.serializable.php)

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);
```

## Constructor

Puede construir el objeto como cualquier otro objeto en PHP. Sin embargo, el constructor acepta un parámetro opcional `array`, que rellenará el objeto por usted.

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);
```

## Reusing

También puede reutilizar el componente, volviéndolo a rellenar. [Phalcon\Registry](api/phalcon_registry#registry) exposes the `clear()` and `init()` methods, which will clear and repopulate the internal array respectively,

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->count(); // 2

$data = [
    'year' => 1776,
];

$collection->clear();

$collection->init($data);

echo $collection->count(); // 1
```

## Get

As mentioned above, [Phalcon\Registry](api/phalcon_registry#registry) implements several interfaces, in order to make the component as flexible as possible. Retrieving data stored in an element can be done by using:

- Propiedad
- `__get()`
- Acceso como arreglo (`$collection[$element]`)
- `offsetGet()`
- `get()`

The fastest way is by using the property syntax:

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->year; // 1776
```

You can use `__get($element)` but it is not advisable as it is much slower than the property syntax. The same applies to `offsetGet`

```php
echo $collection->__get('year');           // 1776
echo $collection['year'];                  // 1776
echo $collection->offsetGet('year');       // 1776
echo $collection->get('year', 1776, true); // 1776
```

```php
public function get(
    string $element, 
    mixed $defaultValue = null, 
    string $cast = null
):  mixed
```

Using `get()` offers three extra parameters. When `$defaultValue` is defined in the call and the element is not found, `$defaultValue` will be returned. The `cast` parameter accepts a string that defines what the returned value will be casted. The available values are:

- `array`
- `bool`
- `boolean`
- `double`
- `float`
- `int`
- `integer`
- `null`
- `object`
- `string`

## Has

To check whether an element exists or not in the collection, you can use the following:

- `isset()` on the property
- `__isset()`
- array based isset (`isset($collection[$element])`)
- `offsetExists()`
- `has()`

The fastest way is by using the property syntax:

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo isset($collection->year); // true
```

You can use `__isset(element)` but it is not advisable as it is much slower than the property syntax. The same applies to `offsetExists`

```php
echo $collection->__isset('year');        // true
echo isset($collection['year']);          // true
echo $collection->offsetExists('year');   // true
echo $collection->has('year', true);      // true
```

```php
public function has(string $element):  bool
```

## Set

Para establecer un elemento en la colección, puede utilizar lo siguiente:

- assign the value to the property
- `__set()`
- array based assignment 
- `offsetSet()`
- `set()`

The fastest way is by using the property syntax:

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
];

$collection = new Registry($data);

$collection->year = 1776;
```

Puedes usar `__set($element, $value)` pero no es recomendable ya que es mucho más lento que la sintaxis de propiedades. Lo mismo se aplica a `offsetSet`

```php
$collection->__set('year', 1776);
$collection['year'] = 1776;
$collection->offsetSet('year', 1776);
$collection->set('year', 1776); 
```

## Remove

Para eliminar un elemento en la colección, puede utilizar lo siguiente:

- unset the property
- `__unset()`
- array based unset 
- `offsetUnset()`
- `remove()`

The fastest way is by using the property syntax:

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
];

$collection = new Registry($data);

unset($collection->year);
```

Puedes usar `__unset($element)` pero no es recomendable ya que es mucho más lento que la sintaxis de propiedades. Lo mismo se aplica a `offsetUnset`

```php
$collection->__unset('year');
unset($collection['year']);
$collection->offsetUnset('year');
$collection->remove('year'); 
```

```php
public function remove(string $element):  void
```

## Iteration

Dado que el objeto de colección implementa `\IteratorAggregate`, puedes iterar con facilidad a través del objeto. El método `getIterator()` devuelve un objeto `ArrayIterator()`

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

foreach ($collection as $key => $value) {
    echo $key . ' - ' . $value . PHP_EOL;
}
```

## Count

La implementación de la interfaz `\Countable` expone el método `count()`, que almacena el número de elementos en la colección.

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->count(); // 2
```

## Serialization

Las interfaces `\Serializable` y `\JsonSerializable` exponen métodos que le permiten serializar y deserializar un objeto. `serialize()` y `unserialize()` utilizan las funciones `serialize` y `unserialize` de PHP. `jsonSerialize()` devuelve un arreglo que puede ser usado con `json_encode()` para serializar el objeto.

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->serialize();    // a:2:{s:6:"colors";a:3:{i:0;s:3:"red";i:1;s:5:"white";i:2;s:4:"blue";}s:4:"year";i:1776;}

$serialized = 'a:2:{s:6:"colors";a:3:{i:0;s:3:"red";i:1;s:5:"white";i:2;s:4:"blue";}s:4:"year";i:1776;}';
$collection->unserialize($serialized);

echo $collection->jsonSerialize(); // $data
```

## Transformations

[Phalcon\Registry](api/phalcon_registry#registry) also exposes two transformation methods: `toArray()` and `toJson(int $options)`. `toArray()` devuelve el objeto transformado como un arreglo. Este método devuelve el mismo *array* que `jsonSerialize()`.

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->toArray(); // $data
```

`toJson(int $options)` devuelve una representación JSON del objeto. Utiliza `json_encode()` internamente y acepta un parámetro, que representa las banderas que `json_encode` acepta. Por defecto, las opciones se configuran con el valor `74`, ([RFC4327](https://www.ietf.org/rfc/rfc4627.txt)) que se traduce a:

- `JSON_HEX_TAG`
- `JSON_HEX_APOS`
- `JSON_HEX_AMP`
- `JSON_HEX_QUOT`
- `JSON_UNESCAPED_SLASHES`

Puede pasar cualquier bandera válida al método según sus necesidades.

```php
<?php

use Phalcon\Registry;

$data = [
    'colors' => [
        'red',
        'white',
        'blue',
    ],
    'year'   => 1776,
];

$collection = new Registry($data);

echo $collection->toJson(); // ["red","white","blue"],"year":1776}

echo $collection->toJson(74 + JSON_PRETTY_PRINT);
/**
{
    "colors": [
        "red",
        "white",
        "blue"
    ],
    "year": 1776
}
*/
```
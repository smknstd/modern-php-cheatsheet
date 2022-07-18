# PHP Moderno Cheatsheet

![PHP Moderno Cheatsheet](https://i.imgur.com/2STEtgG.png)


> Si te gusta este contenido, podés contactarme o seguirme en Twitter. :+1:

[![Tweet for help](https://img.shields.io/twitter/follow/smknstd?label=Tweet%20%40smknstd&style=social)](https://twitter.com/smknstd/)

Traducción por: [Cristian Ferreyra](https://github.com/backendrulz) [![backendrulz](https://img.shields.io/twitter/follow/backendrulz?label=Tweet%20%40backendrulz&style=social)](https://twitter.com/backendrulz/)
> **Nota de traducción:** Elegí no traducir algunos términos ya que son extremadamente técnicos y podrían dificultar el aprendizaje de futuros lectores.

## Introducción

### Motivación

Este documento es un cheatsheet para PHP con código que encontrará con frecuencia en proyectos modernos.

Esta guía no está destinada a enseñarte PHP desde cero, sino a ayudar a los desarrolladores con conocimientos básicos que pueden tener dificultades para familiarizarse con las bases de código modernas (o para aprender Laravel o Symfony, por ejemplo) debido a los nuevos conceptos y características que PHP ha introducido a lo largo de los años.

> **Nota:** Los conceptos presentados acá se basan en la versión más reciente de PHP disponible ([PHP 8.1] (https://www.php.net/releases/8.1/es.php) en el momento de la última actualización)

### Recursos complementarios

Cuando tengas dificultad para comprender un concepto, te sugiero que busques respuestas en los siguientes sitios:
- [Stitcher's blog](https://stitcher.io/blog)
- [PHP.Watch](https://php.watch/versions)
- [Exploring php 8.0](https://leanpub.com/exploringphp80)
- [PHP The Right Way](https://phptherightway.com/)
- [StackOverflow](https://stackoverflow.com/questions/tagged/php)

### Lanzamientos recientes de PHP

| Version                                      |Fecha de lanzamiento|
|----------------------------------------------|---|
| [PHP 8.1](https://www.php.net/releases/8.1/es.php) |Noviembre 2021|
| [PHP 8.0](https://www.php.net/releases/8.0/es.php) |Noviembre 2020|
| PHP 7.4                                      |Noviembre 2019|
| PHP 7.3                                      |Diciembre 2018|
| PHP 7.2                                      |Noviembre 2017|
| PHP 7.1                                      |Diciembre 2016|
| PHP 7.0                                      |Diciembre 2015|

Mas información en [php.net](https://www.php.net/supported-versions.php).

## Tabla de contenidos

- [PHP moderno cheatsheet](#php-moderno-cheatsheet)
    * [Introducción](#introducción)
        + [Motivación](#motivación)
        + [Recursos complementarios](#recursos-complementarios)
        + [Lanzamientos recientes de PHP](#lanzamientos-recientes-de-php)
    * [Tabla de contenidos](#tabla-de-contenidos)
    * [Fundamentos](#fundamentos)
        + [Parámetro por defecto de función](#parámetro-por-defecto-de-función)
        + [Coma final](#coma-final)
        + [Declaración de tipo](#declaración-de-tipo)
        + [Desestructuración de matrices](#desestructuración-de-matrices)
        + [Null Coalescing](#null-coalescing)
        + [Operador Nullsafe](#operador-nullsafe)
        + [Operador Spread](#operador-spread)
        + [Argumentos nombrados](#argumentos-nombrados)
        + [Funciones flecha](#funciones-flecha)
        + [Expresión de coincidencia Match](#expresión-de-coincidencia-match)
        + [Interfaz Stringable](#interfaz-stringable)

## Fundamentos

### Parámetro por defecto de función

Podés establecer un valor predeterminado para tus parámetros de función:

```php
function myFunction($param = 'foo')
{
    return $param;
}
$a = myFunction();
// $a = 'foo'

$b = myFunction('bar');
// $b = 'bar'
```

Pero si enviás una propiedad nula o indefinida, no se utilizará el valor predeterminado:

```php
function myFunction($param = 'foo')
{
    return $param;
}
$a = myFunction(null);
// $a = null

$b = myFunction($undefined); // PHP Warning:  Undefined variable $undefined
// $b = null
```

### Coma final

Una coma final es un símbolo de coma que se escribe después del último elemento de una lista de elementos. Uno de los principales beneficios cuando se usa con multilíneas es que [las diferencias son más limpias](https://medium.com/@nikgraf/why-you-should-enforce-dangling-commas-for-multiline-statements-d034c98e36f8).

#### Matriz

Podés usar una coma final en matrices:

```php
$array = [
    'foo',
    'bar',
];
```

#### Declaración de uso agrupado

![php-version-72](https://shields.io/badge/php->=7.2-blue)

Desde PHP 7.2, podés usar una coma final en declaraciones de uso agrupadas:

```php
use Symfony\Component\HttpKernel\{
    Controller\ControllerResolverInterface,
    Exception\NotFoundHttpException,
    Event\PostResponseEvent,
};
```

#### Llamada de función y método

![php-version-73](https://shields.io/badge/php->=7.3-blue)

Desde PHP 7.3, podés usar una coma final al llamar a una función:

```php
function myFunction($foo, $bar)
{
    return true;
}
$a = myFunction(
    'baz',
    'qux',
);
```

y al llamar a un método:

```php
$f = new Foo();
$f->myMethod(
    'baz',
    'qux',
);
```

#### Parámetros de función

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Desde PHP 8.0, podés usar una coma final al declarar los parámetros de una función:

```php
function myFunction(
    $foo,
    $bar,
)
{
    return true;
}
```

#### Declaración de uso

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Desde PHP 8.0, podés usar una coma final en la declaración de uso:

```php
function() use (
    $foo,
    $bar,
)
{
    return true;
}
```

### Declaración de tipo

![php-version-70](https://shields.io/badge/php->=7.0-blue)

Con la declaración de tipo, se puede especificar el tipo de datos esperado para una propiedad que se aplicará en tiempo de ejecución. Admite muchos tipos, como tipos escalares (int, string, bool y float), pero también array, iterable, object, stdClass, etc.

Podés establecer un tipo para el parámetro de una función:

```php
function myFunction(int $param)
{
    return $param;
}
$a = myFunction(10);
// $a = 10
$b = myFunction('foo'); // TypeError: myFunction(): Argument #1 ($param) must be of type int, string given
```

También se puede establecer un tipo de retorno para una función:

```php
function myFunction() : int
{
    return 'foo';
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type int, string returned
```

Cuando una función no debería devolver algo, se puede usar el tipo "void":

```php
function myFunction() : void
{
    return 'foo';
}
// PHP Fatal error:  A void function must not return a value
```

Tampoco puede devolver null:

```php
function myFunction() : void
{
    return null;
}
// PHP Fatal error:  A void function must not return a value
```

Sin embargo, usar return para salir de la función es válido:

```php
function myFunction() : void
{
    return;
}
$a = myFunction();
// $a = null
```

#### Propiedad de clase

![php-version-74](https://shields.io/badge/php->=7.4-blue)

Podés establecer un tipo en una propiedad de clase:

```php
Class Foo()
{
    public int $bar;
}
$f = new Foo();
$f->bar = 'baz'; // TypeError: Cannot assign string to property Foo::$bar of type int
```

#### Tipo de unión

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Podés utilizar un "tipo de unión" que acepte valores de varios tipos diferentes, en lugar de uno solo:

```php
function myFunction(string|int|array $param) : string|int|array
{
    return $param;
}
```

También funciona con las propiedades de la clase:

```php
Class Foo()
{
    public string|int|array $bar;
}
```

#### Tipo de intersección

![php-version-81](https://shields.io/badge/php->=8.1-blue)

Desde PHP 8.1, podés usar un "tipo de intersección" (también conocido como "puro") que exige que un valor dado pertenezca a todos los tipos. Por ejemplo, este parámetro necesita implementar las interfaces *Stringable* y *Countable*:

```php
function myFunction(Stringable&Countable $param): Stringable&Countable
{
    return $param;
}
Class Foo
{
    public function __toString() {
        return "something";
    }
}
myFunction(new Foo());
// TypeError: myFunction(): Argument #1 ($param) must be of type Stringable&Countable, Foo given
```

También funciona con propiedades de clase:

```php
Class Foo
{
    public Stringable&Countable $bar;
}
```

El tipo de intersección solo admite clases e interfaces. Los tipos escalares (string, int, array, null, mixed, etc.) no están permitidos:

```php
function myFunction(string&Countable $param)
{
    return $param;
}
// PHP Fatal error:  Type string cannot be part of an intersection type
```

##### Recurso externo

- [Intersection types on PHP.Watch](https://php.watch/versions/8.1/intersection-types)

#### Tipo Nullable

![php-version-71](https://shields.io/badge/php->=7.1-blue)

Cuando un parámetro no tiene tipo, puede aceptar un valor nulo:

```php
function myFunction($param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

Pero tan pronto como un parámetro tenga un tipo, ya no aceptará un valor nulo y devolverá un error:

```php
function myFunction(string $param)
{
    return $param;
}
$a = myFunction(null); // TypeError: myFunction(): Argument #1 ($param) must be of type string, null given
```

Si una función tiene un tipo de retorno, tampoco aceptará un valor nulo:

```php
function myFunction() : string
{
    return null;
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type string, null returned
```

Podés hacer una declaración de tipo explícitamente "nullable":

```php
function myFunction(?string $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

o con un tipo de unión:

```php
function myFunction(string|null $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

También funciona con el tipo de retorno:

```php
function myFunction(?string $param) : ?string
{
    return $param;
}
// or
function myFunction(string|null $param) : string|null
{
    return $param;
}
```

Pero void no puede ser "nullable":

```php
function myFunction() : ?void
{
   // algún código
}
// PHP Fatal error:  Void type cannot be nullable
```

o

```php
function myFunction() : void|null
{
   // algún código
}
// PHP Fatal error:  Void type cannot be nullable
```

Podés establecer un tipo que acepta valores null en una propiedad de clase:

```php
Class Foo()
{
    public int|null $bar;
}
$f = new Foo();
$f->bar = null;
$a = $f->bar;
// $a = null
```

### Desestructuración de matrices

Podés desestructurar matrices para extraer varios elementos en variables independientes.

#### Matriz indexada

![php-version-40](https://shields.io/badge/php->=4.0-blue)

Considerando una matriz indexada como:

```php
$array = ['foo', 'bar', 'baz'];
```

Podés desestructurar la matriz usando la sintaxis de lista:

```php
list($a, $b, $c) = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

O a partir de PHP 7.1, usando la sintaxis corta:

```php
[$a, $b, $c] = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

Podés saltar elementos:

```php
list(, , $c) = $array;

// $c = 'baz'
```

O a partir de PHP 7.1, usando la sintaxis corta:

```php
[, , $c] = $array;

// $c = 'baz'
```

Cuando intentes desestructurar un índice que no existe, obtendrás una advertencia:

```php
list($a, $b, $c, $d) = $array; // PHP Warning:  Undefined array key 3

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
// $d = null;
```

También podés intercambiar variables con asignaciones de desestructuración, considerando que tenés variables como:
```php
$a = 'foo';
$b = 'bar';
```

Entonces, si necesitás intercambiar `$a` y `$b` en lugar de usar una variable temporal como esta:

```php
$temp = $a;
$a = $b;
$b = $temp;

// $a = 'bar'
// $b = 'foo'
```

Podés intercambiarlas usando la sintaxis de lista:

```php
list($a, $b) = [$b, $a];

// $a = 'bar'
// $b = 'foo'
```

O desde PHP 7.1, la sintaxis abreviada:

```php
[$a, $b] = [$b, $a];

// $a = 'bar'
// $b = 'foo'
```

#### Matriz asociativa

![php-version-71](https://shields.io/badge/php->=7.1-blue)

Considerando una matriz asociativa (con clave de cadena) como:

```php
$array = [
    'foo' => 'value1',
    'bar' => 'value2',
    'baz' => 'value3',
];
```

La sintaxis de la lista anterior no funcionará con una matriz asociativa y recibirá una advertencia:

```php
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...

// $a = null
// $b = null
// $c = null
```

Pero desde PHP 7.1 (~ diciembre 2016), podés desestructurar la matriz con otra sintaxis basada en llaves:

```php
list('foo' => $a, 'bar' => $b, 'baz' => $c) = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

O usando la sintaxis corta:

```php
['foo' => $a, 'bar' => $b, 'baz' => $c] = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

También podés desestructurar solamente una parte de la matriz (el órden no importa):

```php
['baz' => $c, 'foo' => $a] = $array;

// $a = 'value1'
// $c = 'value3'
```

Cuando trates de desestructurar una llave que no exista en la matriz, recibirás una advertencia:

```php
list('moe' => $d) = $array; // PHP Warning:  Undefined array key "moe"

// $d = null
```

### Null Coalescing

![php-version-70](https://shields.io/badge/php->=7.0-blue)

Desde PHP 7.0 (~ diciembre 2015), podés usar el operador null coalescing para proporcionar un respaldo cuando una propiedad es nula sin error ni advertencia:

```php
$a = null;
$b = $a ?? 'fallback';

// $b = 'fallback'
```

Es equivalente a:

```php
$a = null;
$b = isset($a) ? $a : 'fallback';
// $b = 'fallback'
```

También funciona cuando la propiedad es indefinida:

```php
$a = $undefined ?? 'fallback';

// $a = 'fallback'
```

Cualquier otro valor de la propiedad no activará el respaldo (fallback):

```php
'' ?? 'fallback'; // ''
0 ?? 'fallback'; // 0
false ?? 'fallback'; // false
```

Se puede encadenar el null coalescing varias veces:

```php
$a = null;
$b = null;
$c = $a ?? $b ?? 'fallback';
// $c = 'fallback'
```

#### Operador Elvis

![php-version-53](https://shields.io/badge/php->=5.3-blue)

No se debe confundir con el operador ternario corto (también conocido como el operador elvis), que se introdujo en PHP 5.3:

```php
$a = null;
$b = $a ?: 'fallback';

// $b = 'fallback'
```

El operador ternario corto es equivalente a:

```php
$a = null;
$b = $a ? $a : 'fallback';
// $b = 'fallback'
```

El resultado entre null coalescing y el operador Elvis puede ser similar, pero también diferente para algunos valores específicos:

```php
'' ?: 'fallback'; // 'fallback'
0 ?: 'fallback'; // 'fallback'
false ?: 'fallback'; // 'fallback'
```

#### Null coalescing en matriz

Si existe una clave de matriz, no se activa el respaldo:

```php
$a = ['foo' => 'bar'];
$b = $a['foo'] ?? 'fallback';

// $b = 'bar'
```

Pero cuando la matriz no existe, se activa el respaldo sin error ni advertencia:

```php
$a = null;
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

O si la propiedad de la matriz no está definida, el respaldo se activa sin errores ni advertencias:

```php
$b = $undefined['foo'] ?? 'fallback';

// $b = 'fallback'
```

Cuando existe una matriz pero no se puede encontrar la clave, se activa el respaldo sin error ni advertencia:

```php
$a = [];
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

También funciona con matrices indexadas:

```php
$a = ['foo'];

// reminder: $a[0] = 'foo'

$b = $a[1] ?? 'fallback';

// $b = 'fallback'
```

También funciona con matrices anidadas. Si existe una clave de matriz anidada, no se activa el respaldo:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['bar'] ?? 'fallback';

// $b = 'baz'
```

Pero cuando no se puede encontrar la clave anidada, se activa el respaldo sin error ni advertencia:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['qux'] ?? 'fallback';

// $b = 'fallback'
```

#### Null coalescing en objeto

También podés usar el operador null coalescing con objectos.

##### Atributo del objeto

Si existe el atributo del objeto, no se activa el respaldo:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->foo ?? 'fallback';

// $b = 'bar'
```

Pero cuando no se puede encontrar el atributo del objeto, se activa el respaldo sin error ni advertencia:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->baz ?? 'fallback';

// $b = 'fallback'
```

##### Método de objeto

También podés utilizar el operador null coalescing en la llamada al método de un objeto. Si el método existe, entonces no se activa el respaldo:

```php
class Foo
{
    public function bar()
    {
        return 'baz';
    }
}

$a = new Foo();
$b = $a->bar() ?? 'fallback';

// $b = 'baz'
```

Pero cuando el método del objeto devuelve nulo, se activa el respaldo sin error ni advertencia:

```php
class Foo
{
    public function bar()
    {
        return null;
    }
}

$a = new Foo();
$b = $a->bar() ?? 'fallback';

// $b = 'fallback'
```

Si no se puede encontrar el método del objeto, null coalescing no funcionará y obtendrás un error:

```php
class Foo
{
    public function bar()
    {
        return 'baz';
    }
}

$a = new Foo();
$b = $a->baz() ?? 'fallback'; // PHP Error:  Call to undefined method baz()
```

##### Método encadenado

Cuando se utilizan métodos encadenados en un objeto y no se puede encontrar un elemento intermediario, null coalescing no funcionará y obtendrás un error:

```php
class Foo
{
    public function bar()
    {
        return (object)[];
    }
}

$a = new Foo();
$b = $a->bar()->baz() ?? 'fallback'; // PHP Error:  Call to undefined method baz()
```

#### Operador de asignación de Null Coalescing

![php-version-74](https://shields.io/badge/php->=7.4-blue)

Podés establecer un valor predeterminado para una propiedad cuando es nulo:

```php
$a = null;
$a = $a ?? 'foo';
// $a = 'foo'
```

Desde PHP 7.4, podés usar el operador de asignación de null coalescing para hacer lo mismo:

```php
$a = null;
$a ??= 'foo';
// $a = 'foo'
```

### Operador Nullsafe

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Cuando intentes leer una propiedad o llames a un método en nulo, devolverá una advertencia y un error:

```php
$a = null;
$b = $a->foo; // PHP Warning:  Attempt to read property "foo" on null
// $b = null

$c = $a->foo(); // PHP Error:  Call to a member function foo() on null
```

Con el operador nullsafe, se pueden hacer ambas cosas sin advertencia ni error:

```php
$a = null;
$b = $a?->foo;
// $b = null
$c = $a?->foo();
// $c = null
```

Se pueden encadenar varios operadores nullsafe:

```php
$a = null;
$b = $a?->foo?->bar;
// $b = null
$c = $a?->foo()?->bar();
// $c = null
```

Una expresión está en cortocircuito desde el primer operador null-safe que encuentra un valor nulo:

```php
$a = null;
$b = $a?->foo->bar->baz();
// $b = null
```

El operador nullsafe no tiene ningún efecto si el objetivo no es nulo:

```php
$a = 'foo';
$b = $a?->bar; // PHP Warning:  Attempt to read property "bar" on string
// $b = null
$c = $a?->baz(); // PHP Error:  Call to a member function baz() on string
```

El operador Nullsafe no puede manejar las matrices correctamente, pero aún puede tener algún efecto:

```php
$a = [];
$b = $a['foo']->bar;
// PHP Warning:  Undefined array key "foo"
// PHP Warning:  Attempt to read property "bar" on null
// $b = null

$c = $a['foo']?->bar; // PHP Warning:  Undefined array key "foo"
// $c = null

$d = $a['foo']->bar();
// PHP Warning:  Undefined array key "foo"
// PHP Error:  Call to a member function bar() on null

$e = $a['foo']?->bar(); // PHP Warning:  Undefined array key "foo"
// $e = null
```

No se puede usar el operador nullsafe para escribir, es de solo lectura:

```php
$a = null;
$a?->foo = 'bar'; // PHP Fatal error:  Can't use nullsafe operator in write context
```

### Operador Spread

#### Parámetro Variadic

![php-version-56](https://shields.io/badge/php->=5.6-blue)

Desde PHP 5.6 (~ agosto 2014), se puede agregar un parámetro variadic a cualquier función que te permita usar listas de argumentos con longitud variable:

```php
function countParameters(string $param, string ...$options) : int
{

    foreach ($options as $option) {
        // podés iterar en $options
    }

    return 1 + count($options);
}

countParameters('foo'); // 1
countParameters('foo', 'bar'); // 2
countParameters('foo', 'bar', 'baz'); // 3
```

El parámetro variadic siempre debe ser el último parámetro declarado:

```php
function countParameters(string ...$options, string $param)
{
   // algún código
}
// PHP Fatal error: Only the last parameter can be variadic
```

Solo se puede tener un parámetro variadic:

```php
function countParameters(string ...$options, string ...$moreOptions)
{
   // algún código
}
// PHP Fatal error: Only the last parameter can be variadic
```

No puede tener un valor predeterminado:

```php
function countParameters(string $param, string ...$options = [])
{
   // algún código
}
// PHP Parse error: Variadic parameter cannot have a default value
```

Cuando no es tipado, acepta cualquier valor:

```php
function countParameters(string $param, ...$options) : int
{
    return 1 + count($options);
}

$a = countParameters('foo', null, [], true);
// $a = 4
```

Cuando es tipado, tenés que usar valores correctamente definidos:

```php
function countParameters(string $param, string ...$options) : int
{
    return 1 + count($options);
}

countParameters('foo', null);
// TypeError: countParameters(): Argument #2 must be of type string, null given

countParameters('foo', []);
// TypeError: countParameters(): Argument #2 must be of type string, array given
```

#### Desempaquetando argumentos

![php-version-56](https://shields.io/badge/php->=5.6-blue)

Las matrices y los objetos traversable se pueden desempaquetar en listas de argumentos al llamar a funciones mediante el operador spread:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3];
$r = add(1, ...$array);

// $r = 6
```

La matriz puede tener más elementos de los necesarios:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3, 4, 5];
$r = add(1, ...$array);

// $r = 6
```

La matriz no puede tener menos elementos de los necesarios:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array); // TypeError: Too few arguments to function add(), 2 passed
```

Excepto cuando algunos argumentos tienen un valor predeterminado:

```php
function add(int $a, int $b, int $c = 0) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array);
// $r = 3
```

Si un argumento es tipado y el valor no coincide con el tipo, obtendrás un error:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = ['foo', 'bar'];
$r = add(1, ...$array); // TypeError: add(): Argument #2 ($b) must be of type int, string given
```

Desde PHP 8.0, es posible desempaquetar una matriz asociativa ya que usará [argumentos nombrados](#unpacking-named-arguments).

#### Desempaquetado de Matriz

##### Matriz indexada

![php-version-74](https://shields.io/badge/php->=7.4-blue)

Cuando se desea fusionar varias matrices, generalmente se usa `array_merge`:

```php
$array1 = ['baz'];
$array2 = ['foo', 'bar'];

$array3 = array_merge($array1,$array2);
// $array3 = ['baz', 'foo', 'bar']
```

Pero desde PHP 7.4 (~ noviembre 2019), se pueden desempaquetar matrices indexadas con el operador spread:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1];
// $array2 = ['baz', 'foo', 'bar']
```

Los elementos se fusionarán en el orden en que se pasen:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1, "qux"];
// $array2 = ['baz', 'foo', 'bar', "qux"]
```

No se realiza ninguna deduplicación:

```php
$array1 = ['foo', 'bar'];
$array2 = ['foo', ...$array1];
// $array2 = ['foo', 'foo', 'bar']
```

Se pueden desempaquetar varias matrices a la vez:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz'];
$array3 = [ ...$array1, ...$array2];
// $array3 = ['foo', 'bar', 'baz']
```

Se puede desempaquetar la misma matriz varias veces:

```php
$array1 = ['foo', 'bar'];
$array2 = [ ...$array1, ...$array1];
// $array2 = ['foo', 'bar', 'foo', 'bar']
```

Podés desempaquetar una matriz vacía sin errores ni advertencias:

```php
$array1 = [];
$array2 = ['foo', ...$array1];
// $array2 = ['foo']
```

Podés desempaquetar una matriz que no ha sido almacenada previamente en una propiedad:

```php
$array1 = [...['foo', 'bar'], 'baz'];
// $array1 = ['foo', 'bar', 'baz']
```

El desempaquetado solo funciona con matrices (u objetos que complementan la interfaz Traversable). Si intentás desempaquetar cualquier otro valor (como nulo), generará un error:

```php
$array1 = null;
$array2 = ['foo', ...$array1]; // PHP Error:  Only arrays and Traversables can be unpacked
```

Podés desempaquetar el resultado de una función/método:

```php
function getArray() : array
{
    return ['foo', 'bar'];
}

$array = [...getArray(), 'baz'];
// $array = ['foo', 'bar', 'baz']
```

##### Matriz asociativa

![php-version-81](https://shields.io/badge/php->=8.1-blue)

Desde php 8.1, podés desempaquetar una matriz asociativa (con clave de cadena):

```php
$array1 = ['foo' => 'bar'];
$array2 = [
   'baz' => 'qux',
   ...$array1
];
// $array2 = ['baz' => 'qux', 'foo' => 'bar',]
```

Podés desempaquetar la matriz con una clave ya existente:

```php
$array1 = ['foo' => 'bar'];
$array2 = [
   'foo' => 'baz',
   ...$array1
];
// $array2 = ['foo' => 'bar',]
```

Podés desempaquetar una matriz vacía sin error ni advertencia:

```php
$array1 = [];
$array2 = [
   ...$array1,
   ...[]
];
// $array2 = []
```

### Argumentos Nombrados

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Desde PHP 8.0, es posible pasar argumentos por nombre en lugar de su posición.

Considerando una función como esta:

```php
function concat(string $first, string $second) : string
{
    return $first . ' ' . $second;
}
$a = concat('foo', 'bar');
// $a = 'foo bar'
```

Se puede obtener el mismo resultado con la sintaxis del argumento con nombre:

```php
$a = concat(first: 'foo', second: 'bar');
// $a = 'foo bar'
```

Se puede llamar con argumentos en un orden diferente:

```php
$a = concat(second: 'bar', first: 'foo');
// $a = 'foo bar'
```

Se pueden omitir parámetros opcionales:

```php
function orGate(bool $option1 = false, bool $option2 = false, bool $option3 = false) : bool
{
   return $option1 || $option2 || $option3;
}
$a = orGate(option3: true);
// $a = true
```

Pero no podés omitir un argumento obligatorio:

```php
$a = concat(second: 'bar');
// TypeError: concat(): Argument #1 ($first) not passed
```

Tampoco se pueden incluir argumentos adicionales:

```php
$a = concat(first: 'foo', second: 'bar', third: 'baz');
// PHP Error:  Unknown named parameter $third
```

Los argumentos con nombre también funcionan con el constructor de objetos:

```php
Class Foo()
{
    public function __construct(
        public string $first,
        public string $second
    ) {}

}
$f = new Foo(first: 'bar', second: 'baz');
```

#### Variadics con nombre

Se puede utilizar argumentos con nombre con un parámetro variadic:

```php
function showParams(string ...$params) : array
{
    return $params;
}
$a = showParams(first: 'foo', second: 'bar', third: 'baz');
// $a = ["first" => "foo", "second" => "bar", "third" => "baz"]
```

#### Desempaquetando argumentos con nombre

Se puede desempaquetar una matriz asociativa como argumentos con nombre si las claves coinciden con los nombres de los argumentos:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [
    "b" => 2,
    "c" => 3
];
$r = add(1, ...$array);
// $r = 6
```

El orden de los elementos en la matriz asociativa no importa:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [
    "c" => 3,
    "b" => 2,
];
$r = add(1, ...$array);
// $r = 6
```

Si una clave no coincide con el nombre de un argumento, obtendrás un error:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [
    "b" => 2,
    "c" => 3,
    "d" => 4,
];
$r = add(1, ...$array); // PHP Error:  Unknown named parameter $d
```

#### Recursos adicionales

- [Named arguments in depth on stitcher's blof](https://stitcher.io/blog/php-8-named-arguments)
- [Named Parameters on PHP.Watch](https://php.watch/versions/8.0/named-parameters)

### Funciones flecha

![php-version-74](https://shields.io/badge/php->=7.4-blue)

Las funciones flecha son una alternativa a las [funciones anónimas](https://www.php.net/manual/es/functions.anonymous.php) usando una sintaxis mas corta. El objetivo principal es reducir la verbosidad cuando sea posible: si solo hay una expresión.

Acá hay un ejemplo de una función simple con una sola expresión:

```php
$foo = function ($bar) {
    return $bar + 1;
}
$a = $foo(1);
// $a = 2
```

Podés escribir la misma función usando una función flecha:

```php
$foo = fn ($bar) => $bar + 1;
$a = $foo(1);
// $a = 2
```

No le podés asignar un nombre a una función flecha:

```php
fn foo($bar) => $bar + 1;
// PHP Parse error: Syntax error, unexpected T_STRING, expecting '('
```

Podés usar una función flecha como un parámetro. Por ejemplo, como un parámetro "invocable" en [array_reduce](https://www.php.net/manual/es/function.array-reduce.php):

```php
$myArray = [10,20,30];

$total = array_reduce($myArray, fn ($carry, $item) => $carry + $item, 0);
// $total = 60
```

Se permite la sugerencia de tipo como en una función normal:

```php
fn (int $foo): int => $foo;
```

No es necesario usar `return` ya que no está permitido:

```php
fn ($foo) => return $foo;
// PHP Parse error: Syntax error, unexpected T_RETURN
```

#### Ámbito exterior

La función flecha requiere la palabra clave `use` para poder acceder a las propiedades desde el ámbito externo:

```php
$bar = 10;
$baz = fn ($foo) => $foo + $bar;
$a = $baz(1);
//$a = 11
```

La palabra clave `use` no está permitida:

```php
$bar = 10;
fn ($foo) use ($bar) => $foo + $bar;
// PHP Parse error: Syntax error, unexpected T_USE, expecting T_DOUBLE_ARROW
```

Podés usar `$this` como en cualquier otra función:

```php
fn () => $this->foo + 1;
```

### Expresión de coincidencia Match

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Desde PHP 8.0, existe una nueva sintaxis `match` similar a la sintaxis `switch`. Como cada caso coincidente solo debe contener una expresión, no se puede usar y reemplazar una declaración de cambio en cada situación. Sin embargo, es significativamente más corto y más fácil de leer.

La expresión `match` siempre devuelve un valor. Cada condición solo permite una sola expresión, e inmediatamente devuelve el valor y no fallará en las siguientes condiciones sin una declaración explícita de `break`:

```php
$foo = 'baz';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
    'qux' => 3,
}
// $a = 2
```

Lanza una excepción cuando el valor no puede coincidir:

```php
$foo = 'qux';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
}
// PHP Error:  Unhandled match value of type string
```

Pero admite una condición predeterminada:

```php
$foo = 'qux';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
    default => 3,
}
// $a = 3
```

Permite múltiples condiciones en un solo brazo:

```php
$foo = 'bar';
$a = match($foo) {
    'bar', 'baz' => 1,
    default => 2,
}
// $a = 1
```

Hace una comparación estricta de tipo seguro sin coerción de tipo (es como usar `===` en lugar de `==`):

```php
function showType($param) {
    return match ($param) {
        1 => 'Integer',
        '1' => 'String',
        true => 'Boolean',
    };
}

showType(1); // "Integer"
showType('1'); // "String"
showType(true); // "Boolean"
```

#### Recurso externo

- [Match expression on PHP.Watch](https://php.watch/versions/8.0/match-expression)

### Interfaz Stringable

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Desde PHP 8.0, hay una nueva interfaz llamada `Stringable`, que indica que una clase tiene un método mágico `__toString()`. PHP agrega automáticamente la interfaz `Stringable` a todas las clases que implementan ese método.

```php
interface Stringable {
    public function __toString(): string;
}
```

Cuando se define un parámetro con el tipo `Stringable`, comprobará que la clase dada implementa la interfaz `Stringable`:

```php
class Foo {
    public function __toString(): string {
        return 'bar';
    }
}

function myFunction(Stringable $param): string {
    return (string) $param;
}
$a = myFunction(new Foo);
// $a = 'bar'
```

Si una clase dada no implementa `__toString()`, obtendrá un error:

```php
class Foo {
}

function myFunction(Stringable $param): string {
    return (string) $param;
}
$a = myFunction(new Foo);
// TypeError: myFunction(): Argument #1 ($param) must be of type Stringable, Foo given
```

Un tipo `Stringable` no acepta `string`:

```php
function myFunction(Stringable $param): string {
    return (string) $param;
}
$a = myFunction('foo');
// TypeError: myFunction(): Argument #1 ($param) must be of type Stringable, string given
```

Por supuesto, para aceptar tanto `string` como `Stringable`, puede usar un tipo de unión:

```php
function myFunction(string|Stringable $param): string {
    return (string) $param;
}
```
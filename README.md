# Modern PHP Cheatsheet

![Modern PHP Cheatsheet](https://i.imgur.com/2STEtgG.png)


> If you like this content, you can ping me or follow me on Twitter :+1:

[![Tweet for help](https://img.shields.io/twitter/follow/smknstd?label=Tweet%20%40smknstd&style=social)](https://twitter.com/smknstd/)

## Introduction

### Motivation

This document is a cheatsheet for PHP you will frequently encounter in modern projects and most contemporary sample code.

This guide is not intended to teach you PHP from the ground up, but to help developers with basic knowledge who may struggle to get familiar with modern codebases (or let's say to learn Laravel or Symfony for instance) because of the new PHP concepts and features introduced over the years.

> **Note:** Concepts introduced here are based on the most recent version of PHP available ([PHP 8.0](https://www.php.net/releases/8.0/en.php) at the time of the last update)

### Complementary Resources

When you struggle to understand a notion, I suggest you look for answers on the following resources:
- [Stitcher's blog](https://stitcher.io/blog)
- [php.Watch](https://php.watch/versions)
- [PHP The Right Way](https://phptherightway.com/)
- [StackOverflow](https://stackoverflow.com/questions/tagged/php)

## Table of Contents

- [Modern PHP cheatsheet](#modern-php-cheatsheet)
    * [Introduction](#introduction)
        + [Motivation](#motivation)
        + [Complementary resources](#complementary-resources)
    * [Table of contents](#table-of-contents)
    * [Notions](#notions)
        + [Function default parameter value](#function-default-parameter-value)
        + [Type declaration](#type-declaration)
        + [Destructuring arrays](#destructuring-arrays)
        + [Null Coalescing](#null-coalescing)
        + [Nullsafe operator](#nullsafe-operator)
        + [Spread operator](#spread-operator)

## Notions

### Function default parameter value

You can set default value to your function parameters:

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

But if you send null or an undefined property, default value won't be used:

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

### Type declaration

![php-version-70](https://shields.io/badge/php->=7.0-blue)

With Type declaration you can specify the expected data type for a property that will be enforce at runtime. It supports many types like scalar types (int, string, bool, and float) but also array, iterable, object, stdClass, etc.

You can set a type to a function's parameter:

```php
function myFunction(int $param)
{
    return $param;
}
$a = myFunction(10);
// $a = 10
$b = myFunction('foo'); // TypeError: myFunction(): Argument #1 ($param) must be of type int, string given
```

You can set a return type to a function:

```php
function myFunction() : int
{
    return 'foo';
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type int, string returned
```

When a function should not return something, you can use the type "void":

```php
function myFunction() : void
{
    return 'foo';
}
// PHP Fatal error:  A void function must not return a value
```

You cannot return null either:

```php
function myFunction() : void
{
    return null;
}
// PHP Fatal error:  A void function must not return a value
```

However, using return to exit the function is valid:

```php
function myFunction() : void
{
    return;
}
$a = myFunction();
// $a = null
```

#### Class property

![php-version-74](https://shields.io/badge/php->=7.4-blue)

You can set a return type to a class property:

```php
Class Foo()
{
    public int $bar;
}
$f = new Foo();
$f->bar = 'baz'; // TypeError: Cannot assign string to property Foo::$bar of type int
```

#### Union type

![php-version-80](https://shields.io/badge/php->=8.0-blue)

You can use a “union type” that accepts values of multiple different types, rather than a single one:

```php
function myFunction(string|int|array $param) : string|int|array
{
    return $param;
}
```

It also works with class property:

```php
Class Foo()
{
    public string|int|array $bar;
}
```

#### Nullable type

![php-version-71](https://shields.io/badge/php->=7.1-blue)

When a parameter has no type, it can accept null value:

```php
function myFunction($param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

But as soon as a parameter has a type, it won't accept null value anymore and you'll get an error:

```php
function myFunction(string $param)
{
    return $param;
}
$a = myFunction(null); // TypeError: myFunction(): Argument #1 ($param) must be of type string, null given
```

If a function has a return type, it won't accept null value either:

```php
function myFunction() : string
{
    return null;
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type string, null returned
```

You can make a type declaration explicitly nullable:

```php
function myFunction(?string $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

or with a union type:

```php
function myFunction(string|null $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

It also works with return type:

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

But void cannot be nullable:

```php
function myFunction() : ?void
{
   // some code
} 
// PHP Fatal error:  Void type cannot be nullable
```

or

```php
function myFunction() : void|null
{
   // some code
}
// PHP Fatal error:  Void type cannot be nullable
```

You can set a nullable type to a class property:

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

### Destructuring arrays

You can destructure arrays to pull out several elements into separate variables.

#### Indexed array

![php-version-40](https://shields.io/badge/php->=4.0-blue)

Considering an indexed array like :

```php
$array = ['foo', 'bar', 'baz'];
```

You can destruct it using the list syntax:

```php
list($a, $b, $c) = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

Or since PHP 7.1, the shorthand syntax:

```php
[$a, $b, $c] = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

You can skip elements:

```php
list(, , $c) = $array;

// $c = 'baz'
```

Or since PHP 7.1, the shorthand syntax:

```php
[, , $c] = $array;

// $c = 'baz'
```

When you try to destruct an index that doesn't exist in the given array, you'll get a warning:

```php
list($a, $b, $c, $d) = $array; // PHP Warning:  Undefined array key 3

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
// $d = null;
```

#### Associative array

![php-version-71](https://shields.io/badge/php->=7.1-blue)

Considering an associative array (string-keyed) like :

```php
$array = [
    'foo' => 'value1',
    'bar' => 'value2',
    'baz' => 'value3',
];
```

Previous list syntax won't work with an associative array, and you'll get a warning:

```php
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...

// $a = null
// $b = null
// $c = null
```

But since PHP 7.1.0 (~ dec 2016), you can destruct it with another syntax based on keys:

```php
list('foo' => $a, 'bar' => $b, 'baz' => $c) = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

Or the shorthand syntax:

```php
['foo' => $a, 'bar' => $b, 'baz' => $c] = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

You can also destruct only a portion of the array (The order doesn't matter):

```php
['baz' => $c, 'foo' => $a] = $array;

// $a = 'value1'
// $c = 'value3'
```

When you try to destruct a key that doesn't exist in the given array, you'll get a warning:

```php
list('moe' => $d) = $array; // PHP Warning:  Undefined array key "moe"

// $d = null
```

### Null Coalescing

![php-version-70](https://shields.io/badge/php->=7.0-blue)

Since PHP 7.0 (~ dec 2015), you can use the null coalescing operator to provide a fallback when a property is null with no error nor warning:

```php
$a = null;
$b = $a ?? 'fallback';

// $b = 'fallback'
```

It is equivalent to:

```php
$a = null;
$b = isset($a) ? $a : 'fallback';
// $b = 'fallback'
```

It also works when property is undefined:

```php
$a = $undefined ?? 'fallback';

// $a = 'fallback'

$c = new class{};
$value = $c->myVariable ?? 'fallback';

// $value = 'fallback';
```

Every other value of the property won't trigger the fallback:

```php
'' ?? 'fallback'; // ''
0 ?? 'fallback'; // 0
false ?? 'fallback'; // false
```

You can chain null coalescing multiple times:

```php
$a = null;
$b = null;
$c = $a ?? $b ?? 'fallback';
// $c = 'fallback'
```

#### Elvis operator

![php-version-53](https://shields.io/badge/php->=5.3-blue)

It should not be confused with the shorthand ternary operator (aka the elvis operator), which was introduced in PHP 5.3:

```php
$a = null;
$b = $a ?: 'fallback';

// $b = 'fallback'
```

The shorthand ternary operator is equivalent to:

```php
$a = null;
$b = $a ? $a : 'fallback';
// $b = 'fallback'
```

Result between null coalescing and elvis operator can be similar, but also different for some specific values:

```php
'' ?: 'fallback'; // 'fallback'
0 ?: 'fallback'; // 'fallback'
false ?: 'fallback'; // 'fallback'
```

#### Null coalescing on array

If array key exists, then fallback isn't triggered:

```php
$a = ['foo' => 'bar'];
$b = $a['foo'] ?? 'fallback';

// $b = 'bar'
```

But when array doesn't exist, fallback is triggered with no error nor warning:

```php
$a = null;
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

Or array index is undefined, fallback is triggered with no error nor warning:

```php
$b = $undefined['foo'] ?? 'fallback';

// $b = 'fallback'
```

When array exist but key can't be found in the given array, fallback is triggered with no error nor warning:

```php
$a = [];
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

It also works with indexed arrays:

```php
$a = ['foo'];

// reminder: $a[0] = 'foo'

$b = $a[1] ?? 'fallback';

// $b = 'fallback'
```

It also works with nested arrays. If nested array key exists, then fallback isn't triggered:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['bar'] ?? 'fallback';

// $b = 'baz'
```

But when nested key can't be found in the given array, fallback is triggered with no error nor warning:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['qux'] ?? 'fallback';

// $b = 'fallback'
```

#### Null coalescing on object

You can also use null coalescing operator with object.

##### Object's attribute

If object's attribute exists, then fallback isn't triggered:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->foo ?? 'fallback';

// $b = 'bar'
```

But when object's attribute can't be found, fallback is triggered with no error nor warning:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->baz ?? 'fallback';

// $b = 'fallback'
```

##### Object's method

You can also use the null coalescing operator on call to an object's method. If the given method exists, then fallback isn't triggered:

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

But when object's method returns null, fallback is triggered with no error nor warning:

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

If object's method can't be found, null coalescing won't work and you'll get an error:

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

##### Chained method

When using chained methods on object and an intermediary element can't be found, null coalescing won't work and you'll get an error:

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

#### Null Coalescing Assignment operator

![php-version-74](https://shields.io/badge/php->=7.4-blue)

You can set a default value to a property when it is null:

```php
$a = null;
$a = $a ?? 'foo';
// $a = 'foo'
```

Since PHP 7.4, you can use the null coalescing assignment operator to do the same:

```php
$a = null;
$a ??= 'foo';
// $a = 'foo'
```

### Nullsafe operator

![php-version-80](https://shields.io/badge/php->=8.0-blue)

When trying to read a property or calling a method on null, you'll get a warning and an error:

```php
$a = null;
$b = $a->foo; // PHP Warning:  Attempt to read property "foo" on null
// $b = null

$c = $a->foo(); // PHP Error:  Call to a member function foo() on null
```

With the nullsafe operator, you can do both without warning nor error:

```php
$a = null;
$b = $a?->foo;
// $b = null
$c = $a?->foo();
// $c = null
```

You can chain multiple nullsafe operators:

```php
$a = null;
$b = $a?->foo?->bar;
// $b = null
$c = $a?->foo()?->bar();
// $c = null
```

An expression is short-circuited from the first null-safe operator that encounters null:

```php
$a = null;
$b = $a?->foo->bar->baz();
// $b = null
```

Nullsafe operator has no effect if the target is not null:

```php
$a = 'foo';
$b = $a?->bar; // PHP Warning:  Attempt to read property "bar" on string
// $b = null
$c = $a?->baz(); // PHP Error:  Call to a member function baz() on string
```

Nullsafe operator can't handle arrays properly but still can have some effect:

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

You cannot use the nullsafe operator to write, it is read only:

```php
$a = null;
$a?->foo = 'bar'; // PHP Fatal error:  Can't use nullsafe operator in write context
```

### Spread operator

#### Variadic parameter

![php-version-56](https://shields.io/badge/php->=5.6-blue)

Since PHP 5.6 (~ aug 2014), you can add a variadic parameter to any function that let you use an argument lists with variable-length:

```php
function countParameters(string $param, string ...$options) : int
{

    foreach ($options as $option) {
        // you can iterate on $options
    }
 
    return 1 + count($options);
}

countParameters('foo'); // 1
countParameters('foo', 'bar'); // 2
countParameters('foo', 'bar', 'baz'); // 3
```

Variadic parameter should always be the last parameter declared:

```php
function countParameters(string ...$options, string $param)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

You can have only one variadic parameter:

```php
function countParameters(string ...$options, string ...$moreOptions)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

It can't have a default value:

```php
function countParameters(string $param, string ...$options = [])
{
   // some code
}
// PHP Parse error: Variadic parameter cannot have a default value
```

When not typed, it accepts any value:

```php
function countParameters(string $param, ...$options) : int
{
    return 1 + count($options);
}

countParameters('foo', null, [], true); // 4
```

When typed, you have to use properly typed values:

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

#### Argument unpacking

![php-version-56](https://shields.io/badge/php->=5.6-blue)

Arrays and traversable objects can be unpacked into argument lists when calling functions by using the spread operator:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3];
$r = add(1, ...$array);

// $r = 6
```

The given array can have more elements than needed:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3, 4, 5];
$r = add(1, ...$array);

// $r = 6
```

The given array can't have lesser elements than needed:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array); // TypeError: Too few arguments to function add(), 2 passed
```

Except when some function arguments have a default value:

```php
function add(int $a, int $b, int $c = 0) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array);
// $r = 3
```

If an argument is typed and the passed value does not match the given type, you'll get an error:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = ['foo', 'bar'];
$r = add(1, ...$array); // TypeError: add(): Argument #2 ($b) must be of type int, string given
```

It is possible to use an associative array, but keys should match arguments names

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

Order of the elements in the associative array doesn't matter:

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

If a key doesn't match an argument's name, you'll get an error:

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

#### Array unpacking

##### Indexed array

![php-version-74](https://shields.io/badge/php->=7.4-blue)

When you want to merge multiple arrays, you generally use `array_merge`:

```php
$array1 = ['baz'];
$array2 = ['foo', 'bar'];

$array3 = array_merge($array1,$array2);
// $array3 = ['baz', 'foo', 'bar']
```

But since PHP 7.4 (~ nov 2019), you can unpack indexed arrays, with spread operator:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1];
// $array2 = ['baz', 'foo', 'bar']
```

Elements will be merged in the order they are passed:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1, "qux"];
// $array2 = ['baz', 'foo', 'bar', "qux"]
```

It doesn't do any deduplication:

```php
$array1 = ['foo', 'bar'];
$array2 = ['foo', ...$array1];
// $array2 = ['foo', 'foo', 'bar']
```

You can unpack multiple arrays at once:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz'];
$array3 = [ ...$array1, ...$array2];
// $array3 = ['foo', 'bar', 'baz']
```

You can unpack the same array multiple times:

```php
$array1 = ['foo', 'bar'];
$array2 = [ ...$array1, ...$array1];
// $array2 = ['foo', 'bar', 'foo', 'bar']
```

You can unpack an empty array with no error nor warning:

```php
$array1 = [];
$array2 = ['foo', ...$array1];
// $array2 = ['foo']
```

You can unpack an array that has not been previously stored in a property:

```php
$array1 = [...['foo', 'bar'], 'baz'];
// $array1 = ['foo', 'bar', 'baz']
```

Unpacking only works with arrays (or objects inplementing Traversable interface). If you try to unpack any other value, such as null, you'll get an error: 

```php
$array1 = null;
$array2 = ['foo', ...$array1]; // PHP Error:  Only arrays and Traversables can be unpacked
```

You can unpack the result of a function/method:

```php
function getArray() : array {
  return ['foo', 'bar'];
}

$array = [...getArray(), 'baz']; 
// $array = ['foo', 'bar', 'baz']
```

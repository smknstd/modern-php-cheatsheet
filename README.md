# Modern Php Cheatsheet

![Modern Php Cheatsheet](https://i.imgur.com/3jR7w62.png)


> If you like this content, you can ping me or follow me on Twitter :+1:

[![Tweet for help](https://img.shields.io/twitter/follow/smknstd?label=Tweet%20%40smknstd&style=social)](https://twitter.com/smknstd/)

## Introduction

### Motivation

This document is a cheatsheet for Php you will frequently encounter in modern projects and most contemporary sample code.

This guide is not intended to teach you Php from the ground up, but to help developers with basic knowledge who may struggle to get familiar with modern codebases (or let's say to learn Laravel or Symfony for instance) because of the new Php concepts and features introduced over the years.

> **Note:** Concepts introduced here are based on the most recent version of php available ([Php8](https://www.php.net/releases/8.0/en.php) at the time of the initial writing) 

### Complementary Resources

When you struggle to understand a notion, I suggest you look for answers on the following resources:
- [Stitcher's blog](https://stitcher.io/blog)
- [StackOverflow](https://stackoverflow.com/questions/tagged/php)

## Table of Contents

- [Modern Php cheatsheet](#modern-php-cheatsheet)
    * [Introduction](#introduction)
        + [Motivation](#motivation)
        + [Complementary resources](#complementary-resources)
    * [Table of contents](#table-of-contents)
    * [Notions](#notions)
        + [Function default parameter value](#function-default-parameter-value)
        + [Destructuring arrays](#destructuring-arrays)
        + [Null Coalescing](#null-coalescing)
        + [Spread operator](#spread-operator)

## Notions

### Function default parameter value

You can set default value to your function parameters:

```php
function myFunction(string $param = 'foo') {
    return $param;
}
$a = myFunction();
// $a = 'foo'

$b = myFunction('bar');
// $a = 'bar'
```

But if you send null or an undefined property, default value won't be used:

```php
function myFunction(string $param = 'foo') {
    return $param;
}
$a = myFunction(null);
// $a = null

$b = myFunction($undefined);
// $a = null
```

### Destructuring arrays

You can destructure arrays to pull out several elements into separate variables.

#### Indexed array

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

Or the shorthand syntax:

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

Or the shorthand syntax:

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

But since php 7.1.0 (~ dec 2016), you can destruct it with another syntax based on keys:

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

Since php 7.0 (~ dec 2015), you can use the null coalescing operator to provide a fallback when a property is null with no error nor warning:

```php
$a = null;
$b = $a ?? 'fallback';

// $b = 'fallback'
```

It also works when property is undefined:

```php
$a = $undefined ?? 'fallback';

// $a = 'fallback'
```

Every other value of the property won't trigger the fallback:

```php
'' ?? 'fallback'; // ''
0 ?? 'fallback'; // 0
false ?? 'fallback'; // false
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

Or array property is undefined, fallback is triggered with no error nor warning:

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
    public function bar() {
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
    public function bar() {
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
    public function bar() {
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
    public function bar() {
        return (object)[];
    }
}

$a = new Foo();
$b = $a->bar()->baz() ?? 'fallback'; // PHP Error:  Call to undefined method baz()
```

### Spread operator

#### Variadic parameter

Since php 5.6 (~ aug 2014), you can add a variadic parameter to any function that let you use an argument lists with variable-length:

```php
function countParameters(string $param, string ...$options) : int {

    foreach ($options as $option) {
        // you can iterate on $options
    }
 
    return 1 + count($options);
}

countArguments('foo'); // 1
countArguments('foo', 'bar'); // 2
countArguments('foo', 'bar', 'baz'); // 3
```

Variadic parameter should always be the last parameter declared:

```php
function countParameters(string ...$options, string $param) { ... }
// PHP Fatal error: Only the last parameter can be variadic
```

You can have only one variadic parameter:

```php
function countParameters(string ...$options, string ...$moreOptions) { ... }
// PHP Fatal error: Only the last parameter can be variadic
```

It can't have a default value:

```php
function countParameters(string $param, string ...$options = []) { ... }
// PHP Parse error: Variadic parameter cannot have a default value
```

When not typed, it accepts any value:

```php
function countParameters(string $param, ...$options) : int {
    return 1 + count($options);
}

countArguments('foo', null, [], true); // 4
```

When typed, you have to use properly typed values:

```php
function countParameters(string $param, string ...$options) : int {
    return 1 + count($options);
}

countArguments('foo', null);
// TypeError: countArguments(): Argument #2 must be of type string, null given

countArguments('foo', []);
// TypeError: countArguments(): Argument #2 must be of type string, array given
```

#### Argument unpacking

Since php 5.6 (~ aug 2014), arrays and traversable objects can be unpacked into argument lists when calling functions by using the spread operator:

```php
function add(int $a, int $b, int $c) : int {
    return $a + $b + $c;
}
$array = [2, 3];
$r = add(1, ...$array);

// $r = 6
```

The given array can have more elements than needed:

```php
function add(int $a, int $b, int $c) : int {
    return $a + $b + $c;
}
$array = [2, 3, 4, 5];
$r = add(1, ...$array);

// $r = 6
```

The given array can't have lesser elements than needed:

```php
function add(int $a, int $b, int $c) : int {
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array); // TypeError: Too few arguments to function add(), 2 passed
```

Except when some function arguments have a default value:

```php
function add(int $a, int $b, int $c = 0) : int {
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array);
// $r = 3
```

If an argument is typed and the passed value does not match the given type, you'll get an error:

```php
function add(int $a, int $b, int $c) : int {
    return $a + $b + $c;
}
$array = ['foo', 'bar'];
$r = add(1, ...$array); // TypeError: add(): Argument #2 ($b) must be of type int, string given
```

It is possible to use an associative array, but keys should match arguments names

```php
function add(int $a, int $b, int $c) : int {
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
function add(int $a, int $b, int $c) : int {
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
function add(int $a, int $b, int $c) : int {
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

When you want to merge multiple arrays, you generally use `array_merge`:

```php
$array1 = ['baz'];
$array2 = ['foo', 'bar'];

$array3 = array_merge($array1,$array2);
// $array3 = ['baz', 'foo', 'bar']
```

But since php 7.4 (~ nov 2019), you can unpack indexed arrays, with spread operator:

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

If you try to unpack an array valued with null, you'll get an error: 

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

##### Associative array

Since php 8.1 (~ nov 2021), you can unpack associative array (string-keyed) ... //@todo

# Destructuring array

You can destructure arrays to pull out several elements into separate variables.


## Indexed array

Considering an indexed array like :

```php
$array = ['foo', 'bar', 'baz'];
```

Using the list syntax:

```php
list($a, $b, $c) = $array;
```

Or the shorthand syntax:

```php
[$a, $b, $c] = $array;
```

You can skip elements with both syntax:

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
```


## Associative array

Considering an associative array like :

```php
$array = [
    'foo' => 1,
    'bar' => 2,
    'baz' => 3,
];
```

Previous list syntax won't work with an associative array, and you'll get a warning:

```php
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...
```

You can destruct

```php
list('foo' => $a, 'bar' => $b, 'baz' => $c) = $array;

// $a = 1
// $b = 2
// $c = 3
```

Or the shorthand syntax:

```php
['foo' => $a, 'bar' => $b, 'baz' => $c] = $array;

// $a = 1
// $b = 2
// $c = 3
```

You can also destruct only a portion of the array. The order doesn't matter.

```php
['baz' => $c, 'foo' => $a] = $array;

// $a = 1
// $c = 3
```

When you try to destruct a key that doesn't exist in the given array, you'll get a warning:

```php
list('moe' => $d) = $array;

// PHP Warning:  Undefined array key "moe"
```

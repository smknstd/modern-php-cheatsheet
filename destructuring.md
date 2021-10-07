# Destructuring array

You can destructure arrays to pull out several elements into separate variables.


## Indexed array

Considering an indexed array like :

```
$array = ["eeny", "meeny", "miny"];
```

Using the list syntax:
```
list($a, $b, $c) = $array;
```

Or the shorthand syntax:
```
[$a, $b, $c] = $array;
```

You can skip elements with both syntax:

```
list(, , $c) = $array;

// $c = "miny"
```
Or the shorthand syntax:

```
[, , $c] = $array;

// $c = "miny"
```


## Associative array

Considering an associative array like :

```
$array = [
    'eeny' => 1,
    'meeny' => 2,
    'miny' => 3,
];
```

Previous list syntax won't work with an associative array, and you'll get a warning:

```
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...
```

But you can destruct it with another syntax based on keys:

```
list('eeny' => $a, 'meeny' => $b, miny' => $c) = $array;

// $a = 1
// $b = 2
// $c = 3
```
Or the shorthand syntax:

```
['eeny' => $a, 'meeny' => $b, miny' => $c] = $array;

// $a = 1
// $b = 2
// $c = 3
```

You can also destruct only a portion of the array (The order doesn't matter):

```
['miny' => $c, 'eeny' => $a] = $array;

// $a = 1
// $c = 3
```

When you try to destruct a key that doesn't exist in the given array, you'll get a warning:

```
list('moe' => $d) = $array; // PHP Warning:  Undefined array key "moe"
```

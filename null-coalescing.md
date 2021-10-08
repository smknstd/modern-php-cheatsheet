# Null Coalescing 

Use the null coalescing operator to provide a fallback when a property is null with no error nor warning:

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

## Null coalescing on array

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

## Null coalescing on object

You can also use null coalescing operator with object.

### Object's attribute

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


### Object's method

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
        return null;
    }
}

$a = new Foo();
$b = $a->baz() ?? 'fallback';

// PHP Error:  Call to undefined method baz()
```

### Chained method

When using chained methods on object and an intermediary element can't be found, null coalescing won't work and you'll get an error:

```php
class Foo
{
    public function bar() {
        return (object)[];
    }
}

$a = new Foo();
$b = $a->bar()->baz() ?? 'fallback';

// PHP Error:  Call to undefined method baz()
```

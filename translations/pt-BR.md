# Moderno PHP Cheatsheet

![Moderno PHP Cheatsheet](https://i.imgur.com/2STEtgG.png)


> Se você gostar desse conteudo, você pode me chamar ou me seguir twitter :+1:

[![Tweet for help](https://img.shields.io/twitter/follow/smknstd?label=Tweet%20%40smknstd&style=social)](https://twitter.com/smknstd/)

Traduzido por: [Daniel Lemes](https://github.com/lemesdaniel)
> **Nota da tradução** Optei por não traduzir alguns termos por serem extremanento técnicos e poderia prejudicar em uma futura pesquisa do leitor, no entanto sinta-se a vontade para submeter uma PR se achar algum termo melhor e que seja de fácil entendimento

## Introdução

### Motivação

Este documento é um cheatsheet para PHP você encontrará essas dicas em projetos e exemplos de códigos modernos.

Esse guia não tem intenção de ensinar PHP desde do início, mas ajudar denvolvedor com conhecimento básicos mas que tem dificuldades com projetos/frameworks modernos (como Laravel ou Symfony) devidos aos novos conceitos e características introduzidas ao PHP nos últimos anos.

> **Nota:** Os conceitos aqui introduzidos são baseados na versão mais recente disponível do PHP ([PHP 8.0](https://www.php.net/releases/8.0/en.php) desde da última atualização)

### Recursos complementares

Quando tiver dificuldade em entender algum conceito, eu sugiro que você busque respostas nos seguintes sites:
- [Stitcher's blog](https://stitcher.io/blog)
- [php.Watch](https://php.watch/versions)
- [PHP The Right Way](https://phptherightway.com/)
- [StackOverflow](https://stackoverflow.com/questions/tagged/php)

## Sumário

- [Moderno PHP cheatsheet](#moderno-php-cheatsheet)
    * [Introdução](#introdução)
        + [Motivação](#motivação)
        + [Recursos complementares](#recursos-complementares)
    * [Sumário](#sumario)
    * [Notions](#notions)
        + [Valor padrão para um parâmetro de uma função](#valor-padrão-para-um-parâmetro-de-uma-função)
        + [Declaração de tipo](#declaração-de-tipo)
        + [Destructuring arrays](#destructuring-arrays)
        + [Null Coalescing](#null-coalescing)
        + [Nullsafe operator](#nullsafe-operator)
        + [Spread operator](#spread-operator)

## Notions

### Valor padrão para o parâmetro de uma função

Você pode definir um valor padrão para o parâmetro de uma função:

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
Mas se você enviar um valor null ou undefined, o valor padrão será ignorado:

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

### Declaração de tipo

![php-version-70](https://shields.io/badge/php->=7.0-blue)

Com declaração de tipo você pode especificar o tipo de dado que uma propriedade irá receber e força-lo em rempo de execução. Isso funciona tanto com scalar types (int, string, bool, and float) como também array, iterable, object, stdClass, etc.

Você pode definir o tipo de um parâmetro de uma função:

```php
function myFunction(int $param)
{
    return $param;
}
$a = myFunction(10);
// $a = 10
$b = myFunction('foo'); // TypeError: myFunction(): Argument #1 ($param) must be of type int, string given
```

Você também pode definir o tipo de retorno de uma função:

```php
function myFunction() : int
{
    return 'foo';
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type int, string returned
```

Quando uma função não deve devolver nada, você pode definir o tipo de retorno como void:

```php
function myFunction() : void
{
    return 'foo';
}
// PHP Fatal error:  A void function must not return a value
```

Nesse caso tamnbém não se pode devolver nulo:

```php
function myFunction() : void
{
    return null;
}
// PHP Fatal error:  A void function must not return a value
```

Entretanto, o uso de return para sair da função é válido e não irá disparar em um erro:

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

Você pode definir o tipo de retorno de uma propriedade de uma classe:

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

Você pode usar uma "união de tipo" para aceitar tipos diferentes, ao invés de um único:

```php
function myFunction(string|int|array $param) : string|int|array
{
    return $param;
}
```

Isso também funciona com atributos de classes:

```php
Class Foo()
{
    public string|int|array $bar;
}
```

#### Nullable type

![php-version-71](https://shields.io/badge/php->=7.1-blue)

Quando um parâmetro não tem tipo, pode-se aceitar nulo:

```php
function myFunction($param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

Mas quando um parâmetro é tipado, ao tentar inserir um valor nulo teremos um erro:

```php
function myFunction(string $param)
{
    return $param;
}
$a = myFunction(null); // TypeError: myFunction(): Argument #1 ($param) must be of type string, null given
```

Se a função tem um tipo de retorno, também não será aceito um retorno nulo 
```php
function myFunction() : string
{
    return null;
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type string, null returned
```

É possível fazer uma declaração onde é permitido receber null

```php
function myFunction(?string $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

ou com união de tipos (union type):

```php
function myFunction(string|null $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

Isso também funciona com retornos de função

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

Funções que retornam void não podem retornar null:

```php
function myFunction() : ?void
{
   // some code
} 
// PHP Fatal error:  Void type cannot be nullable
```

ou

```php
function myFunction() : void|null
{
   // some code
}
// PHP Fatal error:  Void type cannot be nullable
```

Pode-se definir null para uma propriedade de uma classe:

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

Pode-se desestruturar um array para extrair seus elementos em variáveis.

#### Indexed array

![php-version-40](https://shields.io/badge/php->=4.0-blue)

Considerando um array indexado :

```php
$array = ['foo', 'bar', 'baz'];
```

Pode-se desestruturar com list:

```php
list($a, $b, $c) = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

Ou desde PHP 7.1, a sintaxe curta (shorthand syntax):

```php
[$a, $b, $c] = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

Você pode pular elementos:

```php
list(, , $c) = $array;

// $c = 'baz'
```

Ou desde PHP 7.1, a sintaxe curta (shorthand syntax):

```php
[, , $c] = $array;

// $c = 'baz'
```

Quando tentamos desestruturar um indexe inexistente recebemos um aviso (warning) e um valor null é lançado nessa posição:

```php
list($a, $b, $c, $d) = $array; // PHP Warning:  Undefined array key 3

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
// $d = null;
```

#### Associative array

![php-version-71](https://shields.io/badge/php->=7.1-blue)

Em array associativos (string-keyed) :

```php
$array = [
    'foo' => 'value1',
    'bar' => 'value2',
    'baz' => 'value3',
];
```

A sintaxe utilizando lista não irá funcionar e um aviso (warning) será lançado

```php
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...

// $a = null
// $b = null
// $c = null
```

Mas desde PHP 7.1.0 (~ dec 2016), você pode desestruturar com uma sintaxe baseada nas chaves:

```php
list('foo' => $a, 'bar' => $b, 'baz' => $c) = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

Ou com a sintaxe curta (shorthand syntax):

```php
['foo' => $a, 'bar' => $b, 'baz' => $c] = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

Podemos também desestruturar apenas uma parte do array (sem que a ordem importe)

```php
['baz' => $c, 'foo' => $a] = $array;

// $a = 'value1'
// $c = 'value3'
```

Quando tentamos desestruturar uma chave que não exista no array, recebemos um aviso (warning):

```php
list('moe' => $d) = $array; // PHP Warning:  Undefined array key "moe"

// $d = null
```

### Null Coalescing

![php-version-70](https://shields.io/badge/php->=7.0-blue)

Desde de PHP 7.0 (~ dec 2015), você pode usar o operador null coalescing para atribuir um valor no caso de variáveis/propriedades nulas sem que seja emitidos avisos ou erros:

```php
$a = null;
$b = $a ?? 'fallback';

// $b = 'fallback'
```

Isso é equivalente a:

```php
$a = null;
$b = isset($a) ? $a : 'fallback';
// $b = 'fallback'
```

Isso também funciona quando a variável não for definida:

```php
$a = $undefined ?? 'fallback';

// $a = 'fallback'
```


Qualquer outro valor será aceito e não será definido em fallback:

```php
'' ?? 'fallback'; // ''
0 ?? 'fallback'; // 0
false ?? 'fallback'; // false
```

Podemos encadear multiplos null coalescing:

```php
$a = null;
$b = null;
$c = $a ?? $b ?? 'fallback';
// $c = 'fallback'
```

#### Elvis operator

![php-version-53](https://shields.io/badge/php->=5.3-blue)

Não deve ser confundido com o operador ternário (aka the elvis operator), que foi introduzido no PHP 5.3:

```php
$a = null;
$b = $a ?: 'fallback';

// $b = 'fallback'
```

A opção equivalente do operador ternário é:

```php
$a = null;
$b = $a ? $a : 'fallback';
// $b = 'fallback'
```

O resultado entre null coalescing e elvis operator podem ser similares, mas também diferente para algumas situações específicas:

```php
'' ?: 'fallback'; // 'fallback'
0 ?: 'fallback'; // 'fallback'
false ?: 'fallback'; // 'fallback'
```

#### Null coalescing on array

Se a chave de um array existe, o fallback não é chamado:

```php
$a = ['foo' => 'bar'];
$b = $a['foo'] ?? 'fallback';

// $b = 'bar'
```

Mas quando o array não existe, fallback é chamado sem emitir erros ou avisos:

```php
$a = null;
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

Ou a propriedade de um array é indefinida, fallback é chamado sem emitir erros ou avisos:

```php
$b = $undefined['foo'] ?? 'fallback';

// $b = 'fallback'
```

Quando o array existe mas a chave não, fallback é chamado sem emitir erros ou avisos:

```php
$a = [];
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```


Isso também funciona com arrays indexados:

```php
$a = ['foo'];

// reminder: $a[0] = 'foo'

$b = $a[1] ?? 'fallback';

// $b = 'fallback'
```

Isso também funciona com array multi dimensionais. Se a chave existir, fallback não é chamado:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['bar'] ?? 'fallback';

// $b = 'baz'
```

Mas em arrays multi dimensionais quando a chave não existir, fallback é chamado sem emitir erros ou avisos:

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


Você também pode usar null operador coalescing com objetos.

##### Object's attribute

If object's attribute exists, then fallback isn't triggered:
Se a propriedade do objeto existir, fallback não é chamado:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->foo ?? 'fallback';

// $b = 'bar'
```

mas se a propriedade do objeto não for encontrada, fallback é chamado sem emitir erros ou avisos:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->baz ?? 'fallback';

// $b = 'fallback'
```

##### Object's method

Você também pode usar o operador null coalescing ao chamar métodos. Se ele existir, fallback é ignorado.


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

Mas quando o método retornar null, fallback é chamado sem emitir erros ou avisos:

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

Se o método não for encontrado na classe, null coalescing não funcionará e um erro será lançado:

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

Quando usamos métodos encadeados um método intermediário retornar null, o operador null coalescing não funcionará e um erro será lançado:

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


Você pode definidir um valor padrão quando o valor for null

```php
$a = null;
$a = $a ?? 'foo';
// $a = 'foo'
```

Desde de PHP 7.4, você pode usar o operador null coalescing para fazer o mesmo:

```php
$a = null;
$a ??= 'foo';
// $a = 'foo'
```

### Operador Nullsafe

![php-version-80](https://shields.io/badge/php->=8.0-blue)

Ao tentar ler uma propriedade ou um chamar um método null, você receberá um aviso e um erro:

```php
$a = null;
$b = $a->foo; // PHP Warning:  Attempt to read property "foo" on null
// $b = null

$c = $a->foo(); // PHP Error:  Call to a member function foo() on null
```
Com o operador nullsafe, você pode fazer sem receber erros ou avisos 

```php
$a = null;
$b = $a?->foo;
// $b = null
$c = $a?->foo();
// $c = null
```

Você pode encadear multiplos operadores nullsafe:

```php
$a = null;
$b = $a?->foo?->bar;
// $b = null
$c = $a?->foo()?->bar();
// $c = null
```

Uma expressão é um short-circuited do primeiro operador null-safe encontrado:

```php
$a = null;
$b = $a?->foo->bar->baz();
// $b = null
```

Operador Nullsafe não tem efeito em valores não nulos.

```php
$a = 'foo';
$b = $a?->bar; // PHP Warning:  Attempt to read property "bar" on string
// $b = null
$c = $a?->baz(); // PHP Error:  Call to a member function baz() on string
```

Operador Nullsafe não consegue trabalhar adequadamente com arrays, mesmo assim tem algum efeito: 

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

Você não pode usar o operador nullsafe em operações de escrita, ele é apenas um recurso de leitura:

```php
$a = null;
$a?->foo = 'bar'; // PHP Fatal error:  Can't use nullsafe operator in write context
```

### Spread operator

#### Variadic parameter

![php-version-56](https://shields.io/badge/php->=5.6-blue)

Desde do PHP 5.6 (~ aug 2014), você pode adicionar uma lista de parâmetros com tamanho indefinido ou variáveis:

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

Parâmetros variáveis devem ser semre o último parâmetros declarados:

```php
function countParameters(string ...$options, string $param)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

Você pode definir um ou mais parâmetros variáveis:

```php
function countParameters(string ...$options, string ...$moreOptions)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

Você não deve ter um valor padrão:

```php
function countParameters(string $param, string ...$options = [])
{
   // some code
}
// PHP Parse error: Variadic parameter cannot have a default value
```

Quando não tipado, isso aceita qualquer valor:

```php
function countParameters(string $param, ...$options) : int
{
    return 1 + count($options);
}

countParameters('foo', null, [], true); // 4
```

Quando tipado, deve-se passar os valores de acordo com o tipo:

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

Array e objetos podem ser desempacotados em uma lista de argumentos usando spread operator:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3];
$r = add(1, ...$array);

// $r = 6
```

O array inserido pode ter mais elementos que o necessário:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2, 3, 4, 5];
$r = add(1, ...$array);

// $r = 6
```

O array passando não pode ter menos elementos que o necessário:

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array); // TypeError: Too few arguments to function add(), 2 passed
```

Exceto quando algum argumento da função tem um valor padrão:

```php
function add(int $a, int $b, int $c = 0) : int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array);
// $r = 3
```

Se um argumento é tipado e o valor inserido não é do mesmo tipo, um erro será lançado:\

```php
function add(int $a, int $b, int $c) : int
{
    return $a + $b + $c;
}
$array = ['foo', 'bar'];
$r = add(1, ...$array); // TypeError: add(): Argument #2 ($b) must be of type int, string given
```


É possível passar um array associativo, mas as chaves desses corresponder aos nomes dos argumentos:

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

A ordem dos elementos em um array associativos não importa:

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

Se a chave não corresponder a um argumento, um erro será lançado:

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

Quando você pretender combinar (unir) vários arrays, normalmente usa-se `array_merge`

```php
$array1 = ['baz'];
$array2 = ['foo', 'bar'];

$array3 = array_merge($array1,$array2);
// $array3 = ['baz', 'foo', 'bar']
```

Desde de PHP 7.4 (~ nov 2019), você pode desempacotar arrays com spread operator:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1];
// $array2 = ['baz', 'foo', 'bar']
```

Elementos são combinados na ordem em que são passados:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1, "qux"];
// $array2 = ['baz', 'foo', 'bar', "qux"]
```

Não é verificado qualquer valor duplicado:

```php
$array1 = ['foo', 'bar'];
$array2 = ['foo', ...$array1];
// $array2 = ['foo', 'foo', 'bar']
```

Você pode desempacotar vários arrays ao mesmo tempo:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz'];
$array3 = [ ...$array1, ...$array2];
// $array3 = ['foo', 'bar', 'baz']
```

Também podemos desempacotar arrays multiplas vezes:

```php
$array1 = ['foo', 'bar'];
$array2 = [ ...$array1, ...$array1];
// $array2 = ['foo', 'bar', 'foo', 'bar']
```

Se tentarmos desempacotar um array vazio não será lançado qualquer erro ou aviso:

```php
$array1 = [];
$array2 = ['foo', ...$array1];
// $array2 = ['foo']
```

Você pode desempacotar um array, que não tenha sido previamente armazenado em uma variável:

```php
$array1 = [...['foo', 'bar'], 'baz'];
// $array1 = ['foo', 'bar', 'baz']
```

Desempacotar funciona apenas com arrays e objetos. Se você tentar desempacotar quaqluer outro valor, como null, um erro será lançado:

```php
$array1 = null;
$array2 = ['foo', ...$array1]; // PHP Error:  Only arrays and Traversables can be unpacked
```

Você também pode desempacotar o resultado de uma função/método:

```php
function getArray() : array {
  return ['foo', 'bar'];
}

$array = [...getArray(), 'baz']; 
// $array = ['foo', 'bar', 'baz']
```

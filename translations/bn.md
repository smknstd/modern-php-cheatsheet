# মডার্ণ পিএইচপি চিটশীট

![মডার্ণ পিএইচপি চিটশীট](https://i.imgur.com/2STEtgG.png)


> আপনি যদি এই বিষয়বস্তু পছন্দ করেন, তবে আপনি আমার সাথে যোগাযোগ করতে পারেন বা টুইটারে আমাকে অনুসরণ করতে পারেন. :+1:

[![Tweet for help](https://img.shields.io/twitter/follow/smknstd?label=Tweet%20%40smknstd&style=social)](https://twitter.com/smknstd/)

অনুবাদ: [S. M. Shamir Imtiaz](https://github.com/Ivanshamir)
> **অনুবাদ নোট:** **কিছু শব্দ অনুবাদ করা হয়নি কারণ সেগুলি অত্যন্ত প্রযুক্তিগত এবং ভবিষ্যতের পাঠকদের জন্য শিখতে অসুবিধা হতে পারে**.

## ভূমিকা

### **মোটিভেশন**

এই ডকুমেন্টটি PHP-এর জন্য কোড সহ একটি চিটশিট যা আপনি বিভিন্ন আপডেটেড প্রজেক্টগুলোতে প্রায়শই পাবেন.

এই নির্দেশিকাটি আপনাকে প্রাথমিকভাবে PHP শেখানোর উদ্দেশ্যে নয়, বরং এমন ডেভেলপার যারা PHP এর বেসিক জানেন কিন্তু মডার্ণ কোডবেসগুলির সাথে পরিচিত হতে সমস্যায় পড়তে পারেন (বা উদাহরণ স্বরূপ লারাভেল বা সিমফনি শেখার জন্য বলা যাক) তাদের সাহায্য করার জন্য কারণ PHP রিসেন্ট বছরগুলোতে নতুন নতুন ফিচার এবং কনসেপ্ট নিয়ে আসছে.

> **বিঃদ্রঃ:** এখানে প্রবর্তিত কনসেপ্টগুলি PHP-এর সবচেয়ে সাম্প্রতিক সংস্করণের উপর ভিত্তি করে ([PHP 8.1](https://www.php.net/releases/8.1/en.php) শেষ আপডেটের সময়)

### কমপ্লিমেন্টারী রির্সোসেস

যদি আপনি কোন একটি টপিক বুঝতে সমস্যায় পড়েন, তখন আমি আপনাকে পরামর্শ দিব নিচের যেকোন একটি রিসোর্স থেকে উত্তর খুঁজে নিতে :
- [Stitcher's blog](https://stitcher.io/blog)
- [PHP.Watch](https://php.watch/versions)
- [Exploring php 8.0](https://leanpub.com/exploringphp80)
- [PHP The Right Way](https://phptherightway.com/)
- [StackOverflow](https://stackoverflow.com/questions/tagged/php)

## সুচিপত্র

- [মডার্ণ পিএইচপি চিটশিট](#মডার্ণ-পিএইচপি-চিটশিট)
    * [ভূমিকা](#ভূমিকা)
        + [মোটিভেশন](#মোটিভেশন)
        + [কমপ্লিমেন্টারী রির্সোসেস](#কমপ্লিমেন্টারী-রির্সোসেস)
    * [সুচিপত্র](#সুচিপত্র)
    * [নোশনস](#নোশনস)
        + [ফাংশন এর ডিফল্ট প্যারামিটার ভ্যালু  ](#ফাংশন-এর-ডিফল্ট-প্যারামিটার-ভ্যালু)
        + [ট্রেইলিং কমা](#ট্রেইলিং-কমা)
        + [টাইপ ডিক্লারেশন](#টাইপ-ডিক্লারেশন)
        + [ডিস্ট্রাকচারিং অ্যারেস](#ডিস্ট্রাকচারিং-অ্যারেস)
        + [নাল কোলেসিং](#নাল-কোলেসিং)
        + [নালসেফ অপারেটর](#নালসেফ-অপারেটর)
        + [স্প্রেড অপারেটর](#স্প্রেড-অপারেটর)
        + [নেমড আর্গুমেন্টস](#নেমড-আর্গুমেন্টস)
        + [শর্ট ক্লোসার্স](#শর্ট-ক্লোসার্স)
        + [ম্যাচ এক্সপ্রেশন](#ম্যাচ-এক্সপ্রেশন)
        + [স্ট্রিংএবল ইন্টারফেস](#স্ট্রিংএবল-ইন্টারফেস)

## নোশনস

### ফাংশন এর ডিফল্ট প্যারামিটার ভ্যালু

আপনি আপনার ফাংশন প্যারামিটারে ডিফল্ট মান সেট করতে পারেন:

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

কিন্তু যদি আপনি নাল বা আনডিফাইন্ড প্রোপার্টি পাঠান, তাহলে ডিফল্ট ভ্যালু ব্যবহার হবে না:

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

### ট্রেইলিং কমা

ট্রেইলিং কমা, যা একটি আনত কমা নামেও পরিচিত, একটি কমা প্রতীক যা আইটেমগুলির তালিকার শেষ আইটেমের পরে টাইপ করা হয়। মাল্টিলাইনের সাথে ব্যবহার করার প্রধান সুবিধাগুলির মধ্যে একটি হল [ডিফ আউটপুট ক্লিনার](https://medium.com/@nikgraf/why-you-should-enforce-dangling-commas-for-multiline-statements-d034c98e36f8).

#### অ্যারে

আপনি অ্যারেতে ট্রেলিং কমা ব্যবহার করতে পারেন :

```php
$array = [
    'foo',
    'bar',
];
```
#### গ্রুপড ইউজ স্টেটমে

![php-version-72](https://shields.io/badge/php->=7.2-blue)

PHP 7.2 ভার্সন থেকে, আপনি গ্রুপড ইউজ স্টেটমেন্টে ট্রেলিং কমা ব্যবহার করতে পারেন:

```php
use Symfony\Component\HttpKernel\{
    Controller\ControllerResolverInterface,
    Exception\NotFoundHttpException,
    Event\PostResponseEvent,
};
```
#### ফাংশন এবং মেথড কল

![php-version-73](https://shields.io/badge/php->=7.3-blue)

PHP 7.3 থেকে, আপনি ফাংশন কল করার সময় ট্রেলিং কমা ব্যবহার করতে পারেন:

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

এবং মেথড কলের সময়:

```php
$f = new Foo();
$f->myMethod(
    'baz',
    'qux',
);
```
#### ফাংশন প্যারামিটারস

![php-version-80](https://shields.io/badge/php->=8.0-blue)

PHP 8.0 থেকে, আপনি ফাংশন প্যারামিটার ডিক্লেয়ার করার সময় ট্রেলিং কমা ব্যবহার করতে পারেন :

```php
function myFunction(
    $foo,
    $bar,
)
{
    return true;
}
```
#### ক্লোজারস ইউজ স্টেটমেন্ট

![php-version-80](https://shields.io/badge/php->=8.0-blue)

PHP 8.0 থেকে, আপনি ক্লোজারের ইউজ স্টেটমেন্টের সাথে ট্রেলিং কমা ব্যবহার করতে পারেন:

```php
function() use (
    $foo,
    $bar,
)
{
    return true;
}
```

### টাইপ ডিক্লারেশন

![php-version-70](https://shields.io/badge/php->=7.0-blue)

টাইপ ডিক্লারেশন এর সাথে আপনি প্রোপার্টি এর জন্য প্রত্যাশিত ডেটা টাইপ নির্দিষ্ট করতে পারেন যা রানটাইমে প্রয়োগ করা হবে। এটি অনেক ধরনের টাইপ সাপোর্ট করে যেমন স্কেলার টাইপ (int, স্ট্রিং, বুল এবং ফ্লোট) তার সাথে অ্যারে, ইটারেবল, অবজেক্ট, stdClass ইত্যাদিও

আপনি একটি ফাংশনের প্যারামিটারে একটি টাইপ সেট করতে পারেন:

```php
function myFunction(int $param)
{
    return $param;
}
$a = myFunction(10);
// $a = 10
$b = myFunction('foo'); // TypeError: myFunction(): Argument #1 ($param) must be of type int, string given
```

আপনি ফাংশনে রিটার্ন টাইপ সেট করতে পারেন:

```php
function myFunction(): int
{
    return 'foo';
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type int, string returned
```

যখন একটি ফাংশন কিছু রিটার্ণ করবেনা, তখন আপনি "void" টাইপ ব্যবহার করতে পারেন:

```php
function myFunction(): void
{
    return 'foo';
}
// PHP Fatal error:  A void function must not return a value
```

তবে এক্ষেত্রে আপনি নাল রিটার্ণ করতে পারবেননা:

```php
function myFunction(): void
{
    return null;
}
// PHP Fatal error:  A void function must not return a value
```

যাইহোক, ফাংশন থেকে এক্সিট করার জন্য রিটার্ন ব্যবহার করা গ্রহণযোগ্য:

```php
function myFunction(): void
{
    return;
}
$a = myFunction();
// $a = null
```

#### ক্লাস প্রোপার্টি

![php-version-74](https://shields.io/badge/php->=7.4-blue)

আপনি একটি ক্লাস প্রোপার্টিতে একটি টাইপ সেট করতে পারেন:

```php
Class Foo
{
    public int $bar;
}
$f = new Foo();
$f->bar = 'baz'; // TypeError: Cannot assign string to property Foo::$bar of type int
```

#### ইউনিয়ন টাইপ

![php-version-80](https://shields.io/badge/php->=8.0-blue)

পিএইচপি 8.0 থেকে, আপনি "ইউনিয়ন টাইপ" ব্যবহার করতে পারেন যা একটি নয় বরং একাধিক ভিন্ন ধরণের টাইপ গ্রহণ করে:

```php
function myFunction(string|int|array $param): string|int|array
{
    return $param;
}
```

এটি ক্লাস প্রোপার্টির সাথেও কাজ করে:

```php
Class Foo
{
    public string|int|array $bar;
}
```

#### ইন্টারসেকশন টাইপ

![php-version-81](https://shields.io/badge/php->=8.1-blue)

PHP 8.1 থেকে, আপনি "ইন্টারসেকশন টাইপ" (এটি "পিউর" নামেও পরিচিত) ব্যবহার করতে পারেন যেখানে প্রদত্ত ভ্যালু প্রতিটি টাইপের সাথে সম্পর্কিত। উদাহরণস্বরূপ এই প্যারামটিকে *Stringable* এবং *Countable* ইন্টারফেস উভয়ই বাস্তবায়ন করতে হবে:

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

এটি ক্লাস প্রোপার্টির সাথেও কাজ করে:

```php
Class Foo
{
    public Stringable&Countable $bar;
}
```

ইন্টারসেকশন টাইপ শুধুমাত্র ক্লাস এবং ইন্টারফেস সমর্থন করে. স্কেলার প্রকার (স্ট্রিং, int, অ্যারে, নাল, মিক্সড, ইত্যাদি) অনুমোদিত নয়:

```php
function myFunction(string&Countable $param)
{
    return $param;
}
// PHP Fatal error:  Type string cannot be part of an intersection type
```

##### এক্সটারনাল রিসোর্স

- [Intersection types on PHP.Watch](https://php.watch/versions/8.1/intersection-types)

#### নালেবল টাইপ

![php-version-71](https://shields.io/badge/php->=7.1-blue)

যখন একটি প্যারামিটারের কোন টাইপ নেই, তখন এটি নাল ভ্যালু গ্রহণ করতে পারে:

```php
function myFunction($param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

কিন্তু যখনি প্যারামিটারের টাইপ থাকবে, তখন এটি আর নাল ভ্যালু গ্রহণ করবে না এবং আপনি একটি এরর পাবেন:

```php
function myFunction(string $param)
{
    return $param;
}
$a = myFunction(null); // TypeError: myFunction(): Argument #1 ($param) must be of type string, null given
```

যদি ফাংশনের রিটার্ন টাইপ থাকে তবে এটি নাল ভ্যালুও গ্রহণ করবে না:

```php
function myFunction(): string
{
    return null;
}
$a = myFunction(); // TypeError: myFunction(): Return value must be of type string, null returned
```

আপনি একটি টাইপ ডিক্লেয়ার করার সময় সেটিকে স্পষ্টভাবে নালেবল করতে পারেন:

```php
function myFunction(?string $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

অথবা একটি ইউনিয়ন টাইপ সঙ্গে:

```php
function myFunction(string|null $param)
{
    return $param;
}
$a = myFunction(null);
// $a = null
```

এটি রিটার্ন টাইপের সাথেও কাজ করে:

```php
function myFunction(?string $param): ?string
{
    return $param;
}
// or
function myFunction(string|null $param): string|null
{
    return $param;
}
```

কিন্তু ভয়েড নালেবল হতে পারে না:

```php
function myFunction(): ?void
{
   // some code
} 
// PHP Fatal error:  Void type cannot be nullable
```

অথবা

```php
function myFunction(): void|null
{
   // some code
}
// PHP Fatal error:  Void type cannot be nullable
```

আপনি ক্লাস প্রোপার্টিতে নালেবল টাইপ সেট করতে পারেন :

```php
Class Foo
{
    public int|null $bar;
}
$f = new Foo();
$f->bar = null;
$a = $f->bar;
// $a = null
```

### ডিস্ট্রাকচারিং অ্যারেস

আপনি আলাদা ভেরিয়েবলে আলাদা এলিমেন্ট রাখতে অ্যারে ডিস্ট্রাকচার করতে পারেন.

#### ইন্ডেক্সড অ্যারে

![php-version-40](https://shields.io/badge/php->=4.0-blue)

এই ইনডেক্সড অ্যারে টিকে বিবেচনা করুন  :

```php
$array = ['foo', 'bar', 'baz'];
```

আপনি লিস্ট সিনট্যাক্স ব্যবহার করে এটিকে ডিস্ট্রাকচার করতে পারেন:

```php
list($a, $b, $c) = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

অথবা পিএইচপি 7.1 থেকে, শর্টহ্যান্ড সিনট্যাক্স:

```php
[$a, $b, $c] = $array;

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
```

আপনি ইলেমেণ্টস স্কিপ করতে পারেন:

```php
list(, , $c) = $array;

// $c = 'baz'
```

অথবা পিএইচপি 7.1 থেকে, শর্টহ্যান্ড সিনট্যাক্স:

```php
[, , $c] = $array;

// $c = 'baz'
```

আপনি যখন প্রদত্ত অ্যারেতে বিদ্যমান নেই এমন একটি ইনডেক্স ডেস্ট্রাক্ট করার চেষ্টা করেন, আপনি একটি ওয়ার্নিং পাবেন:

```php
list($a, $b, $c, $d) = $array; // PHP Warning:  Undefined array key 3

// $a = 'foo'
// $b = 'bar'
// $c = 'baz'
// $d = null;
```

#### অ্যাসোসিয়েটিভ  অ্যারে

![php-version-71](https://shields.io/badge/php->=7.1-blue)

একটি অ্যাসোসিয়েটিভ অ্যারে বিবেচনা করুন (স্ট্রিং-কীড এর মত) :

```php
$array = [
    'foo' => 'value1',
    'bar' => 'value2',
    'baz' => 'value3',
];
```

পূর্ববর্তী লিস্ট সিনট্যাক্স অ্যাসোসিয়েটিভ অ্যারের সাথে কাজ করবে না এবং এক্ষেত্রে আপনি একটি ওয়ার্নিং পাবেন:

```php
list($a, $b, $c) = $array; // PHP Warning:  Undefined array key 0 ...

// $a = null
// $b = null
// $c = null
```

কিন্তু PHP 7.1 থেকে, আপনি কীগুলির উপর ভিত্তি করে অন্য সিনট্যাক্স দিয়ে এটি ডেস্ট্রাক্ট করতে পারেন:

```php
list('foo' => $a, 'bar' => $b, 'baz' => $c) = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

অথবা শর্টহ্যান্ড সিনট্যাক্স:

```php
['foo' => $a, 'bar' => $b, 'baz' => $c] = $array;

// $a = 'value1'
// $b = 'value2'
// $c = 'value3'
```

আপনি অ্যারের শুধুমাত্র একটি অংশ ডেস্ট্রাক্ট করতে পারেন (এখানে অর্ডার কোন ব্যাপার না):

```php
['baz' => $c, 'foo' => $a] = $array;

// $a = 'value1'
// $c = 'value3'
```

আপনি প্রদত্ত অ্যারেতে বিদ্যমান নেই এমন একটি কী ডেস্ট্রাক্ট করার চেষ্টা করলে, আপনি একটি ওয়ার্নিং পাবেন:

```php
list('moe' => $d) = $array; // PHP Warning:  Undefined array key "moe"

// $d = null
```

### নাল কোলেসিং

![php-version-70](https://shields.io/badge/php->=7.0-blue)

PHP 7.0 থেকে, আপনি একটি ফলব্যাক প্রোভাইড করতে নাল কোলেসিং অপারেটর ব্যবহার করতে পারেন যখন কোনো প্রোপার্টি কোনো এরর বা ওয়ার্নিং ছাড়াই নাল থাকে:

```php
$a = null;
$b = $a ?? 'fallback';

// $b = 'fallback'
```

এর সমতুল্য:

```php
$a = null;
$b = isset($a) ? $a : 'fallback';
// $b = 'fallback'
```

প্রোপার্টি আনডিফাইন্ড হলেও এটি কাজ করে:

```php
$a = $undefined ?? 'fallback';

// $a = 'fallback'
```

প্রোপার্টির অন্য ভ্যালুগুলো ফলব্যাক ট্রিগার করবে না:

```php
'' ?? 'fallback'; // ''
0 ?? 'fallback'; // 0
false ?? 'fallback'; // false
```

আপনি একাধিকবার নাল কোলেসিং চেইন করতে পারেন:

```php
$a = null;
$b = null;
$c = $a ?? $b ?? 'fallback';
// $c = 'fallback'
```

#### এলভিস অপারেটর

![php-version-53](https://shields.io/badge/php->=5.3-blue)

এটিকে শর্টহ্যান্ড টারনারি অপারেটর (ওরফে এলভিস অপারেটর) এর সাথে বিভ্রান্ত হওয়া উচিত নয়, যা পিএইচপি 5.3 এ এসেছিল:

```php
$a = null;
$b = $a ?: 'fallback';

// $b = 'fallback'
```

শর্টহ্যান্ড টারনারি অপারেটর এর সমতুল্য:

```php
$a = null;
$b = $a ? $a : 'fallback';
// $b = 'fallback'
```

নাল কোলেসিং এবং এলভিস অপারেটরের মধ্যে ফলাফল একই রকম হতে পারে তবে কিছু নির্দিষ্ট মানের জন্য ভিন্ন:

```php
'' ?: 'fallback'; // 'fallback'
0 ?: 'fallback'; // 'fallback'
false ?: 'fallback'; // 'fallback'
```

#### অ্যারেতে নাল কোলেসিং

যদি অ্যারে কী বিদ্যমান থাকে, তাহলে ফলব্যাক ট্রিগার করা হবে না:

```php
$a = ['foo' => 'bar'];
$b = $a['foo'] ?? 'fallback';

// $b = 'bar'
```

কিন্তু যখন অ্যারে বিদ্যমান থাকে না, তখন কোনো এরর বা ওয়ার্নিং ছাড়াই ফলব্যাক ট্রিগার হয়:

```php
$a = null;
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

অথবা অ্যারে প্রোপার্ট আনডিফাইন্ড হলে, ফলব্যাক কোন ত্রুটি বা সতর্কতা ছাড়াই ট্রিগার হবে:

```php
$b = $undefined['foo'] ?? 'fallback';

// $b = 'fallback'
```

যখন অ্যারে বিদ্যমান থাকে কিন্তু প্রদত্ত অ্যারেতে কী খুঁজে পাওয়া যায় না, তখন কোনো এরর বা ওয়ার্নিং ছাড়াই ফলব্যাক ট্রিগার হয়:

```php
$a = [];
$b = $a['foo'] ?? 'fallback';

// $b = 'fallback'
```

এটি ইন্ডেক্সড অ্যারের সাথেও কাজ করে:

```php
$a = ['foo'];

// reminder: $a[0] = 'foo'

$b = $a[1] ?? 'fallback';

// $b = 'fallback'
```

এটি নেস্টেড অ্যারের সাথেও কাজ করে। নেস্টেড অ্যারেতে কী বিদ্যমান থাকলে, ফলব্যাক ট্রিগার হবেনা :

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['bar'] ?? 'fallback';

// $b = 'baz'
```

কিন্তু যখন প্রদত্ত অ্যারেতে নেস্টেড কী পাওয়া যায় না, তখন ফলব্যাক কোনো এরর বা ওয়ার্নিং ছাড়াই ট্রিগার হয়:

```php
$a = [
   'foo' => [
      'bar' => 'baz'
   ]
];
$b = $a['foo']['qux'] ?? 'fallback';

// $b = 'fallback'
```

#### অবজেক্টের উপর নাল কোলেসিং

আপনি অবজেক্টের সাথে নাল কোলেসিং অপারেটরও ব্যবহার করতে পারেন.

##### অব্জেক্ট'স অ্যাট্রিবিউট

যদি অব্জেক্ট'স অ্যাট্রিবিউট বিদ্যমান থাকে, তাহলে ফলব্যাক ট্রিগার হবেনা:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->foo ?? 'fallback';

// $b = 'bar'
```

কিন্তু যখন অবজেক্টের অ্যাট্রিবিউট খুঁজে পাওয়া যায় না, ফলব্যাক কোনো এরর বা ওয়ার্নিং ছাড়াই ট্রিগার হয়:

```php
$a = (object)[
    'foo' => 'bar'
];
$b = $a->baz ?? 'fallback';

// $b = 'fallback'
```

##### অবজেক্টের মেথড

আপনি একটি অবজেক্টের মেথডকে কল করার সময় নাল কোলেসিং অপারেটর ব্যবহার করতে পারেন। যদি প্রদত্ত মেথড বিদ্যমান থাকে, তাহলে ফলব্যাক ট্রিগার হয় না:

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

কিন্তু যখন অবজেক্টের মেথড নাল রিটার্ন করে, ফলব্যাক কোন এরর বা ওয়ার্নিং ছাড়াই ট্রিগার হয়:

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

কিন্তু যদি অবজেক্টের মেথড নাল রিটার্ন করে, তবে ফলব্যাক কোন এরর বা ওয়ার্নিং ছাড়াই ট্রিগার হবে  :

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

##### চেইনড মেথড

যখন অবজেক্টের উপর চেইনড মেথড ব্যবহার করা হয় এবং একটি মধ্যস্থতাকারী এলিমেন্ট খুঁজে পাওয়া যায় না, তখন নাল কোলেসিং কাজ করবে না এবং আপনি একটি এরর পাবেন :

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

#### নাল কোলেসিং অ্যাসাইনমেন্ট অপারেটর

![php-version-74](https://shields.io/badge/php->=7.4-blue)

আপনি একটি ডিফল্ট মান সেট করতে পারেন প্রোপার্টিতে, যখন এটি নাল হয়:

```php
$a = null;
$a = $a ?? 'foo';
// $a = 'foo'
```

পিএইচপি 7.4 থেকে, আপনি একই কাজ করতে নাল কোলেসিং অ্যাসাইনমেন্ট অপারেটর ব্যবহার করতে পারেন:

```php
$a = null;
$a ??= 'foo';
// $a = 'foo'
```

### নালসেফ অপারেটর

![php-version-80](https://shields.io/badge/php->=8.0-blue)

প্রোপার্টি রিড করার সময় বা নাল পদ্ধতিতে কল করার সময়, আপনি একটি ওয়ার্নিং এবং একটি এরর পাবেন:

```php
$a = null;
$b = $a->foo; // PHP Warning:  Attempt to read property "foo" on null
// $b = null

$c = $a->foo(); // PHP Error:  Call to a member function foo() on null
```

নালসেফ অপারেটর দিয়ে, আপনি ওয়ার্নিং বা এরর ছাড়াই উভয়ই করতে পারেন:

```php
$a = null;
$b = $a?->foo;
// $b = null
$c = $a?->foo();
// $c = null
```

আপনি একাধিক নালসেফ অপারেটর চেইন করতে পারেন:

```php
$a = null;
$b = $a?->foo?->bar;
// $b = null
$c = $a?->foo()?->bar();
// $c = null
```

একটি এক্সপ্রেশন শর্ট এক্সিকিউট হবে যদি তা প্রথম নালসেফ অপারেটরই নাল পায়:

```php
$a = null;
$b = $a?->foo->bar->baz();
// $b = null
```

টার্গেট নাল না হলে নালসেফ অপারেটরের কোন প্রভাব নেই:

```php
$a = 'foo';
$b = $a?->bar; // PHP Warning:  Attempt to read property "bar" on string
// $b = null
$c = $a?->baz(); // PHP Error:  Call to a member function baz() on string
```

নালসেফ অপারেটর অ্যারেকে সঠিকভাবে হ্যান্ডেল করতে পারে না তবে কিছু প্রভাব ফেলতে পারে:

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

আপনি রাইট করার জন্য নালসেফ অপারেটর ব্যবহার করতে পারবেন না, এটি শুধুমাত্র রিডের জন্য:

```php
$a = null;
$a?->foo = 'bar'; // PHP Fatal error:  Can't use nullsafe operator in write context
```

### স্প্রেড অপারেটর

#### ভেরিয়াডিক প্যারামিটার

![php-version-56](https://shields.io/badge/php->=5.6-blue)

PHP 5.6 (~ aug ২০১৪) থেকে, আপনি যেকোন ফাংশনে ভেরিয়াডিক প্যারামিটার যোগ করতে পারেন যাতে আপনি ভেরিয়েবল-লেংথ সহ একটি আর্গুমেন্ট লিস্ট ব্যবহার করতে পারেন :

```php
function countParameters(string $param, string ...$options): int
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

ভ্যারিয়াডিক প্যারামিটার সর্বদা শেষ প্যারামিটারে হওয়া উচিত:

```php
function countParameters(string ...$options, string $param)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

আপনি শুধুমাত্র একটি ভ্যারিয়াডিক প্যারামিটার ব্যাবহার করতে পারেন:

```php
function countParameters(string ...$options, string ...$moreOptions)
{ 
   // some code
}
// PHP Fatal error: Only the last parameter can be variadic
```

এটির ডিফল্ট মান থাকতে পারে না  :

```php
function countParameters(string $param, string ...$options = [])
{
   // some code
}
// PHP Parse error: Variadic parameter cannot have a default value
```

টাইপ বলা না হলে, এটি যে কোনো মান গ্রহণ করে:

```php
function countParameters(string $param, ...$options): int
{
    return 1 + count($options);
}

$a = countParameters('foo', null, [], true);
// $a = 4
```

টাইপ বলা হলে, আপনাকে সঠিকভাবে টাইপড ভ্যালুস ব্যবহার করতে হবে:

```php
function countParameters(string $param, string ...$options): int
{
    return 1 + count($options);
}

countParameters('foo', null);
// TypeError: countParameters(): Argument #2 must be of type string, null given

countParameters('foo', []);
// TypeError: countParameters(): Argument #2 must be of type string, array given
```

#### আর্গুমেন্ট আনপ্যাকিং

![php-version-56](https://shields.io/badge/php->=5.6-blue)

স্প্রেড অপারেটর ব্যবহার করে ফাংশন কল করার সময় অ্যারে এবং ট্রাভার্সেবল অবজেক্টগুলি আর্গুমেন্ট লিস্টে আনপ্যাক করা যেতে পারে:

```php
function add(int $a, int $b, int $c): int
{
    return $a + $b + $c;
}
$array = [2, 3];
$r = add(1, ...$array);

// $r = 6
```

প্রদত্ত অ্যারেতে প্রয়োজনের চেয়ে বেশি এলিমেন্ট থাকতে পারে:

```php
function add(int $a, int $b, int $c): int
{
    return $a + $b + $c;
}
$array = [2, 3, 4, 5];
$r = add(1, ...$array);

// $r = 6
```

প্রদত্ত অ্যারেতে প্রয়োজনের চেয়ে কম উপাদান থাকতে পারবে না:

```php
function add(int $a, int $b, int $c): int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array); // TypeError: Too few arguments to function add(), 2 passed
```

যদি না কিছু ফাংশন আর্গুমেন্টের একটি ডিফল্ট মান থাকে:

```php
function add(int $a, int $b, int $c = 0): int
{
    return $a + $b + $c;
}
$array = [2];
$r = add(1, ...$array);
// $r = 3
```

যদি একটি আর্গুমেন্ট টাইপড হয় এবং পাস করা ভ্যালু প্রদত্ত টাইপের সাথে মেলে না, তাহলে আপনি একটি এরর পাবেন:

```php
function add(int $a, int $b, int $c): int
{
    return $a + $b + $c;
}
$array = ['foo', 'bar'];
$r = add(1, ...$array); // TypeError: add(): Argument #2 ($b) must be of type int, string given
```

পিএইচপি ৮.০ থেকে, এটি একটি অ্যাসোসিয়েটিভ অ্যারে (স্ট্রিং-কীড) হিসেবে আনপ্যাক করা সম্ভব কারণ এটি [নামযুক্ত আর্গুমেন্ট](#unpacking-named-arguments) ব্যবহার করে .

#### অ্যারে আনপ্যাকিং

##### ইন্ডেক্সড অ্যারে

![php-version-74](https://shields.io/badge/php->=7.4-blue)

আপনি যখন একাধিক অ্যারে মার্জ করতে চান, আপনি সাধারণত ব্যবহার করেন `array_merge`:

```php
$array1 = ['baz'];
$array2 = ['foo', 'bar'];

$array3 = array_merge($array1,$array2);
// $array3 = ['baz', 'foo', 'bar']
```

কিন্তু PHP 7.4 থেকে, আপনি স্প্রেড অপারেটরের সাথে ইনডেক্সড অ্যারে আনপ্যাক করতে পারেন:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1];
// $array2 = ['baz', 'foo', 'bar']
```

এলিমেন্টগুলি যে ক্রমে পাস করা হবে সেভাবে মার্জ করা হবে:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz', ...$array1, "qux"];
// $array2 = ['baz', 'foo', 'bar', "qux"]
```

এটি কোনো অনুলিপি করে না:

```php
$array1 = ['foo', 'bar'];
$array2 = ['foo', ...$array1];
// $array2 = ['foo', 'foo', 'bar']
```

আপনি একবারে একাধিক অ্যারে আনপ্যাক করতে পারেন:

```php
$array1 = ['foo', 'bar'];
$array2 = ['baz'];
$array3 = [ ...$array1, ...$array2];
// $array3 = ['foo', 'bar', 'baz']
```

আপনি একই অ্যারে একাধিকবার আনপ্যাক করতে পারেন:

```php
$array1 = ['foo', 'bar'];
$array2 = [ ...$array1, ...$array1];
// $array2 = ['foo', 'bar', 'foo', 'bar']
```

আপনি কোনও এরর বা ওয়ার্নিং ছাড়াই একটি এম্পটি অ্যারে আনপ্যাক করতে পারেন:

```php
$array1 = [];
$array2 = ['foo', ...$array1];
// $array2 = ['foo']
```

আপনি একটি অ্যারে আনপ্যাক করতে পারেন যা পূর্বে একটি প্রোপার্টিতেও রাখা হয়নি:

```php
$array1 = [...['foo', 'bar'], 'baz'];
// $array1 = ['foo', 'bar', 'baz']
```

আনপ্যাকিং শুধুমাত্র অ্যারের সাথে কাজ করে (অথবা ট্রাভার্সেবল ইন্টারফেসের অবজেক্টের সাথে)। আপনি যদি অন্য কোনো ভ্যালু আনপ্যাক করার চেষ্টা করেন, যেমন নাল, আপনি এরর পাবেন:

```php
$array1 = null;
$array2 = ['foo', ...$array1]; // PHP Error:  Only arrays and Traversables can be unpacked
```

আপনি একটি ফাংশন/মেথডের রেজাল্ট আনপ্যাক করতে পারেন:

```php
function getArray(): array
{
    return ['foo', 'bar'];
}

$array = [...getArray(), 'baz']; 
// $array = ['foo', 'bar', 'baz']
```

##### অ্যাসোসিয়েটিভ অ্যারে

![php-version-81](https://shields.io/badge/php->=8.1-blue)

php 8.1 থেকে, আপনি অ্যাসোসিয়েটিভ অ্যারে আনপ্যাক করতে পারেন (স্ট্রিং-কীড):

```php
$array1 = ['foo' => 'bar'];
$array2 = [
   'baz' => 'qux', 
   ...$array1
];
// $array2 = ['baz' => 'qux', 'foo' => 'bar',]
```

আপনি ইতিমধ্যে বিদ্যমান কী দিয়ে অ্যারে আনপ্যাক করতে পারেন:

```php
$array1 = ['foo' => 'bar'];
$array2 = [
   'foo' => 'baz', 
   ...$array1
];
// $array2 = ['foo' => 'bar',]
```

আপনি এরর বা ওয়ার্নিং ছাড়াই একটি খালি অ্যারে আনপ্যাক করতে পারেন:

```php
$array1 = [];
$array2 = [
   ...$array1,
   ...[]
];
// $array2 = []
```

### নেমড আর্গুমেন্টস

![php-version-80](https://shields.io/badge/php->=8.0-blue)

PHP 8.0 থেকে, অবস্থানের পরিবর্তে নাম দিয়ে আর্গুমেন্টে পাস করা সম্ভব.

এই ফাংশনটি বিবেচনা করুন:

```php
function concat(string $first, string $second): string
{
    return $first . ' ' . $second;
}
$a = concat('foo', 'bar');
// $a = 'foo bar'
```

আপনি নামযুক্ত আর্গুমেন্ট সিনট্যাক্সের থেকেও একই ফলাফল পেতে পারেন:

```php
$a = concat(first: 'foo', second: 'bar');
// $a = 'foo bar'
```

আপনি এটিকে ভিন্ন ক্রমে আর্গুমেন্ট সহ কল করতে পারেন:

```php
$a = concat(second: 'bar', first: 'foo');
// $a = 'foo bar'
```

আপনি অপশনাল প্যারামিটার এড়িয়ে যেতে পারেন:

```php
function orGate(bool $option1 = false, bool $option2 = false, bool $option3 = false): bool
{
   return $option1 || $option2 || $option3;
}
$a = orGate(option3: true);
// $a = true
```

কিন্তু আপনি অত্যাবশ্যকীয় আর্গুমেন্ট এড়িয়ে যেতে পারবেন না:

```php
$a = concat(second: 'bar');
// TypeError: concat(): Argument #1 ($first) not passed
```

আপনি অতিরিক্ত আর্গুমেন্ট অন্তর্ভুক্ত করতে পারবেন না:

```php
$a = concat(first: 'foo', second: 'bar', third: 'baz');
// PHP Error:  Unknown named parameter $third
```

নামযুক্ত আর্গুমেন্ট অবজেক্ট কনস্ট্রাক্টরের সাথেও কাজ করে:

```php
Class Foo
{
    public function __construct(
        public string $first,
        public string $second
    ) {}
    
}
$f = new Foo(first: 'bar', second: 'baz');
```

#### নেমড ভেরিয়াডিকস

আপনি ভেরিয়াডিক প্যারামিটার সহ নামযুক্ত আর্গুমেন্ট ব্যবহার করতে পারেন:

```php
function showParams(string ...$params): array
{
    return $params;
}
$a = showParams(first: 'foo', second: 'bar', third: 'baz');
// $a = ["first" => "foo", "second" => "bar", "third" => "baz"]
```

#### আনপ্যাকিং নেমড আর্গুমেন্টস

আপনি নামযুক্ত আর্গুমেন্ট হিসাবে একটি আসোসিয়েটিভ অ্যারে আনপ্যাক করতে পারেন যদি কীগুলি আর্গুমেন্টের নামের সাথে মেলে:

```php
function add(int $a, int $b, int $c): int
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

অ্যাসোসিয়েটিভ অ্যারের উপাদানগুলির ক্ষেত্রে ক্রম কোন ব্যাপার নয়:

```php
function add(int $a, int $b, int $c): int
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

যদি একটি কী একটি আর্গুমেন্টের নামের সাথে না মেলে, তাহলে আপনি এরর পাবেন:

```php
function add(int $a, int $b, int $c): int
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

#### এক্সটারনাল রিসোর্স

- [Named arguments in depth on stitcher's blof](https://stitcher.io/blog/php-8-named-arguments)
- [Named Parameters on PHP.Watch](https://php.watch/versions/8.0/named-parameters)

### শর্ট ক্লোজার্স

![php-version-74](https://shields.io/badge/php->=7.4-blue)

শর্ট ক্লোজার, যাকে অ্যারো ফাংশনও বলা হয়, একটি সংক্ষিপ্ত সিনট্যাক্স গঠনে [অ্যানোনিমাস  ফাংশন](https://www.php.net/manual/en/functions.anonymous.php) লেখার একটি বিকল্প উপায়। শর্ট ক্লোজারের মূল লক্ষ্য হল শব্দচয় কমানো, যদি সম্ভব হয়: যদি শুধুমাত্র একটি সিঙ্গেল এক্সপ্রেশন থাকে.

এখানে শুধুমাত্র একটি এক্সপ্রেশন সহ একটি সিম্পল ক্লোজারের একটি উদাহরণ দেয়া হয়েছে :

```php
$foo = function ($bar) {
    return $bar + 1;
}
$a = $foo(1);
// $a = 2
```

আপনি শর্ট ক্লোজার দিয়ে একই ফাংশন লিখতে পারেন :

```php
$foo = fn ($bar) => $bar + 1;
$a = $foo(1);
// $a = 2
```

আপনি শর্ট ক্লোজারের কোন নাম দিতে পারবেন না :

```php
fn foo($bar) => $bar + 1;
// PHP Parse error: Syntax error, unexpected T_STRING, expecting '('
```

আপনি ফাংশন প্যারামিটার হিসাবে শর্ট ক্লোজার ব্যবহার করতে পারেন। উদাহরণস্বরূপ PHP-এর [array_reduce](https://www.php.net/manual/en/function.array-reduce.php) এর  "কলেবল" প্যারামিটার:

```php
$myArray = [10,20,30];

$total = array_reduce($myArray, fn ($carry, $item) => $carry + $item, 0);
// $total = 60
```

স্বাভাবিক ফাংশন এ টাইপ হিন্টিং অনুমোদিত :

```php
fn (int $foo): int => $foo;
```

আপনাকে `return` কীওয়ার্ড ব্যবহার করতে হবে না কারণ এটি এখানে অনুমোদিত নয় :

```php
fn ($foo) => return $foo;
// PHP Parse error: Syntax error, unexpected T_RETURN
```

#### আউটার স্কোপ

শর্ট ক্লোজারের জন্য আউটার স্কোপ থেকে প্রোপার্টিস অ্যাক্সেস করার জন্য `use` কীওয়ার্ডের প্রয়োজন হয় না :

```php
$bar = 10;
$baz = fn ($foo) => $foo + $bar;
$a = $baz(1);
//$a = 11
```

কীওয়ার্ড `use` অনুমোদিত নয় :

```php
$bar = 10;
fn ($foo) use ($bar) => $foo + $bar;
// PHP Parse error: Syntax error, unexpected T_USE, expecting T_DOUBLE_ARROW
```

আপনি অন্য যেকোনো ফাংশনের মতো `$this` ব্যবহার করতে পারেন :

```php
fn () => $this->foo + 1;
```

### ম্যাচ এক্সপ্রেশন

![php-version-80](https://shields.io/badge/php->=8.0-blue)

PHP 8.0 থেকে, একটি নতুন `match` সিনট্যাক্স আছে `switch` সিনট্যাক্সের মতো। যেহেতু প্রতিটি ম্যাচিং এর ক্ষেত্রে শুধুমাত্র একটি এক্সপ্রেশন থাকতে হবে, তাই এটি ব্যবহার করা যাবে না এবং সেই পরিস্থিতিতে সুইচ স্টেটমেন্ট দিয়ে প্রতিস্থাপন করতে হবে। যদিও এটি উল্লেখযোগ্যভাবে ছোট এবং পড়া সহজ.

ম্যাচ এক্সপ্রেশন সবসময় একটি ভ্যালু রিটার্ণ করে। প্রতিটি শর্ত শুধুমাত্র একটি এক্সপ্রেশনের অনুমতি দেয় এবং এটি অবিলম্বে ভ্যালু রিটার্ণ করে এবং একটি সুস্পষ্ট 'break' বিবৃতি ছাড়া বাকি শর্তগুলিতে যাবেনা :

```php
$foo = 'baz';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
    'qux' => 3,
}
// $a = 2
```

ভ্যালু ম্যাচ না করলে এটি এক্সেপশন থ্রো করে:

```php
$foo = 'qux';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
}
// PHP Error:  Unhandled match value of type string
```

কিন্তু এটি একটি ডিফল্ট কন্ডিশন সমর্থন করে:

```php
$foo = 'qux';
$a = match($foo) {
    'bar' => 1,
    'baz' => 2,
    default => 3,
}
// $a = 3
```

এটি একটি আর্মে একাধিক শর্তের অনুমতি দেয়:

```php
$foo = 'bar';
$a = match($foo) {
    'bar', 'baz' => 1,
    default => 2,
}
// $a = 1
```

এটি টাইপ কোয়ার্শন ছাড়াই স্ট্রিক্ট টাইপ-নিরাপদ তুলনা করতে পারে (এটি `==` এর পরিবর্তে `===` ব্যবহার করার মতো):

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

#### এক্সটারনাল রিসোর্স

- [Match expression on PHP.Watch](https://php.watch/versions/8.0/match-expression)

### স্ট্রিংয়েবল ইন্টারফেস

![php-version-80](https://shields.io/badge/php->=8.0-blue)

PHP 8.0 থেকে, একটি নতুন ইন্টারফেস রয়েছে যার নাম `Stringable`, যা নির্দেশ করে একটি ক্লাসের একটি `__toString()` ম্যাজিক মেথডকে। পিএইচপি স্বয়ংক্রিয়ভাবে সমস্ত ক্লাসে স্ট্রিংয়েবল ইন্টারফেস যুক্ত করে দেয় যারা এই মেথডকে ইমপ্লিমেন্ট করেছে .

```php
interface Stringable {
    public function __toString(): string;
}
```

আপনি যখন `Stringable` টাইপের একটি প্যারামিটার ডিফাইন করেন, তখন এটি চেক করবে যে প্রদত্ত ক্লাসটি `Stringable` ইন্টারফেস ইমপ্লিমেন্ট করেছে কিনা:

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

যদি প্রদত্ত ক্লাস `__toString()` ইমপ্লিমেন্ট না করে, তাহলে আপনি একটি এরর পাবেন :

```php
class Foo {
}

function myFunction(Stringable $param): string {
    return (string) $param;
}
$a = myFunction(new Foo);
// TypeError: myFunction(): Argument #1 ($param) must be of type Stringable, Foo given
```

স্ট্রিংয়েবল টাইপ স্ট্রিং গ্রহণ করে না:

```php
function myFunction(Stringable $param): string {
    return (string) $param;
}
$a = myFunction('foo');
// TypeError: myFunction(): Argument #1 ($param) must be of type Stringable, string given
```

অবশ্যই, স্ট্রিং এবং স্ট্রিংএবল উভয়ই গ্রহণ করার জন্য, আপনি ইউনিয়ন টাইপ ব্যবহার করতে পারেন:

```php
function myFunction(string|Stringable $param): string {
    return (string) $param;
}
```


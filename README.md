# Правила оформления PHP-кода

Данный документ содержит основные правила оформлени PHP-кода, принятые в компании. 

Любой новый код пишется с применением этих правил.

При правках старого кода затронутые участки приводятся в соответствие с документом.
Если есть возможность, исправляется весь файл. При добавлении файл в git вносится комментарий: `note: code rules applied (partially)`.

## Именование

+ Имена классов пишутся в стиле `CamelCase` с первой заглавной буквой
+ Имена функций, методов, свойств и переменных - `camelCase` с первой строчный буквой.
  *Исключеные:* имена методов контроллеров CakePHP, доступных через HTTP, записываются в стиле `under_score`

+ Имена ключей ассоциативных массивов - `under_score`
+ Имена констант - `UPPER_CASE`

Имена приватных методов должны начинаться с двух подчеркиваний, например, `__privateMethod`.

## Компановка кода классов

1. Свойства фреймворка
2. Собственные свойства
3. HTTP-методы
4. Публичные методы
5. Защищенные методы
6. Приватные методы

## Базовое форматирование

### Табуляция

Используется для структурирования кода. Каждый новый вложенный уровень начинается с дополнительной табуляции.
Вложенным уровним считается тело класса, функции, цикла и условия, а также элементы массива, при необходимости
его записи в несколько строк, и операнды длинного выражения, разбитого на строки
(подробнее в разделе "Перенос строк").

**Пример:**

```PHP
class MyClass {

  public $myVar = array(
    'item_one' => 1,
    'item_two' => 2
  );

  public function example($arg) {
    if ($condition) {
      foreach ($arg as $i) {
        print exp($i);
      }
    }
  }
}
```

### Пробелы

Все операторы включая операторы конкатиниции и присваивания выделяится пробелом с обеих сторон.
*Исключение:* оператор отрицания пробелами *не* вы выделяется.

**Пример:**

```PHP
$string = subString1 . subString2;

if (!$condition) {
	$a = $b + $c - ($d / 10 * $i);
}
```

Разрешается выравнивать знаки "равно" в серии присваиваний пробелами по правому краю.

**Пример:**

```PHP
$title        = 'some text';
$orders_count = 5;
$customers    = array ();
```

В списке аргументов ставятся пробелы после запятых.

**Пример:**

```PHP
str_replace(' ', '_', $string);
```

При определении функции перед скобками с аргументами пробел не ставится.

**Пример:**

```PHP
function foo($message) {
    echo $message;
}
```

### Перенос строк

Длинные выражения необходимо разбивать на строки длиной не более 80 символов.

При объявлении массива первый каждый последующий уровень вложенности выделяется дополнительной табуляцией 
(подробнее в разделе "Табуляция").

**Пример**

```PHP
$myArray = array(
	'item_one' => 'Some string',
	'item_two' => array(
		1, 2, 3, 4, 5
	)
	...
);
```

При необходимости записать в несколько строк список аргументов, принимаемый функцией, орматирование кода
производится аналогично объявления массива.

**Пример**

```PHP
$value = someFunction(
	$firstVeryLongNameArgument,
	$secondVeryLongNameArgument
);
```

При необходимости разбиения на строки арифметического, логического или конкатинационного выражения
каждая новая строка должна начинаться с оператора.

**Пример:**

```PHP
$image_aspect_ratio = $this->request->data['Image']['with']
	/ $this->request->data['Image']['height'];
	
if (
	$this->User->save($this->request->data)
	&& $this->request->data['User']['subscribe']
) {
	$this->Mailchimp->add_subscriber($this->request->data);
}

$name = $this->request->data['User’]['first_name'] . ' '
	. $this->request->data['User']['last_name'];
```

## Блоки кода

### Фигурные скобки

Открывающая скобка ставится сразу за выражением на той же строке, через пробел. Закрывающая - *всегда* на новой строке.

**Пример:**

```PHP
if($a == $b) {
    echo 'Hello world!';
    echo 'Goodbye!';
}
```

При использовании множественного условного ветвения каждое очередное условие и конечное условие `else`
пишутся на той же строке, что и закрывающая фигурная скобка блока кода, принадлежащего предыдущему условию.

**Пример:**

```PHP
if($var = 'foo') {
	echo 'bar';
} elseif($var == 'baz') {
	echo 'qux';
} else {
	echo 'Something went wrong';
}
```

Если фигурные скобки требуются, но выражения в них нет, то они не переносятся на новую строку 
(пробел внутри не ставится).

**Пример:**

```PHP
class MyClass implements MyInterface {

	public function sum($a, $b) {
		return $a + $b;
	}
	
	public funtion multiplication($a, $b) {}

}
```

### Пропуск строк

Пустыми строками выделяется каждый блок кода, заключенный в фигурные скобки, а также свойства классов. 
*Исключение:* во множественном условном ветвении отбивка производится только перед начальным условием и после
последней закрывающей фигурной скобки.

Если в коде подряд идут несколько закрывающих фигурных скобок дополнительное выделение пустой строкой не производится.

**Пример**

```PHP
class MyClass {

	public $var = 'Some string';
	
	public funtion someMethod($a, $b) {
	
		if($a > $b) {
			echo 'a more then b';
		} elseif($a < $b) {
			echo 'b more then a';
		} else {
			echo 'a equals b';
		}
	}
}
```

Рекомендуется оставлять пустую строку перед оператором `return` и разбивать пустыми строками длинные участки кода,
группируя логически связанные части.

**Пример:**

```PHP
function myFunction($paramOne, $paramTwo) {
	$insanceOne = new SomeClass($paramOne);
		
	$instanceOne->setWidth(100);
	$instanceOne->setHeight(80);
	$instanceOne->setQuality(75);
		
	$instanceTwo = new OtherClass($paramTwo);
		
	$instanceTwo->render($instanceOne);
		
	return $instanceTwo->getRresult();
}
```

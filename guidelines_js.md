# Руководство по написанию JavaScript-кода

*На основе Airbnb JavaScript Style Guide*

## Оглавление

  1. [Типы](#types)
  1. [Объявление переменных](#references)
  1. [Объекты](#objects)
  1. [Массивы](#arrays)
  1. [Деструктуризация](#destructuring)
  1. [Строки](#strings)
  1. [Функции](#functions)
  1. [Стрелочные функции](#arrow-functions)
  1. [Классы и конструкторы](#classes--constructors)
  1. [Модули](#modules)
  1. [Свойства](#properties)
  1. [Переменные](#variables)
  1. [Подъём](#hoisting)
  1. [Операторы сравнения и равенства](#comparison-operators--equality)
  1. [Блоки](#blocks)
  1. [Управляющие операторы](#control-statements)
  1. [Комментарии](#comments)
  1. [Пробелы](#whitespace)
  1. [Запятые](#commas)
  1. [Точка с запятой](#semicolons)
  1. [Приведение типов](#type-casting--coercion)
  1. [Соглашение об именовании](#naming-conventions)
  1. [Аксессоры](#accessors)
  1. [Стандартная библиотека](#standard-library)

## <a name="types">Типы</a>

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Простые типы**: Когда вы взаимодействуете с простым типом, вы напрямую работаете с его значением.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **Сложные типы**: Когда вы взаимодействуете со сложным типом, вы работаете со ссылкой на его значение.

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="references">Объявление переменных</a>

**Использование var, let, const**
- `var` - должен использоваться только в клиентском коде, который не будет преобразован в es5 (для совместимости со старыми браузерами).
- `let` - используется для изменяемых переменных (mutable).
- `const` - должен использоваться для объявления констант, а также при объявлении объектов (и массивов).

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Используйте `const` для объявления переменных; избегайте `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.

    ```javascript
    // плохо
    var a = 1;
    var b = 2;

    // хорошо
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) Если вам необходимо переопределять значения, то используйте `let` вместо `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Почему? Область видимости `let` — блок, у `var` — функция.

    ```javascript
    // плохо
    var count = 1;
    if (true) {
      count += 1;
    }

    // хорошо, используйте let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) Помните, что у `let` и `const` блочная область видимости.

    ```javascript
    // const и let существуют только в том блоке, в котором они определены.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="objects">Объекты</a>

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Для создания объекта используйте литеральную нотацию. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // плохо
    const item = new Object();

    // хорошо
    const item = {};
    ```

  <a name="es6-object-shorthand"></a><a name="3.2"></a>
  - [3.2](#es6-object-shorthand) Используйте сокращённую запись метода объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // плохо
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // хорошо
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.3"></a>
  - [3.3](#es6-object-concise) Используйте сокращённую запись свойств объекта. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Почему? Это короче и понятнее.

    ```javascript
    const luke_skywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      luke_skywalker: luke_skywalker,
    };

    // хорошо
    const obj = {
      luke_skywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.4"></a>
  - [3.4](#objects--grouped-shorthand) Группируйте ваши сокращённые записи свойств в начале объявления объекта.

    > Почему? Так легче сказать, какие свойства используют сокращённую запись.

    ```javascript
    const anakin_skywalker = 'Anakin Skywalker';
    const luke_skywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      episode_one: 1,
      two_Jedi_walk: 2,
      luke_skywalker,
      episode_three: 3,
      may_the_fourth: 4,
      anakin_skywalker,
    };

    // хорошо
    const obj = {
      luke_skywalker,
      anakin_skywalker,
      episode_one: 1,
      two_Jedi_walk: 2,
      episode_three: 3,
      may_the_fourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.5"></a>
  - [3.5](#objects--quoted-props) Только недопустимые идентификаторы помещаются в кавычки. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > Почему? На наш взгляд, такой код легче читать. Это улучшает подсветку синтаксиса, а также облегчает оптимизацию для многих JS-движков.

    ```javascript
    // плохо
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // хорошо
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--rest-spread"></a>
  - [3.6](#objects--rest-spread) Используйте оператор расширения вместо [`Object.assign`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов. Используйте синтаксис оставшихся свойств, чтобы получить новый объект с некоторыми опущенными свойствами.

    ```javascript
    // очень плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // эта переменная изменяет `original` ಠ_ಠ
    delete copy.a; // если сделать так

    // плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // хорошо
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrays">Массивы</a>

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Для создания массива используйте литеральную нотацию. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // плохо
    const items = new Array();

    // хорошо
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Для добавления элемента в массив используйте [Array#push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания.

    ```javascript
    const someStack = [];

    // плохо
    someStack[someStack.length] = 'abracadabra';

    // хорошо
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) Для копирования массивов используйте оператор расширения `...`.

    ```javascript
    // плохо
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // хорошо
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) Для преобразования итерируемого объекта в массив используйте оператор расширения `...` вместо [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // хорошо
    const nodes = Array.from(foo);

    // отлично
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) для преобразования массивоподобного объекта в массив.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // плохо
    const arr = arrLike.slice();

    // хорошо
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) Используйте [`Array.from`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/from) вместо оператора расширения `...` для маппинга итерируемых объектов, это позволяет избежать создания промежуточного массива.

    ```javascript
    // плохо
    const baz = [...foo].map(bar);

    // хорошо
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) Используйте операторы `return` внутри функций обратного вызова в методах массива. Можно опустить `return`, когда тело функции состоит из одной инструкции, возвращающей выражение без побочных эффектов. [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => x + 1);

    // плохо - нет возвращаемого значения, следовательно, `acc` становится `undefined` после первой итерации
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // хорошо
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // плохо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // хорошо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) Если массив располагается на нескольких строках, то используйте разрывы строк после открытия и перед закрытием скобок.

    ```javascript
    // плохо
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const object_array = [{
      id: 1,
    }, {
      id: 2,
    }];

    const number_array = [
      1, 2,
    ];

    // хорошо
    const arr = [[0, 1], [2, 3], [4, 5]];

    const object_array = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const number_array = [
      1,
      2,
    ];
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="destructuring">Деструктуризация</a>

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) При обращении к нескольким свойствам объекта используйте деструктуризацию объекта. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Почему? Деструктуризация избавляет вас от создания временных переменных для этих свойств.

    ```javascript
    // плохо
    function getFullName(user) {
      const first_name = user.first_name;
      const last_name = user.last_name;

      return `${first_name} ${last_name}`;
    }

    // хорошо
    function getFullName(user) {
      const { first_name, last_name } = user;
      return `${first_name} ${last_name}`;
    }

    // отлично
    function getFullName({ first_name, last_name }) {
      return `${first_name} ${last_name}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) Используйте деструктуризацию массивов. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // плохо
    const first = arr[0];
    const second = arr[1];

    // хорошо
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) Используйте деструктуризацию объекта для множества возвращаемых значений, но не делайте тоже самое с массивами.

    > Почему? Вы сможете добавить новые свойства через некоторое время или изменить порядок без последствий.

    ```javascript
    // плохо
    function processInput(input) {
      // затем происходит чудо
      return [left, right, top, bottom];
    }

    // при вызове нужно подумать о порядке возвращаемых данных
    const [left, __, top] = processInput(input);

    // хорошо
    function processInput(input) {
      // затем происходит чудо
      return { left, right, top, bottom };
    }

    // при вызове выбираем только необходимые данные
    const { left, top } = processInput(input);
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="strings">Строки</a>

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) Используйте одинарные кавычки `''` для строк. eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // плохо
    const name = "Capt. Janeway";

    // плохо - литерал шаблонной строки должен содержать интерполяцию или переводы строк
    const name = `Capt. Janeway`;

    // хорошо
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Строки, у которых в строчке содержится более 100 символов, не пишутся на нескольких строчках с использованием конкатенации.

    > Почему? Работать с разбитыми строками неудобно и это затрудняет поиск по коду.

    ```javascript
    // плохо
    const error_message = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // плохо
    const error_message = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // хорошо
    const error_message = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) При создании строки программным путём используйте шаблонные строки вместо конкатенации. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Почему? Шаблонные строки дают вам читабельность, лаконичный синтаксис с правильными символами перевода строк и функции интерполяции строки.

    ```javascript
    // плохо
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // плохо
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // плохо
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // хорошо
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) Никогда не используйте `eval()`, т.к. это открывает множество уязвимостей. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)


**[⬆ к оглавлению](#Оглавление)**

## <a name="functions">Функции</a>

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) Имя функции должно быть глаголом на английском языке или начинаться с него. Для имён, состоящих из нескольких слов, используйте **camelCase**.

    ```javascript
    // плохо
    function pravkaspiska() {
      // ...
    };

    // хорошо
    function editName() {
    // тело функции
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) Оборачивайте в скобки немедленно вызываемые функции. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > Почему? Немедленно вызываемая функция представляет собой единый блок. Чтобы чётко показать это — оберните функцию и вызывающие скобки в ещё одни скобки. Обратите внимание, что в мире с модулями вам больше не нужны немедленно вызываемые функции.

    ```javascript
    // Немедленно вызываемая функция
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) Никогда не объявляйте функции в нефункциональном блоке (`if`, `while`, и т.д.). Вместо этого присвойте функцию переменной. Браузеры позволяют выполнить ваш код, но все они интерпретируют его по-разному. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **Примечание:** ECMA-262 определяет `блок` как список инструкций. Объявление функции не является инструкцией.

    ```javascript
    // плохо
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // хорошо
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) Никогда не называйте параметр `arguments`. Он будет иметь приоритет над объектом `arguments`, который доступен для каждой функции.

    ```javascript
    // плохо
    function foo(name, options, arguments) {
      // ...
    }

    // хорошо
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) Никогда не используйте `arguments`, вместо этого используйте синтаксис оставшихся параметров `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Почему? `...` явно говорит о том, какие именно аргументы вы хотите извлечь. Кроме того, такой синтаксис создаёт настоящий массив, а не массивоподобный объект как `arguments`.

    ```javascript
    // плохо
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // хорошо
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) Используйте синтаксис записи аргументов по умолчанию, а не изменяйте аргументы функции.

    ```javascript
    // очень плохо
    function handleThings(opts) {
      // Нет! Мы не должны изменять аргументы функции.
      // Плохо вдвойне: если переменная opts будет ложной,
      // то ей присвоится пустой объект, а не то что вы хотели.
      // Это приведёт к коварным ошибкам.
      opts = opts || {};
      // ...
    }

    // всё ещё плохо
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // хорошо
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) Избегайте побочных эффектов с параметрами по умолчанию.

    > Почему? И так всё понятно.

    ```javascript
    var b = 1;
    // плохо
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) Всегда вставляйте последними параметры по умолчанию.

    ```javascript
    // плохо
    function handleThings(opts = {}, name) {
      // ...
    }

    // хорошо
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) Никогда не используйте конструктор функций для создания новых функий. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > Почему? Создание функции в таком духе вычисляет строку подобно `eval()`, из-за чего открываются уязвимости.

    ```javascript
    // плохо
    var add = new Function('a', 'b', 'return a + b');

    // всё ещё плохо
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) Отступы при определении функции. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Почему? Однородность кода — это хорошо. Вам не надо будет добавлять или удалять пробел при манипуляции с именем.

    ```javascript
    // плохо
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // хорошо
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) Никогда не изменяйте параметры. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Манипуляция объектами, приходящими в качестве параметров, может вызывать нежелательные побочные эффекты в вызывающей функции.

    ```javascript
    // плохо
    function f1(obj) {
      obj.key = 1;
    }

    // хорошо
    function f2(obj) {
      const key = obj.hasOwnProperty('key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) Никогда не переназначайте параметры. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Переназначенные параметры могут привести к неожиданному поведению, особенно при обращении к `arguments`. Это также может вызывать проблемы оптимизации, особенно в V8.

    ```javascript
    // плохо
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // хорошо
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) Отдавайте предпочтение использованию оператора расширения `...` при вызове вариативной функции. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Почему? Это чище, вам не нужно предоставлять контекст, и не так просто составить `new` с `apply`.

    ```javascript
    // плохо
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // хорошо
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // хорошо
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) Функции с многострочным определением или запуском должны содержать такие же отступы, как и другие многострочные списки в этом руководстве: с каждым элементом на отдельной строке, с запятой в конце элемента. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // плохо
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // хорошо
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // плохо
    console.log(foo,
      bar,
      baz);

    // хорошо
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrow-functions">Стрелочные функции</a>

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > Почему? Таким образом создаётся функция, которая выполняется в контексте `this`, который мы обычно хотим, а также это более короткий синтаксис.

    > Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить эту логику внутрь её собственного именованного функционального выражения.

    ```javascript
    // плохо
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) Если тело функции состоит из одного оператора, возвращающего [выражение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators#Выражения) без побочных эффектов, то опустите фигурные скобки и используйте неявное возвращение. В противном случае, сохраните фигурные скобки и используйте оператор `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > Почему? Синтаксический сахар. Когда несколько функций соединены вместе, то это лучше читается.

    ```javascript
    // плохо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // хорошо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Неявный возврат с побочными эффектами
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Сделать что-то, если функция обратного вызова вернёт true
      }
    }

    let bool = false;

    // плохо
    foo(() => bool = true);

    // хорошо
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) В случае, если выражение располагается на нескольких строках, то необходимо обернуть его в скобки для лучшей читаемости.

    > Почему? Это чётко показывает, где функция начинается и где заканчивается.

    ```javascript
    // плохо
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // хорошо
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) Всегда оборачивайте аргументы круглыми скобками для ясности и согласованности. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)
    > Почему? Минимизирует различия при удалении или добавлении аргументов.

    ```javascript
    // плохо
    [1, 2, 3].map(x => x * x);

    // хорошо
    [1, 2, 3].map((x) => x * x);

    // плохо
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // хорошо
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // плохо
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) Избегайте схожести стрелочной функции (`=>`) с операторами сравнения (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // плохо
    const item_height = (item) => item.height <= 256 ? item.large_size : item.small_size;

    // плохо
    const item_height = (item) => item.height >= 256 ? item.large_size : item.small_size;

    // хорошо
    const item_height = (item) => (item.height <= 256 ? item.large_size : item.small_size);

    // хорошо
    const item_height = (item) => {
      const { height, large_size, small_size } = item;
      return height <= 256 ? large_size : small_size;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>
  - [8.6](#whitespace--implicit-arrow-linebreak) Зафиксируйте расположение тела стрелочной функции с неявным возвратом. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // плохо
    (foo) =>
      bar;
    (foo) =>
      (bar);

    // хорошо
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="classes--constructors">Классы и конструкторы</a>

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) Всегда используйте `class`. Избегайте прямых манипуляций с `prototype`.

    > Почему? Синтаксис `class` является кратким и понятным.

    ```javascript
    // плохо
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // хорошо
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) Используйте `extends` для наследования.

    > Почему? Это встроенный способ наследовать функциональность прототипа, не нарушая `instanceof`.

    ```javascript
    // плохо
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // хорошо
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) Методы могут возвращать `this`, чтобы делать цепочки вызовов.

    ```javascript
    // плохо
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // хорошо
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) Вы можете определить свой собственный метод `toString()`, просто убедитесь, что он успешно работает и не создаёт никаких побочных эффектов.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) У классов есть конструктор по умолчанию, если он не задан явно. Можно опустить пустой конструктор или конструктор, который только делегирует выполнение родительскому классу. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // плохо
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // плохо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // хорошо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) Избегайте дублирующих членов класса. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Почему? Если объявление члена класса повторяется, без предупреждения будет использовано последнее. Наличие дубликатов скорее всего приведёт к ошибке.

    ```javascript
    // плохо
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // хорошо
    class Foo {
      bar() { return 1; }
    }

    // хорошо
    class Foo {
      bar() { return 2; }
    }
    ```

  <a name="classes--methods-use-this"></a>
  - [9.7](#classes--methods-use-this) Метод класса должен использовать `this` или быть преобразованным в статический метод, если только внешняя библиотека или фреймворк не требуют использования определённых нестатических методов. Будучи методом экземпляра, следует указать, что он ведёт себя по-разному в зависимости от свойств получателя. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // плохо
    class Foo {
      bar() {
        console.log('bar');
      }
    }
    // хорошо - используется this
    class Foo {
      bar() {
        console.log(this.bar);
      }
    }
    // хорошо - конструктор освобождается
    class Foo {
      constructor() {
        // ...
      }
    }
    // хорошо - статические методы не должны использовать this
    class Foo {
      static bar() {
        console.log('bar');
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="modules">Модули</a>

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) Всегда используйте модули (`import`/`export`) вместо нестандартных модульных систем. Вы всегда сможете транспилировать код в вашу любимую модульную систему.

    > Почему? Модули — это будущее. Давайте начнём использовать будущее уже сейчас!

    ```javascript
    // плохо
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // отлично
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) Не используйте импорт через `*`.

    > Почему? Это гарантирует, что у вас есть единственный экспорт по умолчанию.

    ```javascript
    // плохо
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // хорошо
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) Не экспортируйте прямо из импорта.

    > Почему? Несмотря на то, что запись в одну строку является краткой, разделение на отдельные строки делает вещи последовательными.

    ```javascript
    // плохо
    // файл es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // хорошо
    // файл es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) Импортируйте из пути только один раз.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > Почему? Наличие нескольких строк, которые импортируют из одного и того же файла, может сделать код неподдерживаемым.

    ```javascript
    // плохо
    import foo from 'foo';
    // … какие-то другие импорты … //
    import { named1, named2 } from 'foo';

    // хорошо
    import foo, { named1, named2 } from 'foo';

    // хорошо
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) Не экспортируйте изменяемые переменные.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Почему? Вообще, следует избегать мутации, в особенности при экспорте изменяемых переменных. Несмотря на то, что эта техника может быть необходима в редких случаях, в основном только константа должна быть экспортирована.

    ```javascript
    // плохо
    let foo = 3;
    export { foo };

    // хорошо
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) В модулях с единственным экспортом предпочтительнее использовать экспорт по умолчанию, а не экспорт по имени.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Почему? Для того чтобы поощрять создание множества файлов, которые бы экспортировали одну сущность, т.к. это лучше для читабельности и поддержки кода.

    ```javascript
    // плохо
    export function foo() {}

    // хорошо
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) Поместите все импорты выше остальных инструкций.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Почему? Так как `import` обладает подъёмом, то хранение их всех в начале файла предотвращает от неожиданного поведения.

    ```javascript
    // плохо
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // хорошо
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) Импорты на нескольких строках должны быть с отступами как у многострочных литералов массива и объекта.

    > Почему? Фигурные скобки следуют тем же правилам отступа как и любая другая фигурная скобка блока в этом руководстве, тоже самое касается висячих запятых.

    ```javascript
    // плохо
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // хорошо
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) Запретите синтаксис загрузчика Webpack в импорте.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Почему? Использование Webpack синтаксиса связывает код с упаковщиком модулей. Предпочтительно использовать синтаксис загрузчика в `webpack.config.js`.

    ```javascript
    // плохо
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // хорошо
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="properties">Свойства</a>

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) Используйте точечную нотацию для доступа к свойствам. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // плохо
    const is_jedi = luke['jedi'];

    // хорошо
    const is_jedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    const prop_name = 'jedi';
    const is_jedi = luke[prop_name];
    ```

  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) Используйте оператор `**` для возведения в степень. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // плохо
    const binary = Math.pow(2, 10);

    // хорошо
    const binary = 2 ** 10;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="variables">Переменные</a>

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) Всегда используйте `const` или `let` для объявления переменных. Невыполнение этого требования приведёт к появлению глобальных переменных. Необходимо избегать загрязнения глобального пространства имён. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // плохо
    superPower = new SuperPower();

    // хорошо
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) Используйте объявление `const` или `let` для каждой переменной или присвоения. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Почему? Таким образом проще добавить новые переменные. Также вы никогда не будете беспокоиться о перемещении `;` и `,` и об отображении изменений в пунктуации. Вы также можете пройтись по каждому объявлению с помощью отладчика, вместо того, чтобы прыгать через все сразу.

    ```javascript
    // плохо
    const items = getItems(),
        go_sports = true,
        dragonball = 'z';

    // плохо
    // (сравните с кодом выше и попытайтесь найти ошибку)
    const items = getItems(),
        go_sports = true;
        dragonball = 'z';

    // хорошо
    const items = getItems();
    const go_sports = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) В первую очередь группируйте `const`, а затем `let`.

    > Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

    ```javascript
    // плохо
    let i, len, dragonball,
        items = getItems(),
        go_sports = true;

    // плохо
    let i;
    const items = getItems();
    let dragonball;
    const go_sports = true;
    let len;

    // хорошо
    const go_sports = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) Создавайте переменные там, где они вам необходимы, но помещайте их в подходящее место.

    > Почему? `let` и `const` имеют блочную область видимости, а не функциональную.

    ```javascript
    // плохо - вызов ненужной функции
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // хорошо
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) Не создавайте цепочки присваивания переменных. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > Почему? Такие цепочки создают неявные глобальные переменные.

    ```javascript
    // плохо
    (function example() {
      // JavaScript интерпретирует это, как
      // let a = ( b = ( c = 1 ) );
      // Ключевое слово let применится только к переменной a;
      // переменные b и c станут глобальными.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // хорошо
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // тоже самое и для `const`
    ```

  <a name="variables--linebreak"></a>
  - [13.6](#variables--linebreak) В присвоении избегайте разрывов строк до и после `=`. Если ваше присвоение нарушает правило [`max-len`](https://eslint.org/docs/rules/max-len.html), оберните значение в круглые скобки. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > Почему? Разрывы строк до и после `=` могут приводить к путанице в понимании значения.

    ```javascript
    // плохо
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // плохо
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // хорошо
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // хорошо
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

  <a name="variables--no-unused-vars"></a>
  - [13.7](#variables--no-unused-vars) Запретить неиспользуемые переменные. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Почему? Переменные, которые объявлены и не используются в коде, скорее всего, являются ошибкой из-за незавершённого рефакторинга. Такие переменные занимают место в коде и могут привести к путанице при чтении.

    ```javascript
    // плохо

    var some_unused_var = 42;

    // Переменные, которые используются только для записи, не считаются используемыми.
    var y = 10;
    y = 5;

    // Чтение для собственной модификации.
    var z = 0;
    z = z + 1;

    // Неиспользуемые аргументы функции.
    function getX(x, y) {
        return x;
    }

    // хорошо

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));

    // Переменная 'type' игнорируется, даже если она не испольуется, потому что рядом есть rest-свойство.
    // Эта форма извлечения объекта, которая опускает указанные ключи.
    var { type, ...coords } = data;
    // 'coords' теперь 'data' объект без свойства 'type'.
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="hoisting">Подъём</a>

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) Объявления `var` поднимаются в начало области видимости ближайшей закрывающей их функции, а их присвоение нет. Объявления `const` и `let` работают по новой концепции называемой [Временные Мёртвые Зоны (Temporal Dead Zone)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/let#Временные_мертвые_зоны_и_ошибки_при_использовании_let). Важно знать, почему использовать [typeof больше не безопасно](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // мы знаем, что это не будет работать
    // (если нет глобальной переменной notDefined)
    function example() {
      console.log(notDefined); // => выбросит ошибку ReferenceError
    }

    // обращение к переменной до её создания
    // будет работать из-за подъёма.
    // Примечание: значение true не поднимается.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // интерпретатор понимает объявление
    // переменной в начало области видимости.
    // это означает, что наш пример
    // можно переписать таким образом:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // использование const и let
    function example() {
      console.log(declaredButNotAssigned); // => выбросит ошибку ReferenceError
      console.log(typeof declaredButNotAssigned); // => выбросит ошибку ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) Для анонимных функциональных выражений наверх области видимости поднимается название переменной, но не её значение.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous не является функцией

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expressions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) Для именованных функциональных выражений наверх области видимости поднимается название переменной, но не имя или тело функции.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named не является функцией

      superPower(); // => ReferenceError superPower не определена

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // тоже самое справедливо, когда имя функции
    // совпадает с именем переменной.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named не является функцией

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) При объявлении функции её имя и тело поднимаются наверх области видимости.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="comparison-operators--equality">Операторы сравнения и равенства</a>

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) Используйте `===` и `!==` вместо `==` и `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) Условные операторы, такие как `if`, вычисляются путём приведения к логическому типу `Boolean` через абстрактный метод `ToBoolean` и всегда следуют следующим правилам:
    - **Object** соответствует **true**
    - **Undefined** соответствует **false**
    - **Null** соответствует **false**
    - **Boolean** соответствует **значению булева типа**
    - **Number** соответствует **false**, если **+0, -0, or NaN**, в остальных случаях **true**
    - **String** соответствует **false**, если строка пустая `''`, в остальных случаях **true**

    ```javascript
    if ([0] && []) {
      // true
      // Массив (даже пустой) является объектом, а объекты возвращают true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) Используйте сокращения для булевских типов, а для строк и чисел применяйте явное сравнение. Иногда сравнение полезно оставить, чтобы код лучше читался.

    ```javascript
    // плохо
    if (isValid === true) {
      // ...
    }

    // хорошо
    if (isValid) {
      // ...
    }

    // плохо
    if (name) {
      // ...
    }

    // хорошо
    if (name !== '') {
      // ...
    }

    // плохо
    if (collection.length) {
      // ...
    }

    // хорошо
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) Используйте фигурные скобки для `case` и `default`, если они содержат лексические декларации (например, `let`, `const`, `function`, и `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html).

    > Почему? Лексические декларации видны во всем `switch` блоке, но инициализируются только при присваивании, которое происходит при входе в блок `case`. Возникают проблемы, когда множество `case` пытаются определить одно и то же.

    ```javascript
    // плохо
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // хорошо
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Тернарные операторы не должны быть вложены и в большинстве случаев должны быть расположены на одной строке. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // плохо
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // разбит на два отдельных тернарных выражения
    const maybe_null = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2
      ? 'bar'
      : maybe_null;

    // отлично
    const foo = maybe1 > maybe2 ? 'bar' : maybe_null;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) Избегайте ненужных тернарных операторов. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // плохо
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // хорошо
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) При смешивании операторов, помещайте их в круглые скобки. Единственным исключением являются стандартные арифметические операторы: `+`, `-` и `**`, так как их приоритет широко известен. Мы рекомендуем заключить `/` и `*` в круглые скобки, поскольку их приоритет может быть неоднозначным, когда они смешиваются. eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > Почему? Это улучшает читаемость и уточняет намерения разработчика.

    ```javascript
    // плохо
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // плохо
    const bar = a ** b - 5 % d;

    // плохо
    // можно ошибиться, думая что это (a || b) && c
    if (a || b && c) {
      return d;
    }

    // плохо
    const bar = a + b / c * d;

    // хорошо
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // хорошо
    const bar = a ** b - (5 % d);

    // хорошо
    if (a || (b && c)) {
      return d;
    }

    // хорошо
    const bar = a + (b / c) * d;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="blocks">Блоки</a>

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) Используйте фигурные скобки, когда блок кода занимает несколько строк. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // плохо
    if (test)
      return false;

    // хорошо
    if (test) return false;

    // хорошо
    if (test) {
      return false;
    }

    // плохо
    function foo() { return false; }

    // хорошо
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) Если блоки кода в условии `if` и `else` занимают несколько строк, расположите оператор `else` на той же строчке, где находится закрывающая фигурная скобка блока `if`. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // плохо
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // хорошо
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) Если в блоке `if` всегда выполняется оператор `return`, последующий блок `else` не нужен. `return`  внутри блока `else if`, следующем за блоком `if`, который содержит `return`, может быть разделён на несколько блоков `if`. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // плохо
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // плохо
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // плохо
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // хорошо
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // хорошо
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // хорошо
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="control-statements">Управляющие операторы</a>

  <a name="control-statements"></a>
  - [17.1](#control-statements) Если ваш управляющий оператор (`if`, `while` и т.д.) слишком длинный или превышает максимальную длину строки, то каждое (сгруппированное) условие можно поместить на новую строку. Логический оператор должен располагаться в начале строки.

    > Почему? Наличие операторов в начале строки приводит к выравниванию операторов и напоминает цепочку методов. Это также улучшает читаемость, упрощая визуальное отслеживание сложной логики.

    ```javascript
    // плохо
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // плохо
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // плохо
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // плохо
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // хорошо
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // хорошо
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // хорошо
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [17.2](#control-statements--value-selection) Не используйте операторы выбора вместо управляющих операторов.

    ```javascript
    // плохо
    !isRunning && startRunning();

    // хорошо
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="comments">Комментарии</a>

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) Используйте конструкцию `/** ... */` для многострочных комментариев.

    ```javascript
    // плохо
    // make() возвращает новый элемент
    // соответствующий переданному названию тега
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) Используйте двойной слеш `//` для однострочных комментариев. Располагайте такие комментарии отдельной строкой над кодом, который хотите пояснить. Если комментарий не является первой строкой блока, добавьте сверху пустую строку.

  Если нужно пояснить что-от кратко в большом блоке кода, то допускаются и комментарии после оператора.

    ```javascript
    // здесь не очень хорошо, нужно использовать только для кратких комментариев в большом блоке кода
    const active = true;  // это текущая вкладка

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    function getType() {
      console.log('fetching type...');
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // хорошо
    function getType() {
      console.log('fetching type...');

      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // тоже хорошо
    function getType() {
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) Начинайте все комментарии с пробела, так их проще читать. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // плохо
    //это текущая вкладка
    const active = true;

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    /**
     *make() возвращает новый элемент
     *соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) Если комментарий начинается со слов `FIXME` или `TODO`, то это помогает другим разработчикам быстро понять, когда вы хотите указать на проблему, которую надо решить, или когда вы предлагаете решение проблемы, которое надо реализовать. Такие комментарии, в отличие от обычных, побуждают к действию: `FIXME: -- нужно разобраться с этим` или `TODO: -- нужно реализовать`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Используйте `// FIXME:`, чтобы описать проблему.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: здесь не должна использоваться глобальная переменная
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) Используйте `// TODO:`, чтобы описать решение проблемы.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: нужна возможность задать total через параметры
        this.total = 0;
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="whitespace">Пробелы</a>

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) Используйте мягкую табуляцию (символ пробела) шириной в 2 пробела. eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // плохо
    function foo() {
    ∙∙∙∙let name;
    }

    // плохо
    function bar() {
    ∙let name;
    }

    // хорошо
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) Ставьте 1 пробел перед открывающей фигурной скобкой у блока. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // плохо
    function test(){
      console.log('test');
    }

    // хорошо
    function test() {
      console.log('test');
    }

    // плохо
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // хорошо
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) Ставьте 1 пробел перед открывающей круглой скобкой в операторах управления (`if`, `while` и т.п.). Не оставляйте пробелов между списком аргументов и названием в объявлениях и вызовах функций. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // плохо
    if(is_jedi) {
      fight ();
    }

    // хорошо
    if (is_jedi) {
      fight();
    }

    // плохо
    function fight () {
      console.log ('Swooosh!');
    }

    // хорошо
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) Разделяйте операторы пробелами. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // плохо
    const x=y+5;

    // хорошо
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) В конце файла оставляйте одну пустую строку. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // плохо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // плохо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // хорошо
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains) Используйте переносы строк и отступы, когда делаете длинные цепочки методов (больше 2 методов). Ставьте точку в начале строки, чтобы дать понять, что это не новая инструкция, а продолжение цепочки. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // плохо
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // плохо
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // хорошо
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // плохо
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // хорошо
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // хорошо
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) Оставляйте пустую строку между блоком кода и следующей инструкцией.

    ```javascript
    // плохо
    if (foo) {
      return bar;
    }
    return baz;

    // хорошо
    if (foo) {
      return bar;
    }

    return baz;

    // плохо
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // хорошо
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // плохо
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // хорошо
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) Не добавляйте отступы до или после кода внутри блока. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // плохо
    function bar() {

      console.log(foo);

    }

    // тоже плохо
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // хорошо
    function bar() {
      console.log(foo);
    }

    // хорошо
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a>
  - [19.9](#whitespace--no-multiple-blanks) Не используйте множество пустых строк для заполнения кода. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // плохо
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;


        this.email = email;


        this.setAge(birthday);
      }


      setAge(birthday) {
        const today = new Date();


        const age = this.getAge(today, birthday);


        this.age = age;
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // good
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;
        this.email = email;
        this.setAge(birthday);
      }

      setAge(birthday) {
        const today = new Date();
        const age = getAge(today, birthday);
        this.age = age;
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.10](#whitespace--in-parens) Не добавляйте пробелы между круглыми скобками и их содержимым. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // плохо
    function bar( foo ) {
      return foo;
    }

    // хорошо
    function bar(foo) {
      return foo;
    }

    // плохо
    if ( foo ) {
      console.log(foo);
    }

    // хорошо
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.11](#whitespace--in-brackets) Не добавляйте пробелы между квадратными скобками и их содержимым. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // плохо
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // хорошо
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.12](#whitespace--in-braces) Добавляйте пробелы между фигурными скобками и их содержимым. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // плохо
    const foo = {clark: 'kent'};

    // хорошо
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.13](#whitespace--max-len) Старайтесь не допускать, чтобы строки были длиннее 100 символов (включая пробелы). Замечание: согласно [пункту выше](#strings--line-length), длинные строки с текстом освобождаются от этого правила и не должны разбиваться на несколько строк. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Почему? Это обеспечивает удобство чтения и поддержки кода.

    ```javascript
    // плохо
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // плохо
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // хорошо
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // хорошо
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.14](#whitespace--block-spacing) Требуйте согласованного расстояния между открывающим символом блока и следующим символом на одной и той же строке. Тоже самое касается расстояния между закрывающим символом блока и предыдущим символом. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // плохо
    function foo() {return true;}
    if (foo) { bar = 0;}

    // хорошо
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) Избегайте пробелов перед запятыми и ставьте его после. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // плохо
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // хорошо
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) Избегайте пробелов внутри скобок вычисляемого свойства. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // плохо
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // хорошо
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) Избегайте пробелов между функциями и их вызовами. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // плохо
    func ();

    func
    ();

    // хорошо
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) Обеспечьте согласованное расстояние между ключами и значениями в свойствах литералов объекта. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // плохо
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // хорошо
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) Избегайте пробелов в конце строки. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) Избегайте множества пустых строк и новой строки в начале файлов. Разрешайте только одну пустую строку в конце файла. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // плохо - множество пустых строк
    var x = 1;


    var y = 2;

    // плохо - 2+ новых строк в конце файла
    var x = 1;
    var y = 2;


    // плохо - 1+ новая строка в начале файла

    var x = 1;
    var y = 2;

    // хорошо
    var x = 1;
    var y = 2;

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ к оглавлению](#Оглавление)**

## <a name="commas">Запятые</a>

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) Не начинайте строку с запятой. eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // плохо
    const story = [
        once
      , upon
      , aTime
    ];

    // хорошо
    const story = [
      once,
      upon,
      aTime,
    ];

    // плохо
    const hero = {
        first_name: 'Ada'
      , last_name: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // хорошо
    const hero = {
      first_name: 'Ada',
      last_name: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) Добавляйте висячие запятые. eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > Почему? Такой подход даёт понятную разницу при просмотре изменений. Кроме того, транспиляторы типа Babel удалят висячие запятые из собранного кода, поэтому вы можете не беспокоиться о [проблемах](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) в старых браузерах.

    ```diff
    // плохо - git diff без висячей запятой
    const hero = {
         first_name: 'Florence',
    -    last_name: 'Nightingale'
    +    last_name: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // хорошо - git diff с висячей запятой
    const hero = {
         first_name: 'Florence',
         last_name: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // плохо
    const hero = {
      first_name: 'Dana',
      last_name: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // хорошо
    const hero = {
      first_name: 'Dana',
      last_name: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // плохо
    function createHero(
      first_name,
      last_name,
      inventorOf
    ) {
      // ничего не делает
    }

    // хорошо
    function createHero(
      first_name,
      last_name,
      inventorOf,
    ) {
      // ничего не делает
    }

    // хорошо (обратите внимание, что висячей запятой не должно быть после rest-параметра)
    function createHero(
      first_name,
      last_name,
      inventorOf,
      ...heroArgs
    ) {
      // ничего не делает
    }

    // плохо
    createHero(
      first_name,
      last_name,
      inventorOf
    );

    // хорошо
    createHero(
      first_name,
      last_name,
      inventorOf,
    );

    // хорошо (обратите внимание, что висячей запятой не должно быть после rest-аргумента)
    createHero(
      first_name,
      last_name,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="semicolons">Точка с запятой</a>

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **Да.** eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > Почему? Когда JavaScript встречает перенос строки без точки с запятой, он использует правило под названием [Автоматическая Вставка Точки с запятой (Automatic Semicolon Insertion)](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion), чтобы определить, стоит ли считать этот перенос строки как конец выражения и (как следует из названия) поместить точку с запятой в вашем коде до переноса строки. Однако, ASI содержит несколько странных форм поведения, и ваш код может быть сломан, если JavaScript неверно истолкует ваш перенос строки. Эти правила станут сложнее, когда новые возможности станут частью JavaScript. Явное завершение ваших выражений и настройка вашего линтера для улавливания пропущенных точек с запятыми помогут вам предотвратить возникновение проблем.

    ```javascript
    // плохо - выбрасывает исключение
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = 'vader')

    // плохо - выбрасывает исключение
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // плохо - возвращает `undefined` вместо значения на следующей строке. Так всегда происходит, когда `return` расположен сам по себе, потому что ASI (Автоматическая Вставка Точки с запятой)!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // хорошо
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // хорошо
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
      }());

    // хорошо
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="type-casting--coercion">Приведение типов</a>

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) Выполняйте приведение типов в начале инструкции.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) Строки: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // плохо
    const totalScore = new String(this.reviewScore); // тип totalScore будет "object", а не "string"

    // плохо
    const totalScore = this.reviewScore + ''; // вызывает this.reviewScore.valueOf()

    // плохо
    const totalScore = this.reviewScore.toString(); // нет гарантии что вернётся строка

    // хорошо
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) Числа: Используйте `Number` и `parseInt` с основанием системы счисления. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4';

    // плохо
    const val = new Number(inputValue);

    // плохо
    const val = +inputValue;

    // плохо
    const val = inputValue >> 0;

    // плохо
    const val = parseInt(inputValue);

    // хорошо
    const val = Number(inputValue);

    // хорошо
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) Если по какой-то причине вы делаете что-то настолько безумное, что `parseInt` является слабым местом и вам нужно использовать побитовый сдвиг из-за [вопросов производительности](https://jsperf.com/coercion-vs-casting/3), оставьте комментарий, объясняющий почему и что вы делаете.

    ```javascript
    // хорошо
    /**
     * этот код медленно работал из-за parseInt.
     * побитовый сдвиг строки для приведения её к числу
     * работает значительно быстрее.
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **Примечание:** Будьте осторожны с побитовыми операциями. Числа в JavaScript являются [64-битными значениями](https://es5.github.io/#x4.3.19), но побитовые операции всегда возвращают 32-битные значения ([источник](https://es5.github.io/#x11.7)). Побитовый сдвиг может привести к неожиданному поведению для значений больше, чем 32 бита. [Обсуждение](https://github.com/airbnb/javascript/issues/109). Верхний предел — 2&nbsp;147&nbsp;483&nbsp;647:

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) Логические типы: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // плохо
    const hasAge = new Boolean(age);

    // хорошо
    const hasAge = Boolean(age);

    // отлично
    const hasAge = !!age;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="naming-conventions">Соглашение об именовании</a>

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) Избегайте названий из одной буквы. Имя должно быть наглядным. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // плохо
    function q() {
      // ...
    }

    // хорошо
    function query() {
      // ...
    }
    ```

 <a name="naming--descriptive"></a><a name="22.2"></a>
  - [23.2](#naming--descriptive) Для именования булевых переменных используйте имена, отвечающие на вопрос да\нет. Например, добавляя приставки is_, has_, was_ и т.п.

    ```javascript
    let is_activated;
    let has_new_items;
    ```

  <a name="naming--camelCase"></a><a name="22.3"></a>
  - [23.3](#naming--camelCase) Используйте `camelCase` для именования функций и методов. Используйте `snake_case` для именования переменных и свойств.

    ```javascript
    // плохо
    function destroy_this_ship() {}

    // хорошо
    function destroyThisShip() {}
    ```
    
  <a name="naming--camelCase"></a><a name="22.4"></a>
  - [23.4](#naming--snakeCase) Используйте `snake_case` для именования переменных и свойств.

    ```javascript
    // плохо
    const thisIsMyObject = {
        ammoLeft: 100,
    };

    // хорошо
    const this_is_my_object = {
        ammo_left: 100,
    };
    ```


  <a name="naming--PascalCase"></a><a name="22.5"></a>
  - [23.5](#naming--PascalCase) Используйте `PascalCase` только для именования конструкторов и классов. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // плохо
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // хорошо
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.6"></a>
  - [23.6](#naming--leading-underscore) Не используйте `_` в начале или в конце названий. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Почему? JavaScript не имеет концепции приватности свойств или методов. Хотя подчёркивание в начале имени является распространённым соглашением, которое показывает «приватность», фактически эти свойства являются такими же доступными, как и часть вашего публичного API. Это соглашение может привести к тому, что разработчики будут ошибочно думать, что изменения не приведут к поломке или что тесты не нужны. Итог: если вы хотите, чтобы что-то было «приватным», то оно не должно быть доступно извне.

    ```javascript
    // плохо
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // хорошо
    this.first_name = 'Panda';
    ```

  <a name="naming--self-this"></a><a name="22.7"></a>
  - [23.7](#naming--self-this) Не сохраняйте ссылку на `this`, где это возможно. Используйте стрелочные функции или [метод bind()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

    ```javascript
    // плохо
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // плохо
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // хорошо
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.8"></a>
  - [23.8](#naming--filename-matches-export) Название файла точно должно совпадать с именем его экспорта по умолчанию.

    ```javascript
    // содержание файла 1
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // содержание файла 2
    export default function fortyTwo() { return 42; }

    // содержание файла 3
    export default function insideDirectory() {}

    // в других файлах
    // плохо
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // плохо
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // хорошо
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ поддерживает оба варианта: insideDirectory.js и insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.9"></a>
  - [23.9](#naming--camelCase-default-export) Используйте `camelCase`, когда экспортируете функцию по умолчанию. Ваш файл должен называться так же, как и имя функции.
    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.10"></a>
  - [23.10](#naming--PascalCase-singleton) Используйте `PascalCase`, когда экспортируете конструктор / класс / синглтон / библиотечную функцию / объект.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="accessors">Аксессоры</a>

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) Функции-аксессоры для свойств объекта больше не нужны.

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) Не используйте геттеры/сеттеры, т.к. они вызывают неожиданные побочные эффекты, а также их тяжело тестировать, поддерживать и понимать. Вместо этого создавайте методы `getVal()` и `setVal('hello')`.

    ```javascript
    // плохо
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // хорошо
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) Если свойство/метод возвращает логический тип, то используйте названия `isVal()` или `hasVal()`.

    ```javascript
    // плохо
    if (!dragon.age()) {
      return false;
    }

    // хорошо
    if (!dragon.hasAge()) {
      return false;
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="standard-library">Стандартная библиотека</a>

  [Стандартная библиотека](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects)
  содержит утилиты, функциональность которых сломана, но они остались для поддержки старого кода.

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) Используйте `Number.isNaN` вместо глобального `isNaN`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Почему? Глобальная функция `isNaN` приводит не-числа к числам, возвращая `true` для всего, что приводится к `NaN`.
    > Если такое поведение необходимо, сделайте его явным.

    ```javascript
    // плохо
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // хорошо
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) Используйте `Number.isFinite` вместо глобального `isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Почему? Глобальная функция `isFinite` приводит не-числа к числам, возвращая `true` для всего, что приводится к конечному числу.
    > Если такое поведение необходимо, сделайте его явным.

    ```javascript
    // плохо
    isFinite('2e3'); // true

    // хорошо
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ к оглавлению](#Оглавление)**
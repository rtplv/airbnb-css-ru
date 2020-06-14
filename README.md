# Airbnb CSS / Sass Styleguide

*Наиболее разумный подход к написанию CSS и Sass*

## Навигация

1. [Терминология](#Терминология)
    - [Объявление правил](#Объявление-правил)
    - [Селекторы](#Селекторы)
    - [Свойства](#Свойства)
2. [CSS](#css)
    - [Форматирование](#Форматирование)
    - [Комментарии](#Комментарии)
    - [OOCSS и BEM](#oocss-и-bem)
    - [Селекторы id](#Селекторы-id)
    - [JavaScript хуки](#javascript-хуки)
    - [Свойство Border](#Свойство-border)
3. [Sass](#sass)
    - [Синтаксис](#Синтаксис)
    - [Сортировка свойств](#Сортировка-свойств)
    - [Переменные](#Переменные)
    - [Миксины](#Миксины)
    - [Директива @extend](#Директива-extend)
    - [Вложенные селекторы](#Вложенные-селекторы)
4. [Лицензия](#Лицензия)

## Терминология

### Объявление правил

Правило — это имя селектора (или группы селекторов) с определённой группой свойств.
Пример:
```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Селекторы

В объявлении правила «селекторы» означают элементы в DOM, к которым будут применены объявленные свойства. Селекторы могут соответствовать HTML-тэгам, классам элементов, id или атрибутам.
Пример использования селекторов:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Свойства

Наконец, свойства — это то, что описывает стиль выбранных элементов. Свойства указываются в виде `ключ: значение`. Правила могут содержать одно или более объявленных свойств.
Выглядит это так:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ К навигации](#Навигация)**

## CSS

### Форматирование

* Используйте мягкую табуляцию (2 пробела) для отступов.
* Kebab-case предпочтительнее camelCase в названии классов.
  - Snake_case и PascalCase можно использовать при работе с BEM (подробнее [OOCSS и BEM](#oocss-и-bem)).
* Не используйте селекторы #id.
* При использовании нескольких селекторов для одного правила каждое начинается с новой строки.
* Перед `{` ставим пробел в объявлении правила.
* В свойствах после `:` добавляем пробел.
* В конце объявления правила символ `}` должен переноситься на новую строку.
* Между объявлениями правил добавляйте пустую строку.

**Плохо**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Хорошо**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Комментарии

* Предпочитайте однострочные комментарии (`// в Sass`) многострочным.
* Комментарий должен находиться на отдельной строке. Избегайте комментариев в конце строки.
* Пишите детальные комментарии для неочевидных вещей:
  - для z-index;
  - CSS-хаков для отдельных браузеров и совместимости.

### OOCSS и BEM

Мы поощряем некоторые комбинации OOCSS и BEM, вот почему:

  * это позволяет создавать чистую и строгую связь между CSS и HTML;
  * это помогает нам создавать многоразовые, составные компоненты;
  * меньше вложенностей, низкая специфичность правил;
  * это помогает создавать масштабируемые таблицы стилей.

**OOCSS**, или «Объектно-ориентированный CSS» — это подход к написанию CSS, который призывает думать о таблице стилей как о коллекции «объектов»: многоразовых, повторяемых фрагментах кода, которые могут использоваться независимо друг от друга на всём сайте.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, или «Блок-Элемент-Модификатор» - это соглашение об именовании классов в HTML и CSS. Разработано Яндексом с прицелом на большие объёмы кода и масштабируемость. Может послужить как солидный набор правил для использования OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

Мы рекомендуем вариант БЭМ, в котором используются PascalCased «блоки», отлично работающие в связке с компонентами (например, React). Подчёркивания и тире по-прежнему используются для модификаторов и элементов.

**Пример**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

* `.ListingCard` — это «блок» и он представляет из себя компонент верхнего уровня.
* `.ListingCard__title` — это «элемент» и он является дочерним компонентом `.ListingCard`. Из «элементов» состоит «блок».
* `.ListingCard--featured` — это «модификатор» и он предствляет различные состояния «блока» `.ListingCard`.

### Селекторы id

Хоть в CSS и можно находить элемент по ID, это является плохой практикой. Селекторы ID вводят излишне высокий уровень [специфичности](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) для ваших правил и лишают возможности их переиспользовать.

Более подробная статья на эту тему: [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) 

### JavaScript-хуки

Избегайте привязки к одному классу одновременно в CSS и Javascript. Это может привести как минимум к трате времени при рефакторинге, а также боязни разработчика сломать функционал вводом новых изменений.

Мы рекомендуем использовать префикс `.js-`, для тех элементов, которые будут задействованы в Javascript:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Свойство Border

Вместо `none` используйте `0` для свойства border:

**Плохо**

```css
.foo {
  border: none;
}
```

**Хорошо**

```css
.foo {
  border: 0;
}
```
**[⬆ К навигации](#Навигация)**

## Sass

### Синтаксис

* Используйте синтаксис `.scss` вместо оригинального `.sass` синтаксиса.
* Упорядочивайте обычный CSS и `@include`-объявления логически.

### Сортировка свойств

1. Объявление свойств

    Перечисляйте все стандартные свойства, не являющиеся `@include` или вложенными селекторами.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. Объявление `@include`

    Группировка `@include` в конце правила делает код более читаемым.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Вложенные селекторы

Вложенные селекторы (_если они небходимы_) идут в самом конце, и ничего не должно идти после них. Добавляйте пустую строку между свойствами правила и вложенными селекторами, а также между смежными вложенными селекторами. Применяйте эти правила ко всем вложенным селекторам.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Переменные

Предпочитайте kebab-case именование переменных (напр. `$my-variable`) camelCase или snake_cased именованию. Допускается использовать знак нижнего подчеркивания для обозначения переменной, используемой в пределах одного файла (напр. `$_my-variable`).

### Миксины

Миксины делают ваш код более чистым и понятным и позволяют поддерживать абстрактную сложность, так же как и хорошо названные функции. Миксины, не принимающие аргументов, могут быть полезны для этого, но нужно помнить, что если вы не сжимаете свои файлы (например, gzip), то это может привести к лишнему повторению кода.

### Директива @extend

`@extend` следует избегать, поскольку он имеет неинтуитивное и потенциально опасное поведение, особенно при использовании с вложенными селекторами. Даже наследование селекторов верхнего уровня может создать проблемы, если в будущем будет изменён порядок селекторов. Сжатие компенсирует экономию, которую вы получили бы с помощью наследования.

### Вложенные селекторы

**Вложенность селекторов не должна быть больше 3!**

```scss
.page-container {
  .content {
    .profile {
      // Стоп!
    }
  }
}
```

Когда вы пишите такие длинные селекторы, вы, вероятно, пишите CSS, который:

* слишком сильно привязан к HTML (хрупкий); *—или—*
* чрезмерно специфичный; *—или—*
* не переиспользуемый.


Ещё раз: **никаких вложенных ID селекторов!**

Если всё-таки пришлось использовать селекторы ID (а этого на самом деле лучше избегать), они никогда не должны быть вложенными. Если такое происходит, вам требуется пересмотреть свою разметку и выяснить, зачем нужна такая сильная специфика. Если у вас правильно структурирован HTML-код и CSS-стили, вам **никогда** не придётся это делать.

**[⬆ К навигации](#Навигация)**

## Лицензия

(The MIT License)

Copyright (c) 2015 Airbnb

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ К навигации](#Навигация)**

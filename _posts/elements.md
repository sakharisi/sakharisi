---
layout: post
title: "Grid ელემენტები"
description: "Полный список свойств для создания раскладки на гридах и управления ею."
cover:
  author: kirakusto
  desktop: 'images/covers/desktop.svg'
  mobile: 'images/covers/mobile.svg'
  alt: 'Игра, похожая на тетрис, в виде гридов (сетки)'
authors:
  - solarrust
contributors:
  - corocoto
  - filimonovalexey
  - starhamster
  - skorobaeus
editors:
  - tachisis
keywords:
  - Grid Layout
  - грид
related:
  - css/flexbox-guide
  - css/has
  - css/media
tags:
  - article
---
## Свойства грид-элементов

Важное замечание: свойства `float`, `display: inline-block`, `display: table-cell`, `vertical-align` и `column-*` не дают никакого эффекта, когда применяются к грид-элементам.

### `grid-column-start`, `grid-column-end`, `grid-row-start`, `grid-row-end`

Определяют положение элемента внутри грид-сетки при помощи указания на конкретные направляющие линии.

Возможные значения:

- `название или номер линии` — может быть порядковым номером или названием конкретной линии.
- `span число` — элемент растянется на указанное количество ячеек.
- `span имя` — элемент будет растягиваться до следующей указанной линии.
- `auto` — означает автоматическое размещение, автоматический диапазон клеток или дефолтное растягивание элемента, равное одному.

```css
.container {
  display: grid;
  grid-template-columns: [one] 1fr [two] 1fr [three] 1fr [four] 1fr [five] 1fr [six];
  grid-template-rows: [row1-start] 1fr [row1-end] 1fr 1fr;
}

.item1 {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start;
  grid-row-end: 3;
}
```

![Пример реализации свойств grid-column-start, grid-column-end, grid-row-start, grid-row-end с первым вариантом значений.]({{ site.url }}/assets/images/30.png)

Элемент разместился по горизонтали от второй грид-линии до линии с названием `[five]`, а по вертикали — от линии с именем `[row1-start]` до линии под номером 3.

```css
.container {
  display: grid;
  grid-template-columns: [first] 1fr [line2] 1fr [line3] 1fr [col4-start] 1fr [five] 1fr [end];
  grid-template-rows: [row1-start] 1fr [row1-end] 1fr [third-line] 1fr [last-line];
}

.item1 {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```

![Пример реализации свойств grid-column-start, grid-column-end, grid-row-start, grid-row-end со вторым вариантом значений.]({{ site.url }}/assets/images/31.png)

Элемент расположился по вертикали от второй грид-линии и растянулся на две ячейки, а по горизонтали — от первой линии и растянулся до линии с названием `[col4-start]`.

Если не указать значения для свойств `grid-column-end` и `grid-row-end`, то элемент по умолчанию будет размером в одну грид-ячейку.

Элементы могут перекрывать друг друга, накладываться друг на друга. Можно использовать свойство `z-index` для управления контекстом наложения.

### `grid-column`, `grid-row`

Свойства-шорткаты для `grid-column-start`, `grid-column-end` и `grid-row-start`, `grid-row-end` соответственно.

Значения для `*-start` и `*-end` разделяются слэшем.

Можно использовать ключевое слово `span`, буквально говорящее «растянись на столько-то». А на сколько, указывает стоящая за ним цифра.

```css
.item1 {
  grid-column: 3 / span 2;
  grid-row: 3 / 4;
}
```

![Пример реализации свойств-шорткатов grid-column, grid-row.]({{ site.url }}/assets/images/32.png)

Элемент начинается с третьей линии по горизонтали и растягивается на 2 ячейки. По вертикали элемент начинается от третьей линии и заканчивается у четвёртой линии.

Если опустить слэш и второе значение, то элемент будет размером в одну ячейку.

### `grid-area`

Двуличное свойство 🧐, которое может как указывать элементу, какую из именованных областей ему нужно занять, так и служить шорткатом для одновременного указания значений для четырёх свойств: `grid-column-start`, `grid-column-end`, `grid-row-start` и `grid-row-end`.

Пример с указанием на именованную область:

```css
.item1 {
  /* Займёт область content внутри грид-сетки */
  grid-area: content;
}
```

А теперь пример с указанием всех четырёх значений в строку. При таком указании значений есть скользкое место: значения указываются в таком порядке: `row-start / column-start / row-end / column-end`, то есть сначала указываем оба начала, а потом оба конца.

```css
.item2 {
  grid-area: 1 / col4-start / last-line / 6;
}
```

### `justify-self`

С помощью этого свойства можно установить горизонтальное выравнивание для отдельного элемента, отличное от выравнивания, заданного грид-родителю.

Возможные значения аналогичны значениям свойства [`justify-items`](/css/grid-guide/#justify-items).

```css
.container {
  justify-items: stretch;
}

.item1 {
  justify-self: center;
}
```

![Пример реализации свойства justify-self.]({{ site.url }}/assets/images/33.png)

### `align-self`

А это свойство, как нетрудно догадаться, выравнивает отдельный элемент по вертикальной оси. Возможные значения аналогичны значениям свойства [`align-items`](/css/grid-guide/#align-items).

```css
.container {
  align-items: stretch;
}

.item1 {
  align-self: start;
}
```

![Пример реализации свойства align-self.]({{ site.url }}/assets/images/34.png)

### `place-self`

Шорткат для одновременного указания значений свойствам `justify-self` и `align-self`.

Возможные значения:

- `auto` (значение по умолчанию) — стандартное значение, можно использовать для сброса ранее заданных значений.
- `align-self justify-self` — первое значение задаёт значение свойству `align-self`, второе значение устанавливает значение свойства `justify-self`. Если указано всего одно значение, то оно устанавливается для обоих свойств. Например, `place-self: center` отцентрирует элемент по горизонтальной и по вертикальной осям одновременно.

## Специальные функции и ключевые слова

Когда вы задаёте размеры колонкам и рядам, вам доступны не только давно известные единицы измерения (`px`, `vw`, `rem`, `%` и так далее), но и очень крутые ключевые слова `min-content`, `max-content` и `auto`. И уже упомянутые единицы измерения `fr`.

Гриды подарили нам ещё одну волшебную функцию, позволяющую одновременно задавать минимальный и максимальный размер — `minmax()`. Например, в случае записи `grid-template-columns: minmax(200px, 1fr);` колонка займёт 1 часть свободного пространства грид-контейнера, но не меньше 200 пикселей.

Ещё одна полезная функция, появившаяся в гридах, это `repeat()`. Сэкономит вам кучу лишних букв и времени.

## Анимация свойств

Для анимации доступны следующие свойства и их значения:

- Значения свойств `gap`, `row-gap`, `column-gap`, указанные в единицах измерения, процентах или при помощи `calc()`.
- Значения свойств `grid-template-columns`, `grid-template-rows`, указанные в единицах измерения, процентах или при помощи функции `calc()` при условии, что анимируются аналогичные значения.

## Ссылки

1. [A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
1. [Grid Cheatsheet](https://yoksel.github.io/grid-cheatsheet/)
1. [Grid Garden](https://cssgridgarden.com/#ru)
1. [GRID cheat sheet](https://grid.malven.co/)
1. [Learning CSS Grid](https://learncssgrid.com/)
1. [Animating CSS Grid Layout properties](https://codepen.io/matuzo/post/animating-css-grid-layout-properties)

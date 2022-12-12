﻿# Lesson 2.

## Contents

1. [Chapter II](#chapter-ii) \
   1.1.[Vue](#vue) \
   1.2 [Устройство фреймворка](#устройство-фреймворка) \
   1.3 [Vue cli](#vue-cli) \
   1.4 [Структура Vue приложения](#структура-vue-приложения) \
   1.5 [Сравнение с React](#сравнение-с-react)

## Chapter I

Сегодня мы познакомимся с фреймворком Vue. Рассмотрим его основные концепции и проведем аналогии с React.

### Vue

Vue.js — это JavaScript-фреймворк, который создал разработчик Эван Ю. В 2012 году Эван работал в Google, где успел попробовать Backbone.js и Angular. Именно после этого он решил создать собственный фреймворк. На текущий момент актуальной версией является версия 3.х.

### Устройство фреймворка 

В основе Vue.js, так же как и React, лежат понятия компонента и состояния. Vue является реактивным фреймворком: он наблюдает за изменениями в модели и перерисовывает представление по необходимости. Эта функция делает управление состоянием Vue.js довольно простым и интуитивно понятным.

#### Экземпляр приложения и компоненты.

Каждое приложение Vue начинается с создания нового экземпляра приложения с помощью функции createApp.
Экземпляр приложения используется для регистрации каких-то глобальных вещей, которые затем будут использоваться компонентами внутри этого приложения

Пример компонента Vue:
```
// Создаём приложение Vue
const app = Vue.createApp({})

// Определяем новый глобальный компонент с именем button-counter
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button @click="count++">
      Счётчик кликов — {{ count }}
    </button>`
})
```
[Схема жизненных циклов](materials/vue_lifecycles.png)

В methods можно выделить следующие кастомные методы и методы жизненного цикла Vue: \
`-` beforeCreate — смотрит данные и инициализирует события. \
`-` created — смотрит, есть ли el или template. Если есть, то рендерит в них; если нет, то ищет метод render. \
`-` beforeMount — создает vm.$el и заменяет им el. \
`-` mounted — элемент отрендерен. 

При изменении состояния:\
`-` beforeUpdate — снова рендерит VDOM и сравнивает с реальным DOM-ом, применяет изменения. \
`-` updated — изменения отрендерены. \
`-` beforeDestroy — полный демонтаж вотчеров, внутренних компонентов и слушателей событий. \
`-` destroyed — вызывается, когда выполнение операции останавливается. 

Коммуникация между vue-компонентами осуществляется по принципу “Props in, Events out”. То есть от родительского элемента к дочернему информация передается через пропсы, а обратно — вызываются события.

#### Шаблоны

Vue.js использует синтаксис шаблонов, основанный на HTML. Он позволяет декларативно связывать отрисованный DOM с данными экземпляра компонента. Все шаблоны Vue.js являются валидным HTML-кодом, который могут распарсить все HTML-парсеры и браузеры, соответствующие спецификациям.

Наиболее простой способ связывания данных — текстовая интерполяция с использованием «Mustache»-синтаксиса (двойных фигурных скобок):

```
<span>Сообщение: {{ msg }}</span>
```

#### Директивы

Директивы — специальные атрибуты для добавления элементам html дополнительной функциональности.

Рассмотрим некоторые встроенные директивы: \
`-` V-bind — динамически связывается с одним или несколькими атрибутами. \
`-` V-cloak — прячет “усатые” выражения, пока не подтянулись данные. \
`-` v-if — условие для рендера элемента. \
`-` V-else — обозначает “else блок” для v-if. \
`-` V-for — циклично проходит массив объектов. \
`-` V-model — связывает состояние с input элементом. \
`-` V-on — связывает слушателя события с элементом. \
`-` V-once — рендерит элемент только вначале и больше не следит за ним. \
`-` V-pre — не компилирует элемент и его дочерние элементы. \
`-` V-show — переключает видимость элемента, изменяя свойство CSS display. \
`-` V-text — обновляет textContent элемента.

#### Переходы и анимации

Vue предоставляет некоторые абстракции, которые могут помочь в работе с переходами и анимациями, особенно в ответ на изменения чего-либо. Некоторые из них включают в себя: \
`-` Хуки для компонентов, добавляемых или удаляемых из DOM, как для CSS, так и для JS, с помощью встроенного компонента ```<transition>```. \
`-` Режимы перехода для возможности оркестрации порядка выполнения во время перехода. \
`-` Хуки для списка элементов, положение которых на странице изменяется, с поддержкой техники FLIP под капотом для увеличения производительности, с помощью компонента ```<transition-group>```. \
`-` Переходы между различными состояниями в приложении с помощью watchers.


#### Данные приложения 

При создании приложений во Vue вы обычно имеете дело с одним основным местом хранения данных. При создании экземпляра приложения можно определить объект data, который и будет источником истины.
```
const application = new Vue({ data: sourceOfTruth });
```

Также стоит отметить что на одной странице возможно использовать несколько экземпляров приложений.

Также для централизованного хранения данных в приложении существует библиотека [VueX](https://vue3js.cn/vuex/ru/)
Два момента отличают хранилище Vuex от простого глобального объекта: \
`-` Хранилище Vuex реактивно. Когда компоненты Vue полагаются на его состояние, то они будут реактивно и эффективно обновляться, если состояние хранилища изменяется. \
`-` Нельзя напрямую изменять состояние хранилища. Единственный способ внести изменения — явно вызвать мутацию. Это гарантирует, что любое изменение состояния оставляет след и позволяет использовать инструментарий, чтобы лучше понимать ход работы приложения.

### Vue cli
Vue предоставляет официальный CLI (opens new window)для быстрого создания каркаса одностраничных приложений (SPA). Предлагаемые шаблоны содержат всё необходимое для организации современной фронтенд-разработки.

```
vue create my-project
```

[Оффициальная документация](https://cli.vuejs.org/guide/)

### Структура Vue приложения

Что касается организации кода во Vue.js, то, безусловно, самым популярным подходом является создание однофайловых компонентов, где каждый компонент Vue.js — это самодостаточная переиспользуемая единица - файл с расширением .vue.
В целом организация файлов схожа с приложением на React.

[Структура директорий](materials/vue_folder.png)

Структура: \
`-` node_modues — папка с npm-пакетами. \
`-` public — папка содержит в себе общедоступные файлы, которые не попадут в сборку webpack — это файлы html, favicon и тд. \
`-` src — папка хранит в себе ресурсы проекта. \
`-` src/assets — папка для хранения каких то дополнительных ресурсов, обычно статичных — картинки, иконки, возможно стили. \
`-` src/components — папка содержит компоненты проекта. \
`-` src/router — папка для файлов, в которых определяются роуты приложения (Vue router). \
`-` src/store — папка для файлов, в которых определяется хранилище приложения (Vuex). \
`-` src/view —  папка для vue-компонентов, которые служат представлением для маршрутизации роутера. \
`-` src/App.vue — корневой компоненты приложения. \

### Сравнение с React

У React и Vue много общих черт:
`-` Они оба работают с Virtual DOM. \
`-` Имеют структуру основанную на компонентах. \
`-` Используют props для передачи данных. \
`-` Можно разрабатывать как на Javascript так и Typescript. \

Но также есть и ярко выраженные отличия:
`-` JSX в React и HTML шаблоны для отображения в Vue. \
`-` Так же следует следует упомянуть разницу в терминах. Vue - это прогрессивный JavaScript-фреймворк, а React - библиотека JavaScript с открытым исходным кодом. У каждой технологии есть свои сценарии использования. \
`-` У Vue преобладает количество решений, разработанных командой Vue, например VueX, vue-router и т.д, в то время как в React инструменты для похожих целей разрабатываются сообществом. \
 


### Project_01






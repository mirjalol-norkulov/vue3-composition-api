---
# try also 'default' to start simple
theme: seriph
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
background: /images/background.webp
colorSchema: dark
---

<div class="flex justify-center mb-3">
  <img src="/images/vue-logo.svg" class="w-24" />
</div>

# Vue 3 composition api

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: two-cols
---

# Options api nima?

Options api Vue 2 da qo'llaniladigan asosiy kod yozish stili

Dasturchilar Vue komponentga bir qancha option'larni berish orqali veb dasturlarni yaratishlari mumkin.

```js{all|3-7|8-15|all}
export default {
  name: 'Counter',
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    }
  }
}
```

::right::

<div class="h-full flex items-center justify-center">
  <Counter />
</div>
<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

---

# Options API da code reuse qilish usullari

- Mixins
- Renderless components
- Filters

Filterlar Vue 3 da mavjud emas.

---

# Mixin

<v-click>

```js
const myMixin = {
  created: function () {
    this.hello();
  },
  methods: {
    hello: function () {
      console.log("hello from mixin!");
    },
  },
};
```

</v-click>

<v-click>

```js
var Component = Vue.extend({
  mixins: [myMixin],
});
```

</v-click>

<v-click>

```js
var component = new Component(); // => "hello from mixin!"
```

</v-click>

---
layout: image-right
image: /images/mixin.webp
---

# Mixin'larning kamchiliklari

- Unclear source of properties
- Namespace'lar to'qnashuvi
- Mixin'lar aro aloqaning aniq va to'g'ridan to'g'ri emasligi

---
layout: two-cols
class: "p-2"
---

# Renderless components

```html
<FetchApi
  url="https://jsonplaceholder.typicode.com/todos/1"
  v-slot="{ isLoading, data }"
>
  <div v-if="isLoading">Loading...</div>
  <div>Todo: {{ data.title }}</div>
</FetchApi>
```

::right::

<div class="pt-18">
  <FetchApi 
    url="https://jsonplaceholder.typicode.com/todos/1" 
    v-slot="{ isLoading, data }">
    <div v-if="isLoading">Loading...</div>
    <div v-else>
      Todo: {{ data.title }}
    </div>
  </FetchApi>
</div>

---

# Composition API nima?

Composition API - componentda option'larni yozish o'rniga mavjud funksiyalarni import qilish va ulardan foydalanish. Bu funksiyalar composable'lar deyiladi.

Composition API quyidagilardan iborat:

- **Reactivity API:** `ref()`, `reactive()`, `computed()`, `watch()` va boshqa funksiyalar orqali reactive state hosil qilish va uni boshqarish.
- **Lifecycle hooks:** `onMounted()`, `onUnmounted()` va boshqa funksiyalar orqali Vue componentning lifecycle hooklaridan foydalanish.
- **Dependency injection:** `provide()`, `inject()`

---

# Composition API ga misol. setup option

```js{4,11|5|7-8|10|all}
import { ref } from "vue";

export default {
  setup() {
    const count = ref(0);

    const increment = () => count.value++;
    const decrement = () => count.value--;

    return { count, increment, decrement };
  },
};
```

<div v-click="5">

```html
<template>
  <div>
    <button @click="increment">-</button>
    <span>{{ count }}</span>
    <button @click="decrement">+</button>
  </div>
</template>
```

</div>


---

# ref

ref asosan primitive o'zgaruvchilarni reaktiv qilish uchun ishlatiladi. Ammo obyektlar uchun ham ref ishlatish mumkin.

ref funksiyasi argument sifatida biror qiymatni qabul qiladi va mutable ref obyekt qaytaradi. Bu obyektda yagona `.value` degan property mavjud bo'ladi. 

ref obyektning ichidagi `.value` property'sini o'zgartirishimiz mumkin va bu o'zgarish reaktiv bo'ladi.

```js
const count = ref(1);

count.value = 10;

const users = ref(['Ali', 'Bilol']);

users.value.push('Muhammad');
```

---

# reactive

reactive orqali biror bir tayyor obyektni reactive qilishimiz mumkin.

```js
const user = reactive({
  name: 'Ali',
  age: 30,
  gender: 'male'
});

user.name = 'Bilol';
user.age = 35;
```

---

# ref vs reactive

Quyidagi savollar tug'ilishi mumkin:

<v-clicks>

- Nega ref va reactive? Bitta refni o'zi yetarli emasmi?
- Nega ref ni `.value` qilib ishlatamiz, lekin reactive da to'g'ridan-to'g'ri ishlataveramiz?
- Bu ikkalasining farqi nimada va qachon qay birini ishlatish kerak?

</v-clicks>


---

# Composition vs Options API


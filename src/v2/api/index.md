---
type: api
---

## Cấu hình chung

`Vue.config` là một đối tượng chứa các cấu hình chung của Vue. Bạn có thể chỉnh sửa nó với các thuộc tính dưới đây trước khi khởi tạo ứng dụng:

### silent

- **Kiểu dữ liệu:** `boolean`

- **Giá trị mặc định:** `false`

- **Cách dùng:**

  ``` js
  Vue.config.silent = true
  ```

  Ẩn tất cả những lịch sử và cảnh báo của Vue.

### optionMergeStrategies

- **Kiểu dữ liệu:** `{ [key: string]: Function }`

- **Giá trị mặc định:** `{}`

- **Cách dùng:**

  ``` js
  Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {
    return child + 1
  }

  const Profile = Vue.extend({
    _my_option: 1
  })

  // Profile.options._my_option = 2
  ```

  Define custom merging strategies for options.

  The merge strategy receives the value of that option defined on the parent and child instances as the first and second arguments, respectively. The context Vue instance is passed as the third argument.

- **Xem thêm:** [Custom Option Merging Strategies](../guide/mixins.html#Custom-Option-Merge-Strategies)

### devtools

- **Kiểu dữ liệu:** `boolean`

- **Giá trị mặc định:** `true` (`false` khi cho ra sản phẩm)

- **Cách dùng:**

  ``` js
  // make sure to set this synchronously immediately after loading Vue
  Vue.config.devtools = true
  ```

  Định cấu hình cho phép dùng [vue-devtools](https://github.com/vuejs/vue-devtools) để kiểm tra. Giá trị mặc định là `true` giành cho nhà phát triển và `false` khi cho ra sản phẩm. Bạn có thể đặt `true` để cho phép hoạt động khi cho ra sản phẩm.

### errorHandler

- **Kiểu dữ liệu:** `Function`

- **Giá trị mặc định:** `undefined`

- **Cách dùng:**

  ``` js
  Vue.config.errorHandler = function (err, vm, type) {
    // Xử lý lỗi
    // `type` là một kiểu lỗi riêng biệt trong Vue, Ví dụ Sự kết nối trong Vòng đời của Vue
    // Lỗi được tìm thấy. Chỉ dành cho phiên bản 2.2.0+
  }
  ```

  Chỉ định một trình xử lý lỗi được khai báo trong quá trình render and watchers. Trình xử lý được gọi khi có lỗi và khi khởi tạo Vue.

  > Trong phiên bản 2.2.0, Tiến trình này cũng chụp lại những lỗi trong sự kết nối trong vòng đời Vue. Ngoài ra, khi tiến trinh kết nối là `undefined`, những hình ảnh lỗi đã được chụp lại sẽ được lưu lại với `console.error` thay cho việc dừng ứng dùng.

  > [Sentry](https://sentry.io), một dịch vụ theo dõi lỗi, cũng cấp [chính thức](https://sentry.io/for/vue/) để sử dụng tuỳ chọn này.

### ignoredElements

- **Kiểu dữ liệu:** `Array<string>`

- **Giá trị mặc định:** `[]`

- **Cách dùng:**

  ``` js
  Vue.config.ignoredElements = [
    'my-custom-web-component', 'another-web-component'
  ]
  ```

  Make Vue ignore custom elements defined outside of Vue (e.g., using the Web Components APIs). Otherwise, it will throw a warning about an `Unknown custom element`, assuming that you forgot to register a global component or misspelled a component name.

### keyCodes

- **Kiểu dữ liệu:** `{ [key: string]: number | Array<number> }`

- **Giá trị mặc định:** `{}`

- **Cách dùng:**

  ``` js
  Vue.config.keyCodes = {
    v: 86,
    f1: 112,
    mediaPlayPause: 179,
    up: [38, 87]
  }
  ```

  Định nghĩa một hoặc nhiều biến thay thế cho mã bàn phím được dùng trong v-on.

### performance

> Một chức năng mới trong phiên bản 2.2.0

- **Kiểu dữ liệu:** `boolean`

- **Giá trị mặc định:** `false`

- **Cách dùng**:

  Đặt giá trị này là `true` để kích hoạt component init, compile, render and patch để theo dõi hiệu năng trong browser devtool timeline. Chỉ làm việc khi phát triển và với những trình duyệt có hỗ trợ [performance.mark](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) API.

### productionTip

> Một chức năng mới trong phiên bản 2.2.0

- **Kiểu dữ liệu:** `boolean`

- **Giá trị mặc định:** `true`

- **Cách dùng**:

  Đặt giá trị `false` để dừng việc the production tip khi Vue bắt đầu chạy.

## API chung

<h3 id="Vue-extend">Vue.extend( options )</h3>

- **Tham số:**
  - `{Object} options`

- **Cách dùng:**

  Tạo một "subclass" trong cấu trúc nền của Vue. Những tham số nên là một đối tượng chưa các tuỳ chọn của component.

  Trường hợp đặc biệt cần lưu ý ở đây là tuỳ chọn `data` - Nó phải là một hàm (function) when được dùng với `Vue.extend()`.

  ``` html
  <div id="mount-point"></div>
  ```

  ``` js
  // Tạo cấu trúc
  var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
      return {
        firstName: 'Walter',
        lastName: 'White',
        alias: 'Heisenberg'
      }
    }
  })
  // Tạo một hàm khởi tạo cho Profile và gắn nó trên thành phần (element)
  new Profile().$mount('#mount-point')
  ```

  Kết quả sẽ là:

  ``` html
  <p>Walter White aka Heisenberg</p>
  ```

- **Xem thêm:** [Components](../guide/components.html)

<h3 id="Vue-nextTick">Vue.nextTick( [callback, context] )</h3>

- **Tham số:**
  - `{Function} [callback]`
  - `{Object} [context]`

- **Cách dùng:**

  Defer the callback to be executed after the next DOM update cycle. Use it immediately after you've changed some data to wait for the DOM update.

  ``` js
  // modify data
  vm.msg = 'Hello'
  // DOM not updated yet
  Vue.nextTick(function () {
    // DOM updated
  })
  ```

  > New in 2.1.0: returns a Promise if no callback is provided and Promise is supported in the execution environment.

- **Xem thêm:** [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)

<h3 id="Vue-set">Vue.set( object, key, value )</h3>

- **Tham số:**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Giá trị trả về:** giá trị được đặt.

- **Cách dùng:**

  Đặt thuộc tính cho một đối tượng. Nếu đối tượng động, chắc chắn thuộc tính được tạo cũng động và kích hoạt chế độ xem các cập nhập. Cái này chủ yếu được sử dụng để nhận xung quanh các giới hạn cái mà Vue không thể phát hiện các thuộc tính thêm.

  **Lưu ý đối tượng không thể là một Vue instance, hoặc một đối tượng dữ liệu gốc của Vue instance**

- **Xem thêm:** [Reactivity in Depth](../guide/reactivity.html)

<h3 id="Vue-delete">Vue.delete( target, key )</h3>

- **Tham số:**
  - `{Object | Array} target`
  - `{string | number} key`

- **Cách dùng:**

  Xoá một thuộc tính của đối tượng. Nếu là đối tượng động, phải chắc chắn việc xoá phải kích hoạt chế độ xem các cập nhập. Cái này chủ yếu được sử dụng để nhận xung quanh các giới hạn cái mà Vue không thể phát hiện thuộc tính được xoá, nhưng bạn nên hạn chế sử dụng nó.

  > Ngoài ra nó cũng hoạt động với Array + index trong phiên bản 2.2.0+.

  <p class="tip">Đối tượng đích không thể là một Vue instance, hoặc một đối tượng dữ liệu gốc của Vue instance.</p>

- **Xem thêm:** [Reactivity in Depth](../guide/reactivity.html)

<h3 id="Vue-directive">Vue.directive( id, [definition] )</h3>

- **Tham số:**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Cách dùng:**

  Đăng ký hoặc gán một directive chung.

  ``` js
  // Đăng ký
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })

  // đăng ký (hàm directive đơn giản)
  Vue.directive('my-directive', function () {
    // cái này sẽ được gọi như `bind` và `update`
  })

  // getter, trả về định nghĩa của directive nếu đã đăng ký
  var myDirective = Vue.directive('my-directive')
  ```

- **Xem thêm:** [Custom Directives](../guide/custom-directive.html)

<h3 id="Vue-filter">Vue.filter( id, [definition] )</h3>

- **Tham số:**
  - `{string} id`
  - `{Function} [definition]`

- **Cách dùng:**

  Đăng ký hoặc gán một filter chung.

  ``` js
  // đăng ký
  Vue.filter('my-filter', function (value) {
    // trả về giá trị đã được xử lý.
  })

  // getter, trả về filter nếu đã đăng ký
  var myFilter = Vue.filter('my-filter')
  ```

<h3 id="Vue-component">Vue.component( id, [definition] )</h3>

- **Tham số:**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Cách dùng:**

  Đăng ký hoặc gán một component chung. Việc đăng kí cũng tự động đặt `name` cho component với một `id`.

  ``` js
  // đăng ký một cấu trúc mở rộng
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // đăng ký một đối tượng tuỳ chọn (tự động gọi trong Vue.extend)
  Vue.component('my-component', { /* ... */ })

  // gán một component đã đăng kí (luôn luôn trả về một cấu trúc)
  var MyComponent = Vue.component('my-component')
  ```

- **Xem thêm:** [Components](../guide/components.html)

<h3 id="Vue-use">Vue.use( plugin )</h3>

- **Tham số:**
  - `{Object | Function} plugin`

- **Cách dùng:**

  Cài đặt một Vue.js plugin. Nếu plugin là một đối tượng, Nó phải được phải báo trong `install`. Nếu nó là một hàm chính nó, nó sẽ được xem như là install method. Install method sẽ được gọi với Vue như một đối số.

  Khi phương thức này được gọi trên cùng một lúc nhiều plugin, plugin sẽ được cài đặt một lần.

- **Xem thêm:** [Plugins](../guide/plugins.html)

<h3 id="Vue-mixin">Vue.mixin( mixin )</h3>

- **Tham số:**
  - `{Object} mixin`

- **Cách dùng:**

  Apply a mixin globally, which affects every Vue instance created afterwards. This can be used by plugin authors to inject custom behavior into components. **Not recommended in application code**.

- **Xem thêm:** [Global Mixins](../guide/mixins.html#Global-Mixin)

<h3 id="Vue-compile">Vue.compile( template )</h3>

- **Tham số:**
  - `{string} template`

- **Cách dùng:**

  Compiles a template string into a render function. **Only available in the standalone build.**

  ``` js
  var res = Vue.compile('<div><span>{{ msg }}</span></div>')

  new Vue({
    data: {
      msg: 'hello'
    },
    render: res.render,
    staticRenderFns: res.staticRenderFns
  })
  ```

- **Xem thêm:** [Render Functions](../guide/render-function.html)

<h3 id="Vue-version">Vue.version</h3>

- **Details**: Cung cấp phiên bản của Vue đã cài đặt. Cái này đặc biệt hữu dụng cho cộng đồng plugins và components, nơi mà bạn có thể sử dụng các chiến lược khác nhau cho các phiên bản khác nhau.

- **Cách dùng**:

```js
var version = Number(Vue.version.split('.')[0])

if (version === 2) {
  // Vue v2.x.x
} else if (version === 1) {
  // Vue v1.x.x
} else {
  // Không hỗ trợ phiên bản của Vue
}
```

## Tuỳ chọn / Dữ liệu

### data

- **Kiểu dữ liệu:** `Object | Function`

- **Restriction:** Chỉ cho phép `Function` khi định nghĩa một component.

- **Chi tiết:**

  The data object for the Vue instance. Vue will recursively convert its properties into getter/setters to make it "reactive". **The object must be plain**: native objects such as browser API objects and prototype properties are ignored. A rule of thumb is that data should just be data - it is not recommended to observe objects with its own stateful behavior.

  Once observed, you can no longer add reactive properties to the root data object. It is therefore recommended to declare all root-level reactive properties upfront, before creating the instance.

  After the instance is created, the original data object can be accessed as `vm.$data`. The Vue instance also proxies all the properties found on the data object, so `vm.a` will be equivalent to `vm.$data.a`.

  Properties that start with `_` or `$` will **not** be proxied on the Vue instance because they may conflict with Vue's internal properties and API methods. You will have to access them as `vm.$data._property`.

  When defining a **component**, `data` must be declared as a function that returns the initial data object, because there will be many instances created using the same definition. If we still use a plain object for `data`, that same object will be **shared by reference** across all instances created! By providing a `data` function, every time a new instance is created, we can simply call it to return a fresh copy of the initial data.

  If required, a deep clone of the original object can be obtained by passing `vm.$data` through `JSON.parse(JSON.stringify(...))`.

- **Example:**

  ``` js
  var data = { a: 1 }

  // direct instance creation
  var vm = new Vue({
    data: data
  })
  vm.a // -> 1
  vm.$data === data // -> true

  // must use function when in Vue.extend()
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

  <p class="tip">Note that __you should not use an arrow function with the `data` property__ (e.g. `data: () => { return { a: this.myProp }}`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.myProp` will be undefined.</p>

- **Xem thêm:** [Reactivity in Depth](../guide/reactivity.html)

### props

- **Kiểu dữ liệu:** `Array<string> | Object`

- **Chi tiết:**

  A list/hash of attributes that are exposed to accept data from the parent component. It has a simple Array-based syntax and an alternative Object-based syntax that allows advanced configurations such as type checking, custom validation and default values.

- **Example:**

  ``` js
  // simple syntax
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })

  // object syntax with validation
  Vue.component('props-demo-advanced', {
    props: {
      // just type check
      height: Number,
      // type check plus other validations
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
    }
  })
  ```

- **Xem thêm:** [Props](../guide/components.html#Props)

### propsData

- **Kiểu dữ liệu:** `{ [key: string]: any }`

- **Restriction:** only respected in instance creation via `new`.

- **Chi tiết:**

  Pass props to an instance during its creation. This is primarily intended to make unit testing easier.

- **Example:**

  ``` js
  var Comp = Vue.extend({
    props: ['msg'],
    template: '<div>{{ msg }}</div>'
  })

  var vm = new Comp({
    propsData: {
      msg: 'hello'
    }
  })
  ```

### computed

- **Kiểu dữ liệu:** `{ [key: string]: Function | { get: Function, set: Function } }`

- **Chi tiết:**

  Computed properties to be mixed into the Vue instance. All getters and setters have their `this` context automatically bound to the Vue instance.

  <p class="tip">Note that __you should not use an arrow function to define a computed property__ (e.g. `aDouble: () => this.a * 2`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be undefined.</p>

  Computed properties are cached, and only re-computed on reactive dependency changes. Note that if a certain dependency is out of the instance's scope (i.e. not reactive), the computed property will __not__ be updated.

- **Example:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // get only, just need a function
      aDouble: function () {
        return this.a * 2
      },
      // both get and set
      aPlus: {
        get: function () {
          return this.a + 1
        },
        set: function (v) {
          this.a = v - 1
        }
      }
    }
  })
  vm.aPlus   // -> 2
  vm.aPlus = 3
  vm.a       // -> 2
  vm.aDouble // -> 4
  ```

- **Xem thêm:**
  - [Computed Properties](../guide/computed.html)

### methods

- **Kiểu dữ liệu:** `{ [key: string]: Function }`

- **Chi tiết:**

  Methods to be mixed into the Vue instance. You can access these methods directly on the VM instance, or use them in directive expressions. All methods will have their `this` context automatically bound to the Vue instance.

  <p class="tip">Note that __you should not use an arrow function to define a method__ (e.g. `plus: () => this.a++`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be undefined.</p>

- **Example:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function () {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

- **Xem thêm:** [Methods and Event Handling](../guide/events.html)

### watch

- **Kiểu dữ liệu:** `{ [key: string]: string | Function | Object }`

- **Chi tiết:**

  An object where keys are expressions to watch and values are the corresponding callbacks. The value can also be a string of a method name, or an Object that contains additional options. The Vue instance will call `$watch()` for each entry in the object at instantiation.

- **Example:**

  ``` js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3
    },
    watch: {
      a: function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // string method name
      b: 'someMethod',
      // deep watcher
      c: {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      }
    }
  })
  vm.a = 2 // -> new: 2, old: 1
  ```

  <p class="tip">Note that __you should not use an arrow function to define a watcher__ (e.g. `searchQuery: newValue => this.updateAutocomplete(newValue)`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.updateAutocomplete` will be undefined.</p>

- **Xem thêm:** [Instance Methods - vm.$watch](#vm-watch)

## Options / DOM

### el

- **Kiểu dữ liệu:** `string | HTMLElement`

- **Restriction:** only respected in instance creation via `new`.

- **Chi tiết:**

  Provide the Vue instance an existing DOM element to mount on. It can be a CSS selector string or an actual HTMLElement.

  After the instance is mounted, the resolved element will be accessible as `vm.$el`.

  If this option is available at instantiation, the instance will immediately enter compilation; otherwise, the user will have to explicitly call `vm.$mount()` to manually start the compilation.

  <p class="tip">The provided element merely serves as a mounting point. Unlike in Vue 1.x, the mounted element will be replaced with Vue-generated DOM in all cases. It is therefore not recommended to mount the root instance to `<html>` or `<body>`.</p>

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### template

- **Kiểu dữ liệu:** `string`

- **Chi tiết:**

  A string template to be used as the markup for the Vue instance. The template will **replace** the mounted element. Any existing markup inside the mounted element will be ignored, unless content distribution slots are present in the template.

  If the string starts with `#` it will be used as a querySelector and use the selected element's innerHTML as the template string. This allows the use of the common `<script type="x-template">` trick to include templates.

  <p class="tip">From a security perspective, you should only use Vue templates that you can trust. Never use user-generated content as your template.</p>

- **Xem thêm:**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Content Distribution](../guide/components.html#Content-Distribution-with-Slots)

### render

  - **Kiểu dữ liệu:** `(createElement: () => VNode) => VNode`

  - **Chi tiết:**

    An alternative to string templates allowing you to leverage the full programmatic power of JavaScript. The render function receives a `createElement` method as it's first argument used to create `VNode`s.

    If the component is a functional component, the render function also receives an extra argument `context`, which provides access to contextual data since functional components are instance-less.

  - **Xem thêm:**
    - [Render Functions](../guide/render-function)

### renderError

> New in 2.2.0

  - **Kiểu dữ liệu:** `(createElement: () => VNode, error: Error) => VNode`

  - **Chi tiết:**

    **Only works in development mode.**

    Provide an alternative render output when the default `render` function encounters an error. The error will be passed to `renderError` as the second argument. This is particularly useful when used together with hot-reload.

  - **Example:**

    ``` js
    new Vue({
      render (h) {
        throw new Error('oops')
      },
      renderError (h, err) {
        return h('pre', { style: { color: 'red' }}, err.stack)
      }
    }).$mount('#app')
    ```

  - **Xem thêm:**
    - [Render Functions](../guide/render-function)

## Options / Lifecycle Hooks

All lifecycle hooks automatically have their `this` context bound to the instance, so that you can access data, computed properties, and methods. This means __you should not use an arrow function to define a lifecycle method__ (e.g. `created: () => this.fetchTodos()`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.fetchTodos` will be undefined.

### beforeCreate

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called synchronously after the instance has just been initialized, before data observation and event/watcher setup.

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### created

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called synchronously after the instance is created. At this stage, the instance has finished processing the options which means the following have been set up: data observation, computed properties, methods, watch/event callbacks. However, the mounting phase has not been started, and the `$el` property will not be available yet.

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeMount

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called right before the mounting begins: the `render` function is about to be called for the first time.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### mounted

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called after the instance has just been mounted where `el` is replaced by the newly created `vm.$el`. If the root instance is mounted to an in-document element, `vm.$el` will also be in-document when `mounted` is called.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeUpdate

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called when the data changes, before the virtual DOM is re-rendered and patched.

  You can perform further state changes in this hook and they will not trigger additional re-renders.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### updated

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called after a data change causes the virtual DOM to be re-rendered and patched.

  The component's DOM will have been updated when this hook is called, so you can perform DOM-dependent operations here. However, in most cases you should avoid changing state inside the hook. To react to state changes, it's usually better to use a [computed property](#computed) or [watcher](#watch) instead.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### activated

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called when a kept-alive component is activated.

  **This hook is not called during server-side rendering.**

- **Xem thêm:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### deactivated

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called when a kept-alive component is deactivated.

  **This hook is not called during server-side rendering.**

- **Xem thêm:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### beforeDestroy

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called right before a Vue instance is destroyed. At this stage the instance is still fully functional.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### destroyed

- **Kiểu dữ liệu:** `Function`

- **Chi tiết:**

  Called after a Vue instance has been destroyed. When this hook is called, all directives of the Vue instance have been unbound, all event listeners have been removed, and all child Vue instances have also been destroyed.

  **This hook is not called during server-side rendering.**

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

## Options / Assets

### directives

- **Kiểu dữ liệu:** `Object`

- **Chi tiết:**

  A hash of directives to be made available to the Vue instance.

- **Xem thêm:**
  - [Custom Directives](../guide/custom-directive.html)
  - [Assets Naming Convention](../guide/components.html#Assets-Naming-Convention)

### filters

- **Kiểu dữ liệu:** `Object`

- **Chi tiết:**

  A hash of filters to be made available to the Vue instance.

- **Xem thêm:**
  - [`Vue.filter`](#Vue-filter)

### components

- **Kiểu dữ liệu:** `Object`

- **Chi tiết:**

  A hash of components to be made available to the Vue instance.

- **Xem thêm:**
  - [Components](../guide/components.html)

## Options / Composition

### parent

- **Kiểu dữ liệu:** `Vue instance`

- **Chi tiết:**

  Specify the parent instance for the instance to be created. Establishes a parent-child relationship between the two. The parent will be accessible as `this.$parent` for the child, and the child will be pushed into the parent's `$children` array.

  <p class="tip">Use `$parent` and `$children` sparingly - they mostly serve as an escape-hatch. Prefer using props and events for parent-child communication.</p>

### mixins

- **Kiểu dữ liệu:** `Array<Object>`

- **Chi tiết:**

  The `mixins` option accepts an array of mixin objects. These mixin objects can contain instance options just like normal instance objects, and they will be merged against the eventual options using the same option merging logic in `Vue.extend()`. e.g. If your mixin contains a created hook and the component itself also has one, both functions will be called.

  Mixin hooks are called in the order they are provided, and called before the component's own hooks.

- **Example:**

  ``` js
  var mixin = {
    created: function () { console.log(1) }
  }
  var vm = new Vue({
    created: function () { console.log(2) },
    mixins: [mixin]
  })
  // -> 1
  // -> 2
  ```

- **Xem thêm:** [Mixins](../guide/mixins.html)

### extends

- **Kiểu dữ liệu:** `Object | Function`

- **Chi tiết:**

  Allows declaratively extending another component (could be either a plain options object or a constructor) without having to use `Vue.extend`. This is primarily intended to make it easier to extend between single file components.

  This is similar to `mixins`, the difference being that the component's own options takes higher priority than the source component being extended.

- **Example:**

  ``` js
  var CompA = { ... }

  // extend CompA without having to call Vue.extend on either
  var CompB = {
    extends: CompA,
    ...
  }
  ```

### provide / inject

> New in 2.2.0

- **Kiểu dữ liệu:**
  - **provide:** `Object | () => Object`
  - **inject:** `Array<string> | { [key: string]: string | Symbol }`

- **Chi tiết:**

  <p class="tip">`provide` and `inject` are primarily provided for advanced plugin / component library use cases. It is NOT recommended to use them in generic application code.</p>

  This pair of options are used together to allow an ancestor component to serve as a dependency injector for its all descendants, regardless of how deep the component hierarchy is, as long as they are in the same parent chain. If you are familiar with React, this is very similar to React's context feature.

  The `provide` option should be an object or a function that returns an object. This object contains the properties that are available for injection into its descendants. You can use ES2015 Symbols as keys in this object, but only in environments that natively support `Symbol` and `Reflect.ownKeys`.

  The `inject` options should be either an Array of strings or an object where the keys stand for the local binding name, and the value being the key (string or Symbol) to search for in available injections.

  > Note: the `provide` and `inject` bindings are NOT reactive. This is intentional. However, if you pass down an observed object, properties on that object do remain reactive.

- **Example:**

  ``` js
  var Provider = {
    provide: {
      foo: 'bar'
    },
    // ...
  }

  var Child = {
    inject: ['foo'],
    created () {
      console.log(this.foo) // -> "bar"
    }
    // ...
  }
  ```

  With ES2015 Symbols, function `provide` and object `inject`:
  ``` js
  const s = Symbol()

  const Provider = {
    provide () {
      return {
        [s]: 'foo'
      }
    }
  }

  const Child = {
    inject: { s },
    // ...
  }
  ```

## Options / Misc

### name

- **Kiểu dữ liệu:** `string`

- **Restriction:** only respected when used as a component option.

- **Chi tiết:**

  Allow the component to recursively invoke itself in its template. Note that when a component is registered globally with `Vue.component()`, the global ID is automatically set as its name.

  Another benefit of specifying a `name` option is debugging. Named components result in more helpful warning messages. Also, when inspecting an app in the [vue-devtools](https://github.com/vuejs/vue-devtools), unnamed components will show up as `<AnonymousComponent>`, which isn't very informative. By providing the `name` option, you will get a much more informative component tree.

### delimiters

- **Kiểu dữ liệu:** `Array<string>`

- **default:** `{% raw %}["{{", "}}"]{% endraw %}`

- **Chi tiết:**

  Change the plain text interpolation delimiters. **This option is only available in the standalone build.**

- **Example:**

  ``` js
  new Vue({
    delimiters: ['${', '}']
  })

  // Delimiters changed to ES6 template string style
  ```

### functional

- **Kiểu dữ liệu:** `boolean`

- **Chi tiết:**

  Causes a component to be stateless (no `data`) and instanceless (no `this` context). They are simply a `render` function that returns virtual nodes making them much cheaper to render.

- **Xem thêm:** [Functional Components](../guide/render-function.html#Functional-Components)

### model

> New in 2.2.0

- **Kiểu dữ liệu:** `{ prop?: string, event?: string }`

- **Chi tiết:**

  Allows a custom component to customize the prop and event used when it's used with `v-model`. By default, `v-model` on a component uses `value` as the prop and `input` as the event, but some input types such as checkboxes and radio buttons may want to use the `value` prop for a different purpose. Using the `model` option can avoid the conflict in such cases.

- **Example:**

  ``` js
  Vue.component('my-checkbox', {
    model: {
      prop: 'checked',
      event: 'change'
    },
    props: {
      // this allows using the `value` prop for a different purpose
      value: String
    },
    // ...
  })
  ```

  ``` html
  <my-checkbox v-model="foo" value="some value"></my-checkbox>
  ```

  The above will be equivalent to:

  ``` html
  <my-checkbox
    :checked="foo"
    @change="val => { foo = val }"
    value="some value">
  </my-checkbox>
  ```

## Instance Properties

### vm.$data

- **Kiểu dữ liệu:** `Object`

- **Chi tiết:**

  The data object that the Vue instance is observing. The Vue instance proxies access to the properties on its data object.

- **Xem thêm:** [Options - data](#data)

### vm.$props

> New in 2.2.0

- **Kiểu dữ liệu:** `Object`

- **Chi tiết:**

  An object representing the current props a component has received. The Vue instance proxies access to the properties on its props object.

### vm.$el

- **Kiểu dữ liệu:** `HTMLElement`

- **Chỉ đọc**

- **Chi tiết:**

  The root DOM element that the Vue instance is managing.

### vm.$options

- **Kiểu dữ liệu:** `Object`

- **Chỉ đọc**

- **Chi tiết:**

  The instantiation options used for the current Vue instance. This is useful when you want to include custom properties in the options:

  ``` js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // -> 'foo'
    }
  })
  ```

### vm.$parent

- **Kiểu dữ liệu:** `Vue instance`

- **Chỉ đọc**

- **Chi tiết:**

  The parent instance, if the current instance has one.

### vm.$root

- **Kiểu dữ liệu:** `Vue instance`

- **Chỉ đọc**

- **Chi tiết:**

  The root Vue instance of the current component tree. If the current instance has no parents this value will be itself.

### vm.$children

- **Kiểu dữ liệu:** `Array<Vue instance>`

- **Chỉ đọc**

- **Chi tiết:**

  The direct child components of the current instance. **Note there's no order guarantee for `$children`, and it is not reactive.** If you find yourself trying to use `$children` for data binding, consider using an Array and `v-for` to generate child components, and use the Array as the source of truth.

### vm.$slots

- **Kiểu dữ liệu:** `{ [name: string]: ?Array<VNode> }`

- **Chỉ đọc**

- **Chi tiết:**

  Used to programmatically access content [distributed by slots](../guide/components.html#Content-Distribution-with-Slots). Each [named slot](../guide/components.html#Named-Slots) has its own corresponding property (e.g. the contents of `slot="foo"` will be found at `vm.$slots.foo`). The `default` property contains any nodes not included in a named slot.

  Accessing `vm.$slots` is most useful when writing a component with a [render function](../guide/render-function.html).

- **Example:**

  ```html
  <blog-post>
    <h1 slot="header">
      About Me
    </h1>

    <p>Here's some page content, which will be included in vm.$slots.default, because it's not inside a named slot.</p>

    <p slot="footer">
      Copyright 2016 Evan You
    </p>

    <p>If I have some content down here, it will also be included in vm.$slots.default.</p>.
  </blog-post>
  ```

  ```js
  Vue.component('blog-post', {
    render: function (createElement) {
      var header = this.$slots.header
      var body   = this.$slots.default
      var footer = this.$slots.footer
      return createElement('div', [
        createElement('header', header),
        createElement('main', body),
        createElement('footer', footer)
      ])
    }
  })
  ```

- **Xem thêm:**
  - [`<slot>` Component](#slot-1)
  - [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)
  - [Render Functions: Slots](../guide/render-function.html#Slots)

### vm.$scopedSlots

> New in 2.1.0

- **Kiểu dữ liệu:** `{ [name: string]: props => VNode | Array<VNode> }`

- **Chỉ đọc**

- **Chi tiết:**

  Used to programmatically access [scoped slots](../guide/components.html#Scoped-Slots). For each slot, including the `default` one, the object contains a corresponding function that returns VNodes.

  Accessing `vm.$scopedSlots` is most useful when writing a component with a [render function](../guide/render-function.html).

- **Xem thêm:**
  - [`<slot>` Component](#slot-1)
  - [Scoped Slots](../guide/components.html#Scoped-Slots)
  - [Render Functions: Slots](../guide/render-function.html#Slots)

### vm.$refs

- **Kiểu dữ liệu:** `Object`

- **Chỉ đọc**

- **Chi tiết:**

  An object that holds child components that have `ref` registered.

- **Xem thêm:**
  - [Child Component Refs](../guide/components.html#Child-Component-Refs)
  - [ref](#ref)

### vm.$isServer

- **Kiểu dữ liệu:** `boolean`

- **Chỉ đọc**

- **Chi tiết:**

  Whether the current Vue instance is running on the server.

- **Xem thêm:** [Server-Side Rendering](../guide/ssr.html)

## Instance Methods / Data

<h3 id="vm-watch">vm.$watch( expOrFn, callback, [options] )</h3>

- **Tham số:**
  - `{string | Function} expOrFn`
  - `{Function} callback`
  - `{Object} [options]`
    - `{boolean} deep`
    - `{boolean} immediate`

- **Giá trị trả về:** `{Function} unwatch`

- **Cách dùng:**

  Watch an expression or a computed function on the Vue instance for changes. The callback gets called with the new value and the old value. The expression only accepts simple dot-delimited paths. For more complex expression, use a function instead.

<p class="tip">Note: when mutating (rather than replacing) an Object or an Array, the old value will be the same as new value because they reference the same Object/Array. Vue doesn't keep a copy of the pre-mutate value.</p>

- **Example:**

  ``` js
  // keypath
  vm.$watch('a.b.c', function (newVal, oldVal) {
    // do something
  })

  // function
  vm.$watch(
    function () {
      return this.a + this.b
    },
    function (newVal, oldVal) {
      // do something
    }
  )
  ```

  `vm.$watch` returns an unwatch function that stops firing the callback:

  ``` js
  var unwatch = vm.$watch('a', cb)
  // later, teardown the watcher
  unwatch()
  ```

- **Option: deep**

  To also detect nested value changes inside Objects, you need to pass in `deep: true` in the options argument. Note that you don't need to do so to listen for Array mutations.

  ``` js
  vm.$watch('someObject', callback, {
    deep: true
  })
  vm.someObject.nestedValue = 123
  // callback is fired
  ```

- **Option: immediate**

  Passing in `immediate: true` in the option will trigger the callback immediately with the current value of the expression:

  ``` js
  vm.$watch('a', callback, {
    immediate: true
  })
  // callback is fired immediately with current value of `a`
  ```

<h3 id="vm-set">vm.$set( object, key, value )</h3>

- **Tham số:**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Giá trị trả về:** the set value.

- **Cách dùng:**

  This is the **alias** of the global `Vue.set`.

- **Xem thêm:** [Vue.set](#Vue-set)

<h3 id="vm-delete">vm.$delete( object, key )</h3>

- **Tham số:**
  - `{Object} object`
  - `{string} key`

- **Cách dùng:**

  This is the **alias** of the global `Vue.delete`.

- **Xem thêm:** [Vue.delete](#Vue-delete)

## Instance Methods / Events

<h3 id="vm-on">vm.$on( event, callback )</h3>

- **Tham số:**
  - `{string | Array<string>} event` (array only supported in 2.2.0+)
  - `{Function} callback`

- **Cách dùng:**

  Listen for a custom event on the current vm. Events can be triggered by `vm.$emit`. The callback will receive all the additional arguments passed into these event-triggering methods.

- **Example:**

  ``` js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // -> "hi"
  ```

<h3 id="vm-once">vm.$once( event, callback )</h3>

- **Tham số:**
  - `{string} event`
  - `{Function} callback`

- **Cách dùng:**

  Listen for a custom event, but only once. The listener will be removed once it triggers for the first time.

<h3 id="vm-off">vm.$off( [event, callback] )</h3>

- **Tham số:**
  - `{string} [event]`
  - `{Function} [callback]`

- **Cách dùng:**

  Remove event listener(s).

  - If no arguments are provided, remove all event listeners;

  - If only the event is provided, remove all listeners for that event;

  - If both event and callback are given, remove the listener for that specific callback only.

<h3 id="vm-emit">vm.$emit( event, [...args] )</h3>

- **Tham số:**
  - `{string} event`
  - `[...args]`

  Trigger an event on the current instance. Any additional arguments will be passed into the listener's callback function.

## Instance Methods / Lifecycle

<h3 id="vm-mount">vm.$mount( [elementOrSelector] )</h3>

- **Tham số:**
  - `{Element | string} [elementOrSelector]`
  - `{boolean} [hydrating]`

- **Giá trị trả về:** `vm` - the instance itself

- **Cách dùng:**

  If a Vue instance didn't receive the `el` option at instantiation, it will be in "unmounted" state, without an associated DOM element. `vm.$mount()` can be used to manually start the mounting of an unmounted Vue instance.

  If `elementOrSelector` argument is not provided, the template will be rendered as an off-document element, and you will have to use native DOM API to insert it into the document yourself.

  The method returns the instance itself so you can chain other instance methods after it.

- **Example:**

  ``` js
  var MyComponent = Vue.extend({
    template: '<div>Hello!</div>'
  })

  // create and mount to #app (will replace #app)
  new MyComponent().$mount('#app')

  // the above is the same as:
  new MyComponent({ el: '#app' })

  // or, render off-document and append afterwards:
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)
  ```

- **Xem thêm:**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Server-Side Rendering](../guide/ssr.html)

<h3 id="vm-forceUpdate">vm.$forceUpdate()</h3>

- **Cách dùng:**

  Force the Vue instance to re-render. Note it does not affect all child components, only the instance itself and child components with inserted slot content.

<h3 id="vm-nextTick">vm.$nextTick( [callback] )</h3>

- **Tham số:**
  - `{Function} [callback]`

- **Cách dùng:**

  Defer the callback to be executed after the next DOM update cycle. Use it immediately after you've changed some data to wait for the DOM update. This is the same as the global `Vue.nextTick`, except that the callback's `this` context is automatically bound to the instance calling this method.

  > New in 2.1.0: returns a Promise if no callback is provided and Promise is supported in the execution environment.

- **Example:**

  ``` js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // modify data
        this.message = 'changed'
        // DOM is not updated yet
        this.$nextTick(function () {
          // DOM is now updated
          // `this` is bound to the current instance
          this.doSomethingElse()
        })
      }
    }
  })
  ```

- **Xem thêm:**
  - [Vue.nextTick](#Vue-nextTick)
  - [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)

<h3 id="vm-destroy">vm.$destroy()</h3>

- **Cách dùng:**

  Completely destroy a vm. Clean up its connections with other existing vms, unbind all its directives, turn off all event listeners.

  Triggers the `beforeDestroy` and `destroyed` hooks.

  <p class="tip">In normal use cases you shouldn't have to call this method yourself. Prefer controlling the lifecycle of child components in a data-driven fashion using `v-if` and `v-for`.</p>

- **Xem thêm:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

## Directives

### v-text

- **Expects:** `string`

- **Chi tiết:**

  Updates the element's `textContent`. If you need to update the part of `textContent`, you should use `{% raw %}{{ Mustache }}{% endraw %}` interpolations.

- **Example:**

  ```html
  <span v-text="msg"></span>
  <!-- same as -->
  <span>{{msg}}</span>
  ```

- **Xem thêm:** [Data Binding Syntax - interpolations](../guide/syntax.html#Text)

### v-html

- **Expects:** `string`

- **Chi tiết:**

  Updates the element's `innerHTML`. **Note that the contents are inserted as plain HTML - they will not be compiled as Vue templates**. If you find yourself trying to compose templates using `v-html`, try to rethink the solution by using components instead.

  <p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use `v-html` on trusted content and **never** on user-provided content.</p>

- **Example:**

  ```html
  <div v-html="html"></div>
  ```
- **Xem thêm:** [Data Binding Syntax - interpolations](../guide/syntax.html#Raw-HTML)

### v-show

- **Expects:** `any`

- **Cách dùng:**

  Toggle's the element's `display` CSS property based on the truthy-ness of the expression value.

  This directive triggers transitions when its condition changes.

- **Xem thêm:** [Conditional Rendering - v-show](../guide/conditional.html#v-show)

### v-if

- **Expects:** `any`

- **Cách dùng:**

  Conditionally render the element based on the truthy-ness of the expression value. The element and its contained directives / components are destroyed and re-constructed during toggles. If the element is a `<template>` element, its content will be extracted as the conditional block.

  This directive triggers transitions when its condition changes.

  <p class="tip">When used together with v-if, v-for has a higher priority than v-if. See the <a href="../guide/list.html#v-for-with-v-if">list rendering guide</a> for details.</p>

- **Xem thêm:** [Conditional Rendering - v-if](../guide/conditional.html)

### v-else

- **Does not expect expression**

- **Restriction:** previous sibling element must have `v-if` or `v-else-if`.

- **Cách dùng:**

  Denote the "else block" for `v-if` or a `v-if`/`v-else-if` chain.

  ```html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **Xem thêm:**
  - [Conditional Rendering - v-else](../guide/conditional.html#v-else)

### v-else-if

> New in 2.1.0

- **Expects:** `any`

- **Restriction:** previous sibling element must have `v-if` or `v-else-if`.

- **Cách dùng:**

  Denote the "else if block" for `v-if`. Can be chained.

  ```html
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- **Xem thêm:** [Conditional Rendering - v-else-if](../guide/conditional.html#v-else-if)

### v-for

- **Expects:** `Array | Object | number | string`

- **Cách dùng:**

  Render the element or template block multiple times based on the source data. The directive's value must use the special syntax `alias in expression` to provide an alias for the current element being iterated on:

  ``` html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatively, you can also specify an alias for the index (or the key if used on an Object):

  ``` html
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, key, index) in object"></div>
  ```

  The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder elements, you need to provide an ordering hint with the `key` special attribute:

  ``` html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  <p class="tip">When used together with v-if, v-for has a higher priority than v-if. See the <a href="../guide/list.html#v-for-with-v-if">list rendering guide</a> for details.</p>

  The detailed usage for `v-for` is explained in the guide section linked below.

- **Xem thêm:**
  - [List Rendering](../guide/list.html)
  - [key](../guide/list.html#key)

### v-on

- **Shorthand:** `@`

- **Expects:** `Function | Inline Statement`

- **Argument:** `event (required)`

- **Modifiers:**
  - `.stop` - call `event.stopPropagation()`.
  - `.prevent` - call `event.preventDefault()`.
  - `.capture` - add event listener in capture mode.
  - `.self` - only trigger handler if event was dispatched from this element.
  - `.{keyCode | keyAlias}` - only trigger handler on certain keys.
  - `.native` - listen for a native event on the root element of component.
  - `.once` - trigger handler at most once.
  - `.left` - (2.2.0) only trigger handler for left button mouse events.
  - `.right` - (2.2.0) only trigger handler for right button mouse events.
  - `.middle` - (2.2.0) only trigger handler for middle button mouse events.

- **Cách dùng:**

  Attaches an event listener to the element. The event type is denoted by the argument. The expression can either be a method name or an inline statement, or simply omitted when there are modifiers present.

  When used on a normal element, it listens to **native DOM events** only. When used on a custom element component, it also listens to **custom events** emitted on that child component.

  When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.

- **Example:**

  ```html
  <!-- method handler -->
  <button v-on:click="doThis"></button>

  <!-- inline statement -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>

  <!-- stop propagation -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <input @keyup.enter="onEnter">

  <!-- key modifier using keyCode -->
  <input @keyup.13="onEnter">

  <!-- the click event will be triggered at most once -->
  <button v-on:click.once="doThis"></button>
  ```

  Listening to custom events on a child component (the handler is called when "my-event" is emitted on the child):

  ```html
  <my-component @my-event="handleThis"></my-component>

  <!-- inline statement -->
  <my-component @my-event="handleThis(123, $event)"></my-component>

  <!-- native event on component -->
  <my-component @click.native="onClick"></my-component>
  ```

- **Xem thêm:**
  - [Methods and Event Handling](../guide/events.html)
  - [Components - Custom Events](../guide/components.html#Custom-Events)

### v-bind

- **Shorthand:** `:`

- **Expects:** `any (with argument) | Object (without argument)`

- **Argument:** `attrOrProp (optional)`

- **Modifiers:**
  - `.prop` - Bind as a DOM property instead of an attribute. ([what's the difference?](http://stackoverflow.com/questions/6003819/properties-and-attributes-in-html#answer-6004028))
  - `.camel` - transform the kebab-case attribute name into camelCase. (supported since 2.1.0)

- **Cách dùng:**

  Dynamically bind one or more attributes, or a component prop to an expression.

  When used to bind the `class` or `style` attribute, it supports additional value types such as Array or Objects. See linked guide section below for more details.

  When used for prop binding, the prop must be properly declared in the child component.

  When used without an argument, can be used to bind an object containing attribute name-value pairs. Note in this mode `class` and `style` does not support Array or Objects.

- **Example:**

  ```html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc">

  <!-- shorthand -->
  <img :src="imageSrc">

  <!-- with inline string concatenation -->
  <img :src="'/path/to/images/' + fileName">

  <!-- class binding -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">

  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- DOM attribute binding with prop modifier -->
  <div v-bind:text-content.prop="text"></div>

  <!-- prop binding. "prop" must be declared in my-component. -->
  <my-component :prop="someThing"></my-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  The `.camel` modifier allows camelizing a `v-bind` attribute name when using in-DOM templates, e.g. the SVG `viewBox` attribute:

  ``` html
  <svg :view-box.camel="viewBox"></svg>
  ```

  `.camel` is not needed if you are using string templates, or compiling with `vue-loader`/`vueify`.

- **Xem thêm:**
  - [Class and Style Bindings](../guide/class-and-style.html)
  - [Components - Component Props](../guide/components.html#Props)

### v-model

- **Expects:** varies based on value of form inputs element or output of components

- **Limited to:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **Modifiers:**
  - [`.lazy`](../guide/forms.html#lazy) - listen to `change` events instead of `input`
  - [`.number`](../guide/forms.html#number) - cast input string to numbers
  - [`.trim`](../guide/forms.html#trim) - trim input

- **Cách dùng:**

  Create a two-way binding on a form input element or a component. For detailed usage and other notes, see the Guide section linked below.

- **Xem thêm:**
  - [Form Input Bindings](../guide/forms.html)
  - [Components - Form Input Components using Custom Events](../guide/components.html#Form-Input-Components-using-Custom-Events)

### v-pre

- **Does not expect expression**

- **Cách dùng:**

  Skip compilation for this element and all its children. You can use this for displaying raw mustache tags. Skipping large numbers of nodes with no directives on them can also speed up compilation.

- **Example:**

  ```html
  <span v-pre>{{ this will not be compiled }}</span>
   ```

### v-cloak

- **Does not expect expression**

- **Cách dùng:**

  This directive will remain on the element until the associated Vue instance finishes compilation. Combined with CSS rules such as `[v-cloak] { display: none }`, this directive can be used to hide un-compiled mustache bindings until the Vue instance is ready.

- **Example:**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  The `<div>` will not be visible until the compilation is done.

### v-once

- **Does not expect expression**

- **Chi tiết:**

  Render the element and component **once** only. On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. This can be used to optimize update performance.

  ```html
  <!-- single element -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- the element have children -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- component -->
  <my-component v-once :comment="msg"></my-component>
  <!-- v-for directive -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

- **Xem thêm:**
  - [Data Binding Syntax - interpolations](../guide/syntax.html#Text)
  - [Components - Cheap Static Components with v-once](../guide/components.html#Cheap-Static-Components-with-v-once)

## Special Attributes

### key

- **Expects:** `string`

  The `key` special attribute is primarily used as a hint for Vue's virtual DOM algorithm to identify VNodes when diffing the new list of nodes against the old list. Without keys, Vue uses an algorithm that minimizes element movement and tries to patch/reuse elements of the same type in-place as much as possible. With keys, it will reorder elements based on the order change of keys, and elements with keys that are no longer present will always be removed/destroyed.

  Children of the same common parent must have **unique keys**. Duplicate keys will cause render errors.

  The most common use case is combined with `v-for`:

  ``` html
  <ul>
    <li v-for="item in items" :key="item.id">...</li>
  </ul>
  ```

  It can also be used to force replacement of an element/component instead of reusing it. This can be useful when you want to:

  - Properly trigger lifecycle hooks of a component
  - Trigger transitions

  For example:

  ``` html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  When `text` changes, the `<span>` will always be replaced instead of patched, so a transition will be triggered.

### ref

- **Expects:** `string`

  `ref` is used to register a reference to an element or a child component. The reference will be registered under the parent component's `$refs` object. If used on a plain DOM element, the reference will be that element; if used on a child component, the reference will be component instance:

  ``` html
  <!-- vm.$refs.p will be the DOM node -->
  <p ref="p">hello</p>

  <!-- vm.$refs.child will be the child comp instance -->
  <child-comp ref="child"></child-comp>
  ```

  When used on elements/components with `v-for`, the registered reference will be an Array containing DOM nodes or component instances.

  An important note about the ref registration timing: because the refs themselves are created as a result of the render function, you cannot access them on the initial render - they don't exist yet! `$refs` is also non-reactive, therefore you should not attempt to use it in templates for data-binding.

- **Xem thêm:** [Child Component Refs](../guide/components.html#Child-Component-Refs)

### slot

- **Expects:** `string`

  Used on content inserted into child components to indicate which named slot the content belongs to.

  For detailed usage, see the guide section linked below.

- **Xem thêm:** [Named Slots](../guide/components.html#Named-Slots)

## Built-In Components

### component

- **Props:**
  - `is` - string | ComponentDefinition | ComponentConstructor
  - `inline-template` - boolean

- **Cách dùng:**

  A "meta component" for rendering dynamic components. The actual component to render is determined by the `is` prop:

  ```html
  <!-- a dynamic component controlled by -->
  <!-- the `componentId` property on the vm -->
  <component :is="componentId"></component>

  <!-- can also render registered component or component passed as prop -->
  <component :is="$options.components.child"></component>
  ```

- **Xem thêm:** [Dynamic Components](../guide/components.html#Dynamic-Components)

### transition

- **Props:**
  - `name` - string, Used to automatically generate transition CSS class names. e.g. `name: 'fade'` will auto expand to `.fade-enter`, `.fade-enter-active`, etc. Defaults to `"v"`.
  - `appear` - boolean, Whether to apply transition on initial render. Defaults to `false`.
  - `css` - boolean, Whether to apply CSS transition classes. Defaults to `true`. If set to `false`, will only trigger JavaScript hooks registered via component events.
  - `type` - string, Specify the type of transition events to wait for to determine transition end timing. Available values are `"transition"` and `"animation"`. By default, it will automatically detect the type that has a longer duration.
  - `mode` - string, Controls the timing sequence of leaving/entering transitions. Available modes are `"out-in"` and `"in-out"`; defaults to simultaneous.
  - `enter-class` - string
  - `leave-class` - string
  - `appear-class` - string
  - `enter-to-class` - string
  - `leave-to-class` - string
  - `appear-to-class` - string
  - `enter-active-class` - string
  - `leave-active-class` - string
  - `appear-active-class` - string

- **Events:**
  - `before-enter`
  - `before-leave`
  - `before-appear`
  - `enter`
  - `leave`
  - `appear`
  - `after-enter`
  - `after-leave`
  - `after-appear`
  - `enter-cancelled`
  - `leave-cancelled` (`v-show` only)
  - `appear-cancelled`

- **Cách dùng:**

  `<transition>` serve as transition effects for **single** element/component. The `<transition>` does not render an extra DOM element, nor does it show up in the inspected component hierarchy. It simply applies the transition behavior to the wrapped content inside.

  ```html
  <!-- simple element -->
  <transition>
    <div v-if="ok">toggled content</div>
  </transition>

  <!-- dynamic component -->
  <transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </transition>

  <!-- event hooking -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete">
      <div v-show="ok">toggled content</div>
    </transition>
  </div>
  ```

  ``` js
  new Vue({
    ...
    methods: {
      transitionComplete: function (el) {
        // for passed 'el' that DOM element as the argument, something ...
      }
    }
    ...
  }).$mount('#transition-demo')
  ```

- **Xem thêm:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### transition-group

- **Props:**
  - `tag` - string, defaults to `span`.
  - `move-class` - overwrite CSS class applied during moving transition.
  - exposes the same props as `<transition>` except `mode`.

- **Events:**
  - exposes the same events as `<transition>`.

- **Cách dùng:**

  `<transition-group>` serve as transition effects for **multiple** elements/components. The `<transition-group>` renders a real DOM element. By default it renders a `<span>`, and you can configure what element is should render via the `tag` attribute.

  Note every child in a `<transition-group>` must be **uniquely keyed** for the animations to work properly.

  `<transition-group>` supports moving transitions via CSS transform. When a child's position on screen has changed after an updated, it will get applied a moving CSS class (auto generated from the `name` attribute or configured with the `move-class` attribute). If the CSS `transform` property is "transition-able" when the moving class is applied, the element will be smoothly animated to its destination using the [FLIP technique](https://aerotwist.com/blog/flip-your-animations/).

  ```html
  <transition-group tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </transition-group>
  ```

- **Xem thêm:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### keep-alive

- **Props:**
  - `include` - string or RegExp. Only components matched by this will be cached.
  - `exclude` - string or RegExp. Any component matched by this will not be cached.

- **Cách dùng:**

  When wrapped around a dynamic component, `<keep-alive>` caches the inactive component instances without destroying them. Similar to `<transition>`, `<keep-alive>` is an abstract component: it doesn't render a DOM element itself, and doesn't show up in the component parent chain.

  When a component is toggled inside `<keep-alive>`, its `activated` and `deactivated` lifecycle hooks will be invoked accordingly.

  > In 2.2.0 and above, `activated` and `deactivated` will fire for all nested components inside a `<keep-alive>` tree.

  Primarily used with preserve component state or avoid re-rendering.

  ```html
  <!-- basic -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>

  <!-- multiple conditional children -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>

  <!-- used together with <transition> -->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

- **`include` and `exclude`**

  > New in 2.1.0

  The `include` and `exclude` props allow components to be conditionally cached. Both props can either be a comma-delimited string or a RegExp:

  ``` html
  <!-- comma-delimited string -->
  <keep-alive include="a,b">
    <component :is="view"></component>
  </keep-alive>

  <!-- regex (use v-bind) -->
  <keep-alive :include="/a|b/">
    <component :is="view"></component>
  </keep-alive>
  ```

  The match is first checked on the component's own `name` option, then its local registration name (the key in the parent's `components` option) if the `name` option is not available. Anonymous components cannot be matched against.

  <p class="tip">`<keep-alive>` does not work with functional components because they do not have instances to be cached.</p>

- **Xem thêm:** [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### slot

- **Props:**
  - `name` - string, Used for named slot.

- **Cách dùng:**

  `<slot>` serve as content distribution outlets in component templates. `<slot>` itself will be replaced.

  For detailed usage, see the guide section linked below.

- **Xem thêm:** [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)

## VNode Interface

- Please refer to the [VNode class declaration](https://github.com/vuejs/vue/blob/dev/src/core/vdom/vnode.js).

## Server-Side Rendering

- Please refer to the [vue-server-renderer package documentation](https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer).

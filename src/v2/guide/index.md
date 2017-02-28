---
title: Giới thiệu
type: guide
order: 2
---

## Vue.js là gì?

Vue (đọc là /vjuː/, phát âm như **view**) là một **framework tân tiến** được dùng để xây dựng các giao diện người dùng. Không giống như các framework khác, Vue được thiết kế từ nền tảng chung sao cho người sử dụng có thể thích nghi dần dần. Cốt lõi của Vue là tập trung toàn bộ vào tầng View của hệ thống, và có thể dễ dàng tiếp nhận và tích hợp với các thư viện cũng như các dự án khác hiện có. Mặt khác, Vue là một sự lựa chọn hoàn hảo cho các ứng dụng web một trang (Single-Page Applications) khi kết hợp với [các công cụ hiện đại](single-file-components.html) và [các thư viện hỗ trợ](https://github.com/vuejs/awesome-vue#libraries--plugins).

Nếu bạn là một frontend developer có kinh nghiệm và muốn có một cái nhìn tổng quan, so sánh giữa Vue và các thư viện/frameworks khác, bạn có thể xem [So sánh với các Frameworks khác](comparison.html).


## Bắt đầu

<p class="tip">Hướng dẫn này được viết với yêu cầu khả năng tốt về HTML, CSS và JavaScript. Nếu bạn còn xa lạ với frontend, tìm cách sử dụng một framework ngay từ bước đầu của bạn không phải là ý hay! Hãy nắm chắc các kiến thức cơ bản trước. Kinh nghiệm sử dụng các framework khác sẽ rất tốt, nhưng không đòi hỏi.</p>

Cách đơn giản nhất để thử Vue.js là xem [Ví dụ JSFiddle Hello World](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Bạn có thể mở ví dụ này trong tab mới, đọc hướng dẫn này và làm theo các chỉ dẫn cơ bản. Hoặc là bạn có thể tự tạo một file `.html` và cài đặt Vue vào trang với: 

``` html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

Trang [Cài đặt](installation.html) cung cấp cho bạn thêm một số lựa chọn khác để cài đặt Vue. Xin lưu ý là chúng tôi **không** khuyến cáo những người nhập môn dùng `vue-cli`, đặt biệt là khi bạn chưa quen thao tác với các công cụ build và hỗ trợ của Node.js.

## Thể hiện theo các dữ liệu khai báo - Declarative Rendering

Cốt lõi của Vue.js là một hệ thống cho phép người viết thể hiện các dữ liệu theo như khai báo vào DOM, sử dụng các cú pháp mẫu - template syntax

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

Vậy là chúng ta đã tạo ra một ứng dụng Vue đơn giản đầu tiên! Đoạn code trên có thể nhìn giống như đang thể hiện chuỗi thông thường, nhưng Vue đã làm nhiều hơn thế. Dữ liệu này và DOM hiện đã liên kết với nhau, và tất cả mọi thứ liên quan đều được **linh động hóa**. Cách đơn giản nhất để tự chứng minh điều này và mở console của trang web này lên, và gán cho `app.message` một giá trị khác, bạn sẽ thấy ví dụ trên thay đổi theo đúng giá trị đó.

Không chỉ có thể diễn dịch các dữ liệu thành văn bản trong mẫu, chúng ta còn có thể ràng buộc thêm các thuộc tính như sau:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Hãy để con chuột vào đây trong vài giây và bạn sẽ nhìn thấy thuộc tính "title" được ràng buộc một cách linh động của tôi!
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Bạn đã tải trang này vào lúc ' + new Date()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Hãy để con chuột vào đây trong vài giây và bạn sẽ thấy thuộc tính "title" được ràng buộc một cách linh động của tôi!
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Bạn đã tải trang này vào lúc ' + new Date()
  }
})
</script>
{% endraw %}

Ở đây chúng ta có một từ khóa mới. Thuộc tính `v-bind` mà bạn thấy được ở đây được gọi là một **directive** (tạm dịch: chỉ đạo). Các directive đều được bắt đầu với `v-` để chỉ ra rằng chúng là những thuộc tính đặc biệt được cung cấp bởi Vue, và chắc là bạn cũng đã đoán ra được là, chúng cho phép DOM được sinh ra có thể có trạng thái linh động. Ở đây có thể tạm dịch ra ngôn ngữ người là "Hãy làm cho thuộc tính `title` của element này luôn luôn giống với thuộc tính `message` của đối tượng Vue của ứng dụng, bất kể thay đổi của `message` là thế nào."

Nếu bạn lại mở JavaScript console và nhập `app2.message = 'some new message'`, bạn sẽ lại thấy là đoạn HTML bị ràng buộc, trong trường hợp này là `title` - đã được sửa đổi.

## Diễn dịch theo điều kiện và vòng lập - Conditionals and Loops
Bật tắt sự hiện diện của một thành phần cũng khá là đơn giản:

``` html
<div id="app-3">
  <p v-if="seen">Bạn đang thấy tôi!</p>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Bạn đang thấy tôi!</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Bây giờ bạn hãy nhập `app3.seen = false` trong console, thành phần kia sẽ biến mất.

Ví dụ này cho thấy rằng chúng ta có thể ràng buộc dữ liệu không chỉ vào văn bản và thuộc tính, mà còn có thể và **cấu trúc** của DOM. Hơn nữa, Vue còn cung cấp một hệ thống [hiệu ứng chuyển đổi](transitions.html) để chúng ta có thể áp dụng vào khi phần tử được thêm vào/cập nhật/xóa bỏ bởi Vue.

Còn khá nhiều directive nữa mà bạn có thể sử dụng, mỗi directive đều có chức năng đặc biệt riêng. Ví dụ, directive `v-for` có thể được dùng để hiển thị một danh sách các phần tử dữ liệu từ một mảng:
``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
</script>
{% endraw %}

Trong console, nhập vào `app4.todos.push({ text: 'New item' })`. Bạn sẽ thấy thêm một phần tử dữ liệu đã được thêm vào danh sách.
## Xử lý từ các phương thức nhập của người dùng

Để người dùng có thể tương tác với ứng dụng của bạn, bạn có thể dùng directive `v-on` để gán listener cho một sự kiện gọi các phương thức trong đối tượng Vue thuộc ứng dụng của bạn: 
``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Đảo chuỗi</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Đảo chuỗi</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Hãy chú ý rằng, trong phương thức `reverseMessage`, chúng ta chỉ đơn giản là thay đổi thuộc tính `message` của ứng dụng (hay là trạng thái của ứng dụng) mà không hề đụng vào DOM - tất cả mọi hình thức điều khiển DOM đã được Vue xử lý, bạn chỉ cần phải quan tâm đến thuật toán của bạn.

Vue cũng cung cấp directive `v-model` để ràng buộc hai chiều giữa trạng thái ứng dụng và dữ liệu nhập từ input trở nên dễ dàng: 
``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Thiết lập từ các thành phần nhỏ hơn (Components)

Hệ thống các component - thành phần là một khái niệm quan trọng trong Vue, bởi vì nó là một cách trừu tượng hóa giúp chúng ta xây dựng những ứng dụng có quy mô lớn gồm các thành phần nhỏ, khép kín và thường được tái sử dụng nhiều lần. Nếu như nhìn nhận kỹ hơn, thì hầu như bất kỳ loại giao diện ứng dụng nào cũng có thể trừu tượng hóa thành một sơ đồ cây có các thành phần nhỏ:

![Component Tree](/images/components.png)

Trong Vue, một component cũng chỉ là một đối tượng Vue với những cài đặt đã được đinh hình trước. Thiết lập một component trong Vue khá đơn giản:

``` js
// Định nghĩa một component cho Vue
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

Bây giờ bạn đã có thể cài đặt component này vào mẫu - template của component khác
``` html
<ol>
  <!-- Tạo một đối tượng của todo-item -->
  <todo-item></todo-item>
</ol>
```
Nhưng, cũng như bạn có thể thấy, những đoạn code trên sẽ đơn giản là hiển thị cùng một văn bản cho mỗi todo, và mục đích của ứng dụng thường không phải là vậy. Chúng ta cần phải làm sao để chuyển được dữ liệu từ component cha sang các component con. Để đạt được điều đó, chúng ta sẽ phải sửa lại định hình của component một chút để cho nó chấp nhận một [prop](components.html#Props), hay gọi là thuộc tính chuyền vào:


``` js
Vue.component('todo-item', {
  //Bây giờ thì todo-item sẽ chấp nhận 'prop'
  //'prop' cũng giống như một thuộc tính
  //prop này có tên là 'todo'
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Bây giờ chúng ta có thể chuyển các "todo" vào trong các component `todo-item` được lập lại bằng `v-bind`:
``` html
<div id="app-7">
  <ol>
    <!-- Chúng ta gán cho từng todo-item với đối tượng todo mà chúng đại diện cho -->
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

Đây chỉ là một ví dụ đơn giản, nhưng điểm chính là chúng ta đã có thể phân tách ứng dụng ra thành nhiều phần tử nhỏ hơn, và component con được tách rời ra khỏi component cha một cách hợp lý bằng giao diện 'props'. Bây giờ chúng ta có thể nâng cấp component `<todo-item>` với một template và logic phức tạp hơn nữa mà không hề ảnh hưởng đến component cha.

Trong các ứng dụng quy mô lớn, chuyện tách rời thành các component là cực kỳ cần thiết để cho việc phát triển ứng dụng trở nên dễ quản lý. Chúng ta sẽ nói thêm về component [sau này](components.html), nhưng bạn có thê hình dung ra cấu trúc lớn của ứng dụng sử dụng component sẽ có thể giống thế này:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Mối quan hệ với các thành phần tự tạo (Custom Elements)

Bạn có thể đã để ý rằng, Vue components rất giống với các **Custom Elements - Các thành phần tự tạo**, một phần trong [Các đặc điểm kỹ thuật của Web Components](http://www.w3.org/wiki/WebComponents/). Đó là bởi vì hệ thống component của Vue được thiết kế dựa trên những đặc điểm kỹ thuật đó. Ví dụ, Slot Component của [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) và thuộc tính đặc biệt `is`. Nhưng giữa chúng vẫn có vài khác biệt nhỏ:

1. Web Components vẫn còn nằm trong giai đoạn thử nghiệm, và không phải browser nào cũng có hỗ trợ. Vue Components thì sẽ không cần sử dụng các thư viện hỗ trợ đa nền tảng và sẽ hoạt động trên tất cả các browser mà Vue hỗ trợ (từ IE9 trở lên). Khi cần thiết thì Vue component vẫn có thể được bọc trong một thành phần tự tạo.

2. Vue components cung cấp những tính năng quan trọng mà thành phần tự tạo thông thường không thể có, dễ chú ý nhất là dòng chuyển tiếp dữ liệu (như đã ví dụ ở trên) giữa các component, các event tự tạo và sự kết hợp giữa các công cụ hỗ trợ. 

## Bạn đã sẵn sàng để tiếp tục chưa?

Chúng tôi chỉ vừa mới giới thiệu những tính năng cơ bản nhất của Vue.js mà thôi! Những tính năng và thành phần cao cấp hơn sẽ được giới thiệu và hướng dẫn chi tiết hơn trong hướng dẫn sử dụng này!
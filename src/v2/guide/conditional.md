---
title: Biên dịch theo điều kiện
type: guide
order: 7
---

## `v-if`

Trong các khai báo biểu mẫu, như là Handlebars chả hạn, thì chúng ta thường sẽ viết khối điều kiện như sau:
``` html
<!-- Handlebars template -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```

Trong Vue, chúng ta có thể sử dụng directive `v-if` tương tự:

``` html
<h1 v-if="ok">Yes</h1>
```

Chúng ta cũng có thể viết một khối "else" bằng cách sử dụng `v-else`:

``` html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

### Khối điều kiện với `v-if` trong tag `<template>`


Bởi vì `v-if` là một directive, nó phải được gắn vào một tag cụ thể nào đó. Nhưng nếu như có trường hợp bạn muốn bật/tắt nhiều hơn 1 tag thì sao? Trong trường hợp đó chúng ta có thể dùng `v-if` vào trong tag `<template>`, lúc đó tag `<template>` sẽ là một tag bao vô hình, vì kết quả cuối cùng sẽ không có tag `<template>` có `v-if` đó.

``` html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### `v-else`

Như đã nói trên, chúng ta có thể dùng `v-else` để tạo khối "else" sau `v-if`:

``` html
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

Tag có directive `v-else` phải ngay lập tức theo sau một tag có directive `v-if` hoặc `v-else-if`, nếu không thì trình biên dịch sẽ không nhận.

### `v-else-if`

> Directive mới từ bản 2.1.0

Directive `v-else-if`, đúng như tên gọi của nó, là một khối "else if" cho `v-if`. Khối này có thể được dùng liên tiếp nhiều lần: 
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

Tương tự như `v-else`, một khối `v-else-if` phải được đặt ngay sau `v-if` hoặc `v-else-if`.

### Điều khiển các thành phần có thể tái sử dụng với `key`

Vue sẽ cố gắng biên dịch các thành phần hiệu quả nhất có thể, thường sẽ tái sử dụng chúng thay vì biên dịch lại từ đầu. Ngoài tác dụng tăng tốc độ của ứng dụng, chuyện này cũng đem lại nhiều lợi thế khác. Ví dụ, nếu bạn cho phép người đùng thay đổi qua lại các kiểu login khác nhau:

``` html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

Thì thay đổi `loginType` trong đoạn code phía trên SẼ KHÔNG xoá bỏ dữ liệu đã nhập vào của người dùng. Khi mà cả 2 biểu mẫu đều dùng chung các tag giống nhau, tag `<input>` sẽ không hề được thay thế, chỉ có `placeholder` của nó mới được thay thế mà thôi.


Bạn hãy tự kiểm chứng bằng cách nhập text vào input dưới đây rồi nhấn nút toggle:

{% raw %}
<div id="no-key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address">
    </template>
  </div>
  <button @click="toggleLoginType">Toggle login type</button>
</div>
<script>
new Vue({
  el: '#no-key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Tất nhiên không phải lúc nào chuyện này cũng cần thiết, vì thế nên Vue sẽ cho phép bạn ra lệnh : "Hai phần tử này khác nhau hoàn toàn - đừng tái sử dũng nó". Hãy thêm một thuộc tính `key` với giá trị đặc biệt vào.

``` html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

Bây giờ thì các phần tử input đó sẽ được khởi tạo từ đầu mỗi khi thay đổi giá trị `loginType`:

{% raw %}
<div id="key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username" key="username-input">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address" key="email-input">
    </template>
  </div>
  <button @click="toggleLoginType">Toggle login type</button>
</div>
<script>
new Vue({
  el: '#key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Note that the `<label>` elements are still efficiently re-used, because they don't have `key` attributes.
Hãy chú ý rằng tag `<label>` vẫn được tái sử dụng, vì chúng không có thuộc tính `key`.
## `v-show`

Một lựa chọn khác để hiển thị block theo điều kiện là directive `v-show`. Cách sử dụng cũng gần tương tự:

``` html
<h1 v-show="ok">Hello!</h1>
```

Sự khác biệt là, `v-if` chỉ biên dịch khi đúng điều kiện đặt ra, còn `v-show` chắn chắn sẽ biên dịch và thành phần kia luôn có mặt trong DOM, và khi sai điều kiện, `v-show` chỉ thay đổi thuộc tính CSS `display` của thành phần thành `none`.

<p class="tip">Hãy lưu ý rằng `v-show` không thể dùng với `<template>` hoặc `v-else`.</p>

## `v-if` và `v-show`

`v-if` là kiểu biên dịch theo điều kiện thật sự vì nó sẽ bảo đảm rằng các sự kiện liên quan, cùng với các thành phần con sẽ được phá bỏ hoàn toàn và được tái tạo lại khi điều kiện thay đổi.

`v-if` cũng mang tính chất **khởi tạo trễ**, nếu như ngay từ đầu mà điều kiện ràng buộc đã sai, thì nó sẽ chẳng làm gì cả, khối sẽ không được khởi tạo cho tới khi điều kiện được thoả mãn lần đầu tiên.

Trong khi đó thì `v-show` lại đơn giản hơn, phần tử luôn luôn được tạo mà không cần biết điều kiện kia đúng hay sai, chỉ đơn giản là thay đổi thuộc tính CSS của nó.

Vậy tổng thể, `v-if` sẽ nặng về thay đổi DOM giữa chừng còn `v-show` sẽ nặng hơn khi khởi tạo.
Vì vậy, nên dùng `v-show` khi bạn cần thay đổi một điều kiện nào đó rất thường xuyên, và nên dùng `v-if` khi mà điều kiện ràng buộc hiếm khi thay đổi trong lúc chạy (runtime).

## `v-if` dùng chung với `v-for`

Khi sử dụng chung `v-if` và `v-for` trên cùng 1 tag, `v-for` sẽ có độ ưu tiên cao hơn `v-if`, hãy xem [biên dịch danh sách](../guide/list.html#V-for-and-v-if) để biết thêm chi tiết.

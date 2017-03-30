---
title: Ràng buộc Class và Style 
type: guide
order: 6
---

Một nhu cầu quen thuộc khi chúng ta sử dụng các ràng buộc dữ liệu là điều khiển được Class và Style inline của các element trong DOM. Cả hai thứ đó đều là thuộc tính HTML, chúng ta có thể dùng `v-bind` để ràng buộc chúng, và chúng ta chỉ cần phải xuất ra một "chuỗi kết quả" cuối cùng bằng các biểu thức. Tuy nhiên, can thiệp vào việc cắt nối chuỗi là một việc rắc rối và dễ phát sinh lỗi. Vì thế mà Vue cung cấp những cải tiến đặc biệt cho `v-bind` khi nó được dùng cho `class` và `style`: chúng có thể được ràng buộc dưới dạng Object - đối tượng hoặc Array - mảng.


## Ràng buộc các Class HTML

### Theo kiểu Object

Chúng ta có thể truyền một object vào `v-bind:class` để chuyển đổi class một cách linh động:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Cú pháp nêu trên có nghĩa là: class `active` sẽ chỏ có khi `isActive` là true, ở đây, class được quyết định bởi [tính đúng sai](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) của dữ liệu.

Bạn có thể có nhiều class bằng cách có nhiều trường hơn trong object mà bạn gởi vào. Hơn nữa, `v-bind:class` cũng có thể tồn tại đồng thời với thuộc tính `class` thông thường. Với biểu mẫu như sau:

``` html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

Và dữ liệu như sau:

``` js
data: {
  isActive: true,
  hasError: false
}
```

Nó sẽ cho ra:
``` html
<div class="static active"></div>
```

Điều này cũng có nghĩa là khi `isActive` hoặc `hasError` thay đổi, danh sách class của tag cũng sẽ biến đổi. Ví dụ, nếu `hasError` trở thành `true`, danh sách class sẽ biến thành `"static active text-danger"`.

Đối tượng chuyền vào cũng không cần phải viết trực tiếp:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Và chuyện đó cũng cho ra kết quả tương tự. Chúng ta cũng có thể ràng buộc với một [thuộc tính computed](computed.html) có trả về object. Đây là một kiểu rất hay được sử dụng:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### Theo kiểu Array

Chúng ta có thể chuyền một array vào `v-bind:class` để áp một danh sách class:

``` html
<div v-bind:class="[activeClass, errorClass]">
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Và nó sẽ cho ra:
``` html
<div class="active text-danger"></div>
```

Nếu như bạn muốn chuyển đổi một class một cách linh động, bạn luôn có thể áp dụng biểu thức 3 chiều:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```

Đoạn code trên có nghĩa là: `v-bind` sẽ luôn áp `errorClass`, nhưng chỉ áp dụng `activeClass` khi `isActive` là `true`.

Tuy nhiên, điều này có thể hơi rườm rà nếu như bạn có quá nhiều ràng buộc class theo điều kiện như thế. Đó là lý do vì sao mà bạn cũng có thể kết hợp object trong array để chuyền vào:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### Với các thành phần - Components

> Phần này yêu cầu kiến thức cơ bản về [Vue Components - thành phần con](components.html). Nếu bạn muốn bạn có thể bỏ qua và trở lại sau.

Khi bạn dùng thuộc tính `class` trong thành phần con của bạn tạo ra, những class đó sẽ được thêm vào trong tag gốc của thành phần. Những class đã có này sẽ không bị viết đè lên.

Đơn cử, khi bạn khai báo như thế này:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
Và lại thêm class khi sử dụng thành phần như thế này:

``` html
<my-component class="baz boo"></my-component>
```

Thì kết quả HTML của bạn sẽ như thế này:

``` html
<p class="foo bar baz boo">Hi</p>
```

Tương tự với `v-bind:class`:


``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

Khi `isActive` là true, kết quả HTML của bạn sẽ là:

``` html
<p class="foo bar active">Hi</p>
```

## Ràng buộc Styles Inline

### Theo kiểu Object

Ràng buộc kiểu object cho `v-bind:style` khá đơn giản - nhìn gần giống như CSS, nhưng thật ra là Object của JavaScript. Bạn có thể dùng kiểu camelCase hoặc kebab-case (với kebab-case thì dùng dấu nháy) cho tên thuộc tính CSS:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Nhưng tốt hơn là nên ràng buộc một object thực sự để cho biểu mẫu dễ đọc:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Một lần nữa, ràng buộc kiểu Object có thể và thường được sử dụng chung với computed properties có hàm trả về object.

### Theo kiểu Array

Ràng buộc kiểu Array cho `v-bind:style` cho phép bạn áp dụng nhiều object style cho cùng một tag:

``` html
<div v-bind:style="[baseStyles, overridingStyles]">
```

### Tự động thêm tiền tố

Khi bạn dùng một thuộc tính CSS mà cần tiền tố của nhà cung cấp trong `v-bind:style`, như `transform` chả hạn, thì Vue sẽ tự động thêm vào cho bạn.
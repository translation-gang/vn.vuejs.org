---
title: Biểu mẫu - Template Syntax
type: guide
order: 4
---

Vue.js sử dụng các template syntax dựa trên nền tảng căn bản của HTML, cho phép bạn ràng buộc các thuộc tính của đối tượng Vue vào DOM. Tất cả template đều là HTML hợp lệ và có thể được đọc bởi tất cả các browser được hỗ trợ.

Ở bên dưới, Vue sẽ chịu trách nhiệm biên dịch template thành các function sinh Virtual DOM. Kết hợp với hệ thống phản ứng, Vue sẽ có khả năng nhận biết được số lượng component tối thiểu để tái sử dụng và điều khiển DOm khi trạng thái của ứng dụng thay đổi.

Nếu bạn có quen thuộc với khái niệm Virtual DOM - DOM ảo, và lại thích sử dụng tối đa chức năng của JavaScript, thì bạn cũng có thể [tự viết các hàm render](render-function.html) thay vì viết các biểu mẫu, với hỗ trợ JSX (không bắt buộc).

## Interpolations - Các phép nối gán

### Text - Văn bản

Dạng cơ bản nhất của ràng buộc dữ liệu là nối gán văn bản sử dụng cú pháp "Mustache" quen thuộc (dấu ngoặc nhọn đôi):

``` html
<span>Message: {{ msg }}</span>
```

Dấu "Mustache" trên sẽ được thay thế bằng dữ liệu của thuộc tính `msg` trong đối tượng Vue chủ. Nó cũng sẽ được thay đổi theo mỗi khi `msg` thay đổi.

Bạn cũng có thể cho nó gán chỉ một lần duy nhất, và khi làm thế, nội dung của dấu "Mustache" sẽ không thay đổi nữa, đó là dùng [v-once](../api/#v-once), nhưng lưu ý rằng khi dùng thế này thì tất cả các ràng buộc trong cùng tag html cũng sẽ ảnh hưởng:

``` html
<span v-once>This will never change: {{ msg }}</span>
```

### Raw HTML - HTML thuần

Cú pháp "Mustache" chỉ xuất ra dữ liệu dưới dạng văn bản thuần túy, chứ không phải HTML. Để có thể xuất ra được HTML, bạn cần phải dùng directive `v-html`:

``` html
<div v-html="rawHtml"></div>
```
Ở ví dụ trên, `rawHhtml` chính là nguồn dữ liệu của thuộc tính
The contents are inserted as plain HTML - data bindings are ignored. Note that you cannot use `v-html` to compose template partials, because Vue is not a string-based templating engine. Instead, components are preferred as the fundamental unit for UI reuse and composition.

Nội dung được truyền tải dưới dạng HTML thuần, tất cả các ràng buộc dữ liệu sẽ bị bỏ qua. Chú ý rằng bạn sẽ không thể sử dụng `v-html` để có thể xuất ra các biểu mẫu template theo component bên trong nó. Thay vào đó, các thành phần component con thường được dùng riêng lẻ nhiều hơn để tăng tính tái sử dụng.

<p class="tip">Diễn dịch các đoạn HTML linh động trên trang web của bạn sẽ làm tăng nguy cơ bị tấn công qua [điểm yếu XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Chỉ nên dùng thao tác này đối với những nội dung mà bạn tin tưởng và *không bao giờ* áp dụng cho nhưng thông tin người dùng nhập vào</p>

### Attributes

Cú pháp "Mustache" cũng không thể sử dụng được trong thao tác tạo HTML này, thay vào đó hãy dùng [v-bind directive](../api/#v-bind):

``` html
<div v-bind:id="dynamicId"></div>
```

Directive này cũng có thể dùng cho các thuộc tính tag HTML kiểu boolean - thuộc tính đó sẽ bị loại bỏ nếu như điều kiện ràng buộc là false:
``` html
<button v-bind:disabled="someDynamicCondition">Button</button>
```

### Sử dụng cách thể hiện của JavaScript


Cho tới lúc này chúng ta chỉ mới ràng buộc đơn giản vào trong biểu mẫu của chúng ta. Nhưng Vue.js cũng hỗ trợ đầy đủ các biểu thức của JavaScript trong ràng buộc:

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Các biểu thức này sẽ được tính toán như JavaScript trong tầm dữ liệu của đối tượng Vue. Quy định là mỗi ràng buộc dữ liệu như vậy chỉ có **một biểu thức** thôi, vì vậy, những trường hợp sau đây là sai và sẽ **KHÔNG** hoạt động
``` html
<!-- đây là khai báo, không phải biểu thức -->
{{ var a = 1 }}

<!-- cấu trúc rẽ nhánh cũng không được, hãy sử dụng biểu thức điều kiện a?b:c-->
{{ if (ok) { return message } }}
```

<p class="tip">Các biểu thức trong biểu mẫu đã được gói gọn và chỉ có thể truy nhập vào những thành phần toàn cục đã có trong whitelist như `Math` và `Date`. Bạn không nên thử truy cập vào những globals do người dùng tạo ra ở khai báo biểu mẫu.</p>

## Directives - Chỉ thị


Các chỉ thị - directive là những thuộc tính HTML đặc biệt với mở đầu là `v-`. Các thuộc tính directive này nhận **một biểu thức JavaScript** (trường hợp ngoại lệ là `v-for`, sẽ được nói đến sau). Công việc của một directive là áp dụng các hiệu ứng phụ vào DOM một cách linh hoạt khi giá trị của biểu thức gán cho nó thay đổi. Hãy xem lại các ví dụ sau đây:

``` html
<p v-if="seen">Now you see me</p>
```

Ở đây , directive `v-if` sẽ có nhiệm vụ thêm/bỏ tag `<p>` dựa vào biến logic `seen`.

### Arguments - Đối số

Có một số directive có thể nhận các "argument", được biểu thị bởi một dấu 2 chấm sau tên của directive. Ví dụ `v-bind` được dùng để thay đổi một thuộc tính tag HTML linh động:


``` html
<a v-bind:href="url"></a>
```

Ở đây `href` là đối số, và `v-bind` có nhiệm vụ thay đổi thuộc tính `href` này theo giá trị của biểu thức `url`, hoặc giá trị của thuộc tính Vue `url`.

Một ví dụ khác là directive `v-on`, đặt một listener vào các sự kiện DOM:

``` html
<a v-on:click="doSomething">
```

Ở đây đối số sẽ là sự kiện cần được lắng nghe. Chúng ta sẽ nói về nắm bắt sự kiện chi tiết hơn về sau.

### Modifiers - Hiệu chỉnh

Các Modifiers - Các hiệu chỉnh là các từ hậu tố, đặt sau một dấu chấm, bảo rằng directive đó sẽ được ràng buộc vào DOM một cách đặc biệt. Ví dụ, hiệu chỉnh `.prevent` sẽ bảo directive `v-on` chạy `event.preventDefault()` khi event của DOM được kích hoạt:

``` html
<form v-on:submit.prevent="onSubmit"></form>
```

Chúng ta sẽ được biết về các Modifiers này nhiều hơn khi chúng ta xem về `v-on` và `v-model`.

## Filters - Bộ lọc

Vue.js cho phép bạn định nghĩa các `filters` - bộ lọc có thể được áp dũng cho các định dạng văn bản thường gặp. Filter có thể dùng được ở 2 chỗ: **cú pháp "Mustache" và biểu thức của directive `v-bind`**. Các filter phải được áp vào cuối các biểu thức JavaScript, sau một dấu xổ dọc:

``` html
<!-- mustaches -->
{{ message | capitalize }}

<!-- v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

<p class="tip">Các filter trong Vue 2.x chỉ có thể được dùng trong 2 trường hợp : cú pháp "Mustache" và `v-bind` (`v-bind` được hỗ trợ sau 2.1.0), vì filters được tạo ra chủ yếu là cho việc biến đổi văn bản. Để có được các biến đổi và lọc phức tạp hơn, bạn nên sử dụng [Các thuộc tính Computed](computed.html)</p>

Một hàm filter luôn luôn nhận giá trị của biểu thức là tham chiếu đầu tiên.

``` js
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

Có thể có nhiều filter liên tục nhau:

``` html
{{ message | filterA | filterB }}
```

Filters are JavaScript functions, therefore they can take arguments:
Filter cũng là hàm của JavaScript, vì vậy chúng cũng có thể có thêm tham chiếu

``` html
{{ message | filterA('arg1', arg2) }}
```

Ở đây, thì chuỗi 'arg1' sẽ là tham chiếu thứ hai, và arg2 sẽ là tham chiếu thứ 3

## Shorthands - Viết tắt

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building an [SPA](https://en.wikipedia.org/wiki/Single-page_application) where Vue.js manages every template. Therefore, Vue.js provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

Tiền tố `v-` cho phép nhận biết đâu là thuộc tính HTML do Vue.js sinh ra trong biểu mẫu của bạn. Điều này rất hữu dụng khi bạn dùng Vue để điều khiển các trạng thái của DOM bằng các tag HTML sẵn có, nhưng  đôi lúc, có một số directives lại bị sử dụng quá nhiều khiến cho việc đọc và tu sửa trở nên khó khăn. Hơn nữa, độ cần thiết của tiền tố `v-` sẽ trở nên giảm dần đi khi mà bạn đang xây dựng một [ứng dụng 1 trang](https://en.wikipedia.org/wiki/Single-page_application) với Vue.js là nền tảng quản lý tất cả. Vì vậy, Vue.js sẽ cho phép bạn viết tắt 2 directive thường dùng nhất là `v-bind` và `v-on`

### Viết tắt của `v-bind`

``` html
<!-- đầy đủ -->
<a v-bind:href="url"></a>

<!-- viết tắt -->
<a :href="url"></a>
```


### Viết tắt của `v-on`

``` html
<!-- đầy đủ -->
<a v-on:click="doSomething"></a>

<!-- viết tắt -->
<a @click="doSomething"></a>
```

Chúng có thể nhìn hơi khác với HTML thường dùng, nhưng `:` và `@` là những ký tự hợp lệ cho thuộc tính HTML và bất kỳ trình duyệt nào được Vue.js hỗ trợ cũng có thể đọc được nó. Thêm vào đó, chúng cũng sẽ không xuất hiện khi trình duyệt đã sinh ra DOM. Bãn không nhất thiết phải dùng kiểu viết tắt này, nhưng bạn sẽ thấy chúng hữu dụng khi bạn dùng nhiều.
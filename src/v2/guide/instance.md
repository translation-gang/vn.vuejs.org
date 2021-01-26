---
title: Đối tượng Vue
type: guide
order: 3
---

## Constructor

Mọi Vue vm đều được tải và khởi tạo bằng cách tạo ra **một đối tượng Vue gốc** với hàm constructor:
``` js
var vm = new Vue({
  // options
})
```

Mặc dù không hoàn toàn theo mô hình framework [MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel), Một phần thiết kế của Vue được lấy cảm hứng từ mô hình này, vì thế mà chúng tôi hay sử dụng từ `vm` (viết tắt của View Model) để gán cho các đối tượng Vue được khai báo.

Khi bạn khởi tạo một đối tượng Vue, bạn cần gán cho nó  **options object - đối tượng cài đặt** ,trong đó có chứa những cài đặt về `data` - dữ liệu, element - phần tử HTML mà đối tượng Vue được cài đặt vào, `methods` - các phương thức của đối tượng Vue này, chu kỳ sống của đối tượng Vue, v.v. . Bạn có thể đọc danh sách đầy đủ các tùy chỉnh tại [tài liệu API](../api).

Constructor `Vue` cũng có thể được mở rộng - `extend` - để tạo ra các component con với cài đặt sẵn có thể tái sử dụng:
``` js
var MyComponent = Vue.extend({
  // cài đặt tùy chỉnh phần mở rộng
})

// tất cả đối tượng thuộc lớp `MyComponent` được tạo bởi
// các cài đặt tùy chỉnh trên
var myComponentInstance = new MyComponent()
```

Mặc dù có tạo ra một đối tượng mở rộng của Vue một cách đơn lẻ vô điều kiện, nhưng hầu hết trường hợp chúng ta nên ràng buộc chúng với nhưng elements tự tạo và với templates. Chúng ta sẽ nói chi tiết về [hệ thống component](components.html) sau. Bây giờ, bạn chỉ cần biết rằng tất các các component của Vue về mặt căn bản đều được mở rộng từ đối tượng Vue.

## Thuộc tính và Phương Thức

Mỗi đối tượng Vue đều **đại diện** cho tất cả các thuộc tính được tìm thấy trong đối tượng con `data` của nó:

``` js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> true

// biến đổi thuộc tính sẽ ảnh hưởng tới dữ liệu gốc
vm.a = 2
data.a // -> 2

// ... và ngược lại
data.a = 3
vm.a // -> 3
```

Cần lưu ý là chỉ có những thuộc tính đại diện này mới có phản ứng tới view. Nếu như bạn cài đặt một thuộc tính mới SAU KHI đối tượng đã được khởi tạo, nó sẽ không gây ra bất kỳ một sự cập nhật nào ở View. Chúng ta sẽ nói chi tiết về những phản ứng này sau.

Ngoài thuộc tính `data`, đối tượng Vue cũng có thêm các cài đặt hữu dụng khác về thuộc tính và phương thức. Những thuộc tính và phương thức này được mở đầu với `$` để phân biệt chúng với các dữ liệu 'đại diện' trên. Ví dụ:

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true

// $watch là một phương thức callback
vm.$watch('a', function (newVal, oldVal) {
  // phương thức này sẽ được gọi khi 'a' thay đổi
})
```

<p class="tip">Don't use [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) on an instance property or callback (e.g. `vm.$watch('a', newVal => this.myMethod())`). As arrow functions are bound to the parent context, `this` will not be the Vue instance as you'd expect and `this.myMethod` will be undefined.</p>

<p class="tip">Đừng sử dụng [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) trong các phương thức đặt biệt hay callback (e.g. `vm.$watch('a', newVal => this.myMethod())`). Vì arrow functions đã bị ràng buộc với đối tượng gốc, từ khóa `this` sẽ không mang giá trị đối tượng Vue hiện tại này như bạn mong muốn và `this.myMethod` sẽ bị lỗi undefined.</p>

Hãy đọc thêm về [API reference](../api) để có được danh sách đầy đủ các đối tượng, phương thức và thuộc tính.

## Các móc nối với chu kỳ sống của đối tượng


Mỗi đối tượng Vue đều đi qua một quá trình chuẩn bị khi nó vừa được khởi tạo - lấy ví dụ, đối tượng này cần cài đặt lắng nghe biến đổi dữ liệu, điền lại biểu mẫu, đè biểu mẫu đó lại vào DOM, và vập nhật lại DOM khi dữ liệu có thay đổi. Trong những giai đoạn đó, nó cũng sẽ cho ra những **móc nối đến chu kỳ sống**, và đây là những lúc mà chúng ta có thể đưa các thuật toán và xử lý vào. Chẳng hạn như [`created`](../api/#created) sẽ được gọi sau khi đối tượng được khởi tạo:

``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` là đối tượng Vue, hay biến vm lúc này
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```

Cũng có một số móc nối khác được gọi ở các "chặn" khác nhau của chu kỳ sống của đối tượng, ví dụ [`mounted`](../api/#mounted), [`updated`](../api/#updated), và [`destroyed`](../api/#destroyed). Tất cả các móc nối này đều được gọi gọi với từ khóa `this` luôn chỉ vào đối tượng Vue của chúng. Bạn có thể đang tự hỏi rằng, liệu khái niệm "controllers" có tồn tại trong Vue hay không và câu trả lời là : không. Thuật toán và các phương thức của bạn sẽ được chia ra và hoạt động bằng các móc nối này.

## Biểu đồ chu kỳ sống

Dưới đây là biểu đồ về các chặn trong chu kỳ sống của Vue, bạn chưa cần hiểu hết nhưng biểu đồ này sẽ rất hữu dụng trong tương lai.
![Lifecycle](/images/lifecycle.png)

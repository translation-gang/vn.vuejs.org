---
title: Các thuộc tính Computed và Watched
type: guide
order: 5
---

## Các thuộc tính Computed

Các biểu thức thể hiện trên các template - biểu mẫu rất là tiện lợi, nhưng chúng chỉ nên được dùng cho những thao tác đơn giản. Đặt quá nhiều logic lên biểu mẫu của bạn sẽ khiến chúng khó đọc và khó sửa chữa. Ví dụ:

``` html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

Như thế này thì biểu mẫu không còn đơn giản và súc tích nữa. Bạn phải nhìn một lúc mới hiểu được rằng nó sẽ đảo ngược gía trị chuỗi của thuộc tính `message`. Và vấn đề sẽ trở nên phức tạp hơn nữa khi mà bạn lại cho cái logic đảo chuỗi này vào trong biểu mẫu của bạn nhiều lần nữa.

Đó là lý do mà bạn nên sử dụng **thuộc tính computed - hậu kỳ** cho những trường hợp như thế.

### Basic Example

``` html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` là vm
      return this.message.split('').reverse().join('')
    }
  }
})
```

Result:

{% raw %}
<div id="example" class="demo">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },vì
})
</script>
{% endraw %}

Ở đây chúng ta đã khai báo ra một thuộc tính computed mang tên `reversedMessage`. Hàm mà chúng ta gán cho nó sẽ được dùng để tính ra `vm.reversedMessage`:

``` js
console.log(vm.reversedMessage) // -> 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // -> 'eybdooG'
```

Bạn cũng có thể mở console ra và tự thử nghiệm với vm. Giá trị của `vm.reversedMessage` luôn luôn phụ thuộc vào giá trị của `vm.message`.

Bạn có thể ràng buộc các thuộc tính computed vào trong biểu mẫu của bạn như các thuộc tính thông thường khác. Vue biết rằng `vm.reversedMessage` phụ thuộc vào `vm.message`, vì vậy mà nó sẽ cập nhật liên tục những ràng buộc liên quan tới `vm.reversedMessage` khi mà `vm.message` thay đổi. Và tuyệt vời nhất là chúng ta tạo ra mối quan hệ này rất ngắn gọn: hàm giá trị computed không có tác dụng phụ, làm cho chúng dễ dàng kiểm soát.

### Thuộc tính Computed và Methods

Chắc bạn đã có để ý rằng, chúng ta có thể đạt được điều tương tự bằng cách ràng buộc một method có trả về trong biểu mẫu dưới dạng biểu thức:

``` html
<p>Reversed message: "{{ reverseMessage() }}"</p>
```

``` js
// in component
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

Thay vì thuộc tính computed, bạn cũng có thể khai báo một method với hàm tương đương. Về kết quả, cả 2 cách đều cho ra kết quả như nhau. Nhưng, điểm khác nhau là **thuộc tính computed sẽ được cached - lưu vào vùng nhớ tạm tùy thuộc vào thuộc tính mà nó phụ thuộc vào.** Một thuộc tính computed sẽ chỉ được tính lại khi các ràng buộc của nó thay đổi. Điều này có nghĩa là, khi mà `message` còn chưa đổi, tất cả những gì đang sử dụng `reversedMessage` sẽ sử dụng lại dữ liệu và nó đã lưu trước đó mà không cần phải chạy lại hàm.

Điều này cũng có nghĩa là thuộc tính computed dưới đây sẽ không bao giờ được cập nhật, bởi vì `Date.now()` cũng không phải là ràng buộc động:

``` js
computed: {
  now: function () {
    return Date.now()
  }
}
```

Trong khi đó, một method - phương thức sẽ **luôn luôn** chạy hàm tính toán mỗi khi có một sự kiện thay đổi diễn ra.

Vậy vì sao chúng ta cần phải sao lưu cache? Giả sử chúng ta có một thuộc tính computed có hàm logic rắc rối và nặng nề **A**, mà muốn có giá trị trả về, chúng ta phải xét qua một mảng cực kỳ lớn và thông qua nhiều phép tính. Và sau đó chúng ta lại có những thuộc tính computed khác phụ thuộc vào gái trị trả về của **A**. Nếu không có sao lưu cache mà lại dùng method, chúng ta sẽ phải chạy qua hàm tính toán nặng nề kia nhiều lần hơn cần thiết. Nếu bạn không muốn cache, bạn có thể dùng methods.

### Thuộc tính computed và thuộc tính watched

Vue cũng cung cấp một cách rộng rãi hơn để quan sát và phản ứng theo sự thay đổi của dữ liệi: **thuộc tính watched - quan sát**. Khi bạn có một số dữ liệu cần được thay đổi dựa trên các dữ liệu khác, chuyện sử dụng `watch` quá đà sẽ rất là hấp dẫn, đặc biệt là khi bạn từng có tiền sử sử dụng AngularJS. Nhưng, thuộc tính computed vẫn tốt hơn so với việc sử dụng hàm gọi ngược `watch` mang tính áp đặt. Hãy xem ví dụ sau đây:

``` html
<div id="demo">{{ fullName }}</div>
```

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

Đoạn code trên một là áp đặt, hai là lặp lại. So sánh với đoạn này khi sử dụng computed:

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Thì sẽ tốt hơn.

### Thiết lập Computed


Các thuộc tính Computed, mặc định là lấy giá trị (get), nhưng bạn cũng có thể thiết lập giá trị (set) khi cần thiết:

``` js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Bây giờ thì khi bạn thử `vm.fullName = 'John Doe'`, phần `set` sẽ hoạt động và `vm.firstName` cùng `vm.lastName` sẽ được thiết lập và cập nhật theo logic đã cho.

## Watchers - Các thuộc tính được quan sát

Các thuộc tính computed thích hợp cho rất nhiều trường hợp, nhưng vẫn có một số trường hợp mà chúng ta nhất thiết phải sử dụng watcher. Đó là lý do mà Vue cung cấp cài đặt `watch` để thêm một cách theo dõi sự biến đổi của dữ liệu. Khi bạn muốn thực hiện các thao tác bất đồng bộ hoặc có logic nặng khi dữ liệu thay đổi thì việc dùng cài đặt `watch` rất hữu dụng.
Đơn cử:

``` html
<div id="watch-example">
  <p>
    Hỏi một câu hỏi có/không:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

``` html
<!-- Cộng đồng đã có rất nhiều thư viện ajax   -->
<!-- và các snippets cho những công việc thường xảy ra, cho Vue -->
<!-- sẽ không tái thiết lập lại chúng để giữ dung lượng nhỏ, và chuyện này cũng sẽ   -->
<!-- cho bạn sự tự do sử dụng những gì quen thuộc -->
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // mỗi khi question đổi, hàm này sẽ chạy
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {

    // _.debounce là hàm được cung cấp bởi lodash để giới hạn
    // số lần một bước tính toán có thể chạy.
    // Trong trường hợp này, chúng ta đang muốn giới hạn số lần truy cập
    // vào yesno.wtf/api, chờ cho đến khi người dùng đã gõ phím xong mới gọi ajax.
    
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // Đây là số mili giây mà code sẽ chờ cho đến khi người dùng gõ xong.
      500
    )
  }
})
</script>
```
Kết quả:

{% raw %}
<div id="watch-example" class="demo">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      500
    )
  }
})
</script>
{% endraw %}

In this case, using the `watch` option allows us to perform an asynchronous operation (accessing an API), limit how often we perform that operation, and set intermediary states until we get a final answer. None of that would be possible with a computed property.

Trong trường hợp nêu trên, cài đặt `watch` cho phép chúng ta thực hiện một thao tác bất đồng bộ là truy vấn API, giới hạn tần suất thực hiện thao tác đó, và đặt ra những vùng trạng thái đệm cho đến khi chúng ta có kết quả cuối cùng. Nhưng việc đó khi6ng thể được thực hiện bởi `computed`.

Ngoài cài đặt `watch`, bạn cũng có thể sử dụng [vm.$watch](../api/#vm-watch).
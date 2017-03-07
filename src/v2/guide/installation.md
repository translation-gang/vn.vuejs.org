---
title: Cài đặt
type: guide
order: 1
vue_version: 2.1.10
dev_size: "218.92"
min_size: "70.99"
gz_size: "25.86"
ro_gz_size: "18.01"
---

### Lưu ý về tính tương thích

Vue **không** hỗ trợ IE8 trở xuống, vì đó là những trình duyệt sử dụng các tính năng lỗi thời của ECMAScript 5. Ngoài ra, [tất cả những trình duyện hiện đại có ECMAScript 5](http://caniuse.com/#feat=es5) đều được hỗ trợ.

### Các lưu ý về phiên bản

Chi tiết về các phiên bản đều có thể được đọc trên [GitHub](https://github.com/vuejs/vue/releases).

## Cài đặt `<script>` trực tiếp

Bạn chỉ việc tải và thêm vào trang với tag `script`, lúc đó `Vue` sẽ trở thành biến toàn cục.

<p class="tip">Đừng sử dụng bản nén khi đang phát triển ứng dụng. Bạn sẽ rất khó biết rằng mình đang gặp phải nhưng lỗi thường gặp nào!</p>

<div id="downloads">
  <a class="button" href="/js/vue.js" download>Bản nguyên</a><span class="light info">Với đầy đủ các cảnh báo lỗi và chế độ debug</span>
  <a class="button" href="/js/vue.min.js" download>Bản nén</a><span class="light info">Đã tháo bỏ các cảnh báo lỗi, {{gz_size}}kb min+gzip</span>
</div>

### CDN

Khuyến khích: [unpkg](https://unpkg.com/vue/dist/vue.js), đường dẫn này sẽ cho phép thấy được phiên bản mới nhất trên npm. Bạn cũng có thể tra nguồn của gói npm package ở [unpkg.com/vue/](https://unpkg.com/vue/).

Cũng có mặt trên [jsDelivr](//cdn.jsdelivr.net/vue/latest/vue.js) hoặc [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js), nhưng hai dịch vụ này sẽ mất một khoảng thời gian để có được phiên bản mới nhất.

## NPM

Khi xây dựng những ứng dụng có quy mô lớn với Vue, thì phương thức cài đặt bằng NPM được chúng tôi khuyến khích sử dụng. Phương thức này có thể kết hợp rất tốt với [Webpack](https://webpack.js.org/) hoặc [Browserify](http://browserify.org/). Vue cũng cung cấp nhưng công cụ quản lý những [Component một trang](single-file-components.html).

``` bash
# latest stable
$ npm install vue
```

### So sánh giữa các bản build khác nhau

Có hai bản build khả dụng, bản build đơn và một bản build chỉ chạy lúc runtime. Điểm khác nhau là bản build đơn có bao gồm **trình biên dịch template** và bản build runtime không có.

Trình biên dịch template chịu trách nhiệm biên dịch những khai báo dưới `template` trong các đối tượng Vue thành những hàm JavaScript thông thường, nếu như bạn muốn dùng `template: `, bạn sẽ cần trình này.

- Bản build đơn có trình biên dịch này và hỗ trợ cài đặt `template`, **Bản này cũng phải dựa vào browser APIs nên bạn không thể dùng để diễn dịch phía server được.**

- Bản build chỉ chạy runtime không bao gồm trình biên dịch template, và tất nhiên không hỗ trợ cài đặt `template`. Bạn chỉ có thể sử dụng cài đặt `render` khi sử dụng bản build này, nhưng bản này lại hoạt động với các components một trang, bởi vì template của các component đó sẽ được dịch thành `render` khi build. Bản build runtime chỉ nhẹ hơn bản build đơn chừng 30%, với dung lượng {{ro_gz_size}}kb min+gzip.

Mặc định thì gói NPM sẽ cho xuất - exports - ra bản build runtime. Nếu bạn muốn dùng bản build đơn, hãy thêm alias vào Webpack config lại như sau:
``` js
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.common.js'
  }
}
```

Còn với Browserify, bạn có thể thêm alias như sau vào package.json:
``` js
"browser": {
  "vue": "vue/dist/vue.common"
},
```

<p class="tip">KHÔNG ĐƯỢC khai báo `import Vue from 'vue/dist/vue.js'` - một số công cụ hoặc thư viện của bên thứ 3 cũng có thể đã import Vue, điều này có thể khiến cho ứng dụng của bạn load cả 2 bản build cùng một lúc và sinh ra lỗi.</p>

### Các môi trường áp dụng CSP

Một số môi trường, như ứng dụng Chrome, bắt buộc thực thi các chính sách bảo mật nội dung (CSP), và sẽ cấm sử dụng khai báo `new Function()`. Bản build đơn dựa vào chức năng `new Function()` này để biên dịch templates, vì thế mà trở nên bất khả dụng trong những môi trường như thế.

Mặt khác, bản build runtime lại hoàn toàn theo sát các quy tắc CSP. Khi bạn sử dụng bản build runtime với Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) hoặc [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), templates của bạn sẽ được chuyển hóa thành `render`, và chạy được trên các môi trường có CSP.

## CLI

Vue.js có cung cấp một công cụ [CLI chính thức](https://github.com/vuejs/vue-cli) giúp cho người viết ứng dụng có thể nhanh chóng thiết lập môi trường làm việc cho những ứng dụng có quy mô lớn. Công cụ CLI này cung cấp một môi trường phù hợp với quy trình làm việc của frontend hiện đại. Chỉ mất một vài phút để cài đặt và chạy với hot-reload (reload lại trang khi có thauy đổi ngay lập tức), lint-on-save (phân tích và dự báo lỗi của code khi lưu lại), và sẵn sàng xây dựng bản release cho ứng dụng của bạn.

``` bash
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project
# install dependencies and go!
$ cd my-project
$ npm install
$ npm run dev
```
<p class="tip">Công cụ CLI yêu cầu một số kiến thức cơ bản về Node.js và các công cụ build của nó. Nếu như bạn còn khá xa lạ với Vue hoặc các công cụ front-end, chúng tôi khuyến khích bạn nên xem qua <a href="./">hướng dẫn này</a> trước khi sử dụng công cụ CLI.</p>

## Dev Build
## Bản tự build

**Quan trọng**: bản build trong mục `/dist` của GitHub là bản đã được phát hành chính thức. Nếu bạn nmuốn sử dụng những tính năng mới nhất của Vue còn đang thử nghiệm trên các mã nguồn GitHub, bạn phải tự build!
``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

Bản tải về lẻ hoặc phiên bản cài đặt bằng Bower đã được bọc bằng UMD (Định danh module toàn cục - Universal Module Definition) nên có thể sử dụng trực tiếp như một AMD module (AMD: Định danh module bất đồng bộ, có thể tìm hiểu thêm tại [đây](https://en.wikipedia.org/wiki/Asynchronous_module_definition))
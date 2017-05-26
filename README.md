# vue-calendar

> 没什么用的移动端日历，支持左右滑动，支持缩小显示周日历，自己看demo呗。

## 使用方式

``` bash
# bash:
npm i vue2-mobile-calendar --save
```
``` javascript
// *.js:
import vueCalendar from 'vue2-mobile-calendar'

// *.vue:
components: {
  vueCalendar
}
```
``` html
<!-- html: -->
<vue-calendar></vue-calendar>
```

## 参数
**不支持低版本浏览器，懒得适配**

``` javascript
highlight: {
  type: String,
  default: '#000'
},
selectcolor: {
  type: String,
  default: '#000'
},
weekstart: {
  type: Number,
  default: 7
},
selectdate: {
  type: [Date, String],
  default () {
    return new Date()
  }
},
callback: {
  type: Function,
  default () {
    return () => {}
  }
},
open: {
  type: Boolean,
  default: false
}
```

## Build Setup
**Requires Node.js 7+**

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

## License

MIT

### 说明
```
本组件是基于photoswipe，API和方法可参考[官网文档](https://photoswipe.com/)
同时扩展了原插件，新增了内容和左右按钮的插槽，方便自定义内容，具体见src/App.vue
```
## 快速开始
```
import photoSwipe from './components/photoSwipe.vue'

```

## 使用方法
```
<photo-swipe
  v-model="showPhotoSwipe"
  :options="options"
  :images="images"
  @init="init"
/>
v-model设置组件显示隐藏，options基本设置 images图片数组，格式： [{url: 'a.jpg'}, {url: 'b.jpg'},...]
init初始化完成，如下
init(gallery){
  //gallery返回PhotoSwipe对象，然后可以使用各种API和Methods
  console.log(gallery)
  this.gallery = gallery
  this.gallery.listen('afterChange', () => {
    this.current = this.gallery.getCurrentIndex() + 1
  })
}
```

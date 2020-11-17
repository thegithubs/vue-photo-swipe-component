## 说明
```
基于photoswipe的预览图组件，具体见使用方法或参考官网文档 https://photoswipe.com
同时扩展了原插件，新增了内容和左右按钮的插槽，方便自定义内容
NPM地址 https://www.npmjs.com/package/vue-photo-swipe-thenpmjs
```
## 安装
```
npm i vue-photo-swipe-thenpmjs

```

## 使用方法
```
import photoSwipe from 'vue-photo-swipe-thenpmjs'

<div @click="showPhotoSwipe=true">popup</div>
<photo-swipe
  v-model="showPhotoSwipe"
  :options="options"
  :images="images"
  @init="init"
>
  <div slot="content" class="content" @click="close">点击关闭 自定义索引 {{current}}/{{images.length}}</div>
  <div slot="prev">prev</div>
  <div slot="next">next</div>
</photo-swipe>

v-model  是否显示组件，布尔值
options  基本设置，{index: 3, escKey: false, ...}
         更多请参考官网文档 https://photoswipe.com/documentation/options.html
images   图片数组，格式： [{url: 'a.jpg'}, {url: 'b.jpg'},...]

//init初始化完成回调，之后才能使用api和methods

init(gallery){
  //gallery 返回PhotoSwipe对象，然后可以使用各种API和Methods
  console.log(gallery)
  this.gallery = gallery
  this.listen()
},
listen(){
  //可以监听多个事件，请参考官网 https://photoswipe.com/documentation/api.html
  this.gallery.listen('initialZoomIn', () => {
    console.log('initialZoomIn')
  })
  this.gallery.listen('afterChange', () => {
    this.current = this.gallery.getCurrentIndex() + 1
  })
},
close(){
  this.gallery.close()
}
```
## 插槽
```
content  内容
prev     左箭头
next     右箭头
```

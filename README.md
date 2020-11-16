### 说明
```
基于photoswipe的预览图组件，API和方法可参考![官网文档](https://photoswipe.com/)
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
>
  <div slot="content" class="content" @click="close">点击关闭 自定义索引 {{current}}/{{images.length}}</div>
  <div slot="prev">prev</div>
  <div slot="next">next</div>
</photo-swipe>

v-model是否显示预览组件，布尔值
options基本设置，参考官网文档
images图片数组，格式： [{url: 'a.jpg'}, {url: 'b.jpg'},...]

init初始化完成回调，回调之后才能使用api和methos，参考src/App.vue
init(gallery){
  //gallery返回PhotoSwipe对象，然后可以使用各种API和Methods
  console.log(gallery)
  this.gallery = gallery
  this.gallery.listen('afterChange', () => {
    this.current = this.gallery.getCurrentIndex() + 1
  })
}
```
## 插槽
```
slot="content" 内容
slot="prev" 左箭头
slot="next" 右箭头
```

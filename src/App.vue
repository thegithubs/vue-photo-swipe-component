<template>
  <div id="app">
    <div @click="showSwipe">photoSwipe</div>
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
  </div>
</template>

<script>
  import photoSwipe from './components/photoSwipe.vue'

  export default {
    name: 'app',
    components: {
      photoSwipe,
    },
    data(){
      return {
        showPhotoSwipe: false,
        images: [
          {
            src: 'https://placekitten.com/1200/900'
          },
          {
            src: 'https://farm4.staticflickr.com/3920/15008465772_d50c8f0531_h.jpg'
          }
        ],
        options: {
          
        },
        gallery: null,
        current: 0,
      }
    },
    mounted(){
      
    },
    methods: {
      showSwipe(){
        this.showPhotoSwipe = true
      },
      init(gallery){
        console.log(gallery)
        this.gallery = gallery
        this.gallery.listen('afterChange', () => {
          this.current = this.gallery.getCurrentIndex() + 1
        })
      },
      close(){
        this.gallery.close()
      },
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.content {
  position: absolute;
  bottom: 20px;
  left: 0;
  width: 100%;
  text-align: center;
  color: #fff;
  z-index: 99;
}
</style>

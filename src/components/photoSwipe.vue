<template>
  <div class="photoSwipe">
    <!-- Root element of PhotoSwipe. Must have class pswp. -->
    <div class="pswp" tabindex="-1" role="dialog" aria-hidden="true">
      <!-- Background of PhotoSwipe. 
           It's a separate element as animating opacity is faster than rgba(). -->
      <div class="pswp__bg"></div>
      <!-- Slides wrapper with overflow:hidden. -->
      <div class="pswp__scroll-wrap">
        <!-- Container that holds slides. 
            PhotoSwipe keeps only 3 of them in the DOM to save memory.
            Don't modify these 3 pswp__item elements, data is added later on. -->
        <div class="pswp__container" @click="hide">
          <div class="pswp__item"></div>
          <div class="pswp__item"></div>
          <div class="pswp__item"></div>
        </div>
        <!-- Default (PhotoSwipeUI_Default) interface on top of sliding area. Can be changed. -->
        <div class="pswp__ui pswp__ui--hidden">
          <div class="pswp__top-bar">
            <!--  Controls are self-explanatory. Order can be changed. -->
            <div class="pswp__counter"></div>
            <button class="pswp__button pswp__button--close"></button>
            <button class="pswp__button pswp__button--share"></button>
            <button class="pswp__button pswp__button--fs"></button>
            <button class="pswp__button pswp__button--zoom"></button>
            
            <!-- Preloader demo https://codepen.io/dimsemenov/pen/yyBWoR -->
            <!-- element will get class pswp__preloader active when preloader is running -->
            <div class="pswp__preloader">
              <div class="pswp__preloader__icn">
                <div class="pswp__preloader__cut">
                  <div class="pswp__preloader__donut"></div>
                </div>
              </div>
            </div>
          </div>
          
          <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
            <div class="pswp__share-tooltip"></div> 
          </div>
          
          <button class="pswp__button pswp__button--arrow--left" v-if="!$slots.prev"></button>
          <div class="pswp__button--arrow--left pswp__prev" @click="prev" v-else>
            <slot name="prev"></slot>
          </div>
          
          <button class="pswp__button pswp__button--arrow--right" v-if="!$slots.next"></button>
          <div class="pswp__button--arrow--right pswp__next" @click="next" v-else>
            <slot name="next"></slot>
          </div>
          
          <div class="pswp__caption">
            <div class="pswp__caption__center"></div>
          </div>
        </div>
        
        <slot name="content"></slot>
        
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import '@/assets/css/photoswipe.css'
  import '@/assets/css/default-skin.css'
  import '@/assets/js/photoswipe.min'
  import '@/assets/js/photoswipe-ui-default.min'
  
  export default {
    name: 'photoSwipe',
    model: {
      prop: 'modelPhotoSwipe',
      event: 'close'
    },
    props: {
      modelPhotoSwipe: {
        type: Boolean,
        default: false,
      },
      images: {
        type: Array,
        default: () => []
      },
      options: {
        type: Object,
        default: () => {}
      },
    },
    data(){
      return {
        gallery: null,
        baseOptions: {
          spacing: 0,
          focus: false,
          zoomEl: false,
          history: false,
          shareEl: false,
          arrowEl: false,
          closeEl: false,
          fullscreenEl: false,
          closeOnScroll: false,
          showAnimationDuration: 0,
          hideAnimationDuration: 0,
          tapToToggleControls: false,
          clickToCloseNonZoomable: true,
        },
      }
    },
    mounted(){
      this.init()
    },
    watch: {
      modelPhotoSwipe(model) {
        if (!model) return
        this.init()
        this.render()
      },
    },
    methods: {
      init(){
        const pswpElement = document.querySelectorAll('.pswp')[0],
          options = Object.assign(this.baseOptions, this.options)
        this.images.forEach(i => {
          const img = new Image()
          img.src = i.src
          if (img.complete) {
            this.$set(i, 'w', img.width)
            this.$set(i, 'h', img.height)
          } else {
            img.onload = () => {
              this.$set(i, 'w', img.width)
              this.$set(i, 'h', img.height)
            }
          }
        })
        this.gallery = new PhotoSwipe( pswpElement, PhotoSwipeUI_Default, this.images, options)
      },
      render(){
        this.$emit('init', this.gallery)
        this.gallery.init()
        this.closePhoto()
      },
      closePhoto(){
        this.gallery.listen('close', () => {
          this.value = false
          this.$emit('close', this.value)
        })
      },
      hide(e){
        if (e.target.className !== 'pswp__img') this.gallery.close()
      },
      prev(){
        this.gallery.prev()
        setTimeout(() => {
          this.gallery.ui.showControls()
        }, 300)
        this.$emit('prev')
      },
      next(){
        this.gallery.next()
        setTimeout(() => {
          this.gallery.ui.showControls()
        }, 300)
        this.$emit('next')
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pswp__prev {
    position: absolute;
    top: 50%;
    left: 8px;
    transform: translateY(-50%);
    color: #fff;
    z-index: 99;
  }
  .pswp__button--arrow--left.pswp__prev, .pswp__button--arrow--right.pswp__next {
    margin-top: 0;
    width: auto;
    height: auto;
  }
  .pswp__button--arrow--left.pswp__prev:before, .pswp__button--arrow--right.pswp__next:before {
    width: 0;
    height: 0;
  }
  .pswp__next {
    position: absolute;
    top: 50%;
    right: 8px;
    transform: translateY(-50%);
    color: #fff;
    z-index: 99;
  }
  
</style>

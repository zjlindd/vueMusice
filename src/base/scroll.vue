<template>
  <div ref='wrapper'>
    <slot></slot>
  </div>
</template>

<script>
  // 封装一个scroll组件 引入BSscroll
  import BScroll from 'better-scroll'

  export default{
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true,
      },
      data: {
        type: Array,
        default: null
      },
      refreshDelay: {
        type: Number,
        default: 20
      },
      listenScroll: {
        type: Boolean,
        default: false
      },
      pullUpRefresh: {
        type: Boolean,
        default: false
      },
      beforeScroll:{
        type: Boolean,
        default: false
      }
    },
    components: {},
    data(){
      return {}
    },
    created(){

    },
    mounted(){
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    computed: {},
    methods: {
      _initScroll(){
        console.log(this.listenScroll);
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: this.click
        })
        if (this.listenScroll) {
          let _this = this;
          this.scroll.on('scroll', (pos) => {
            _this.$emit('scroll', pos);
          })
        }

        if (this.pullUpRefresh) {
          this.scroll.on('scrollEnd', () => {
            console.log(this.scroll.y);
            console.log(this.scroll.maxScrollY);
            if (this.scroll.y <= (this.scroll.maxScrollY + 50)) {
              this.$emit('UPEnd')
            }
          })
        }
        if(this.beforeScroll){
          this.scroll.on('beforeScrollStart',()=>{
            this.$emit('beforeScroll')
          })
        }
      },
      disabled(){
        this.scroll && this.scroll.disable()
      },
      enabled(){
        this.scroll && this.scroll.enable()
      },
      refresh(){
        this.scroll && this.scroll.refresh()
      },
      scrollTo(){
        this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments);
      },
      scrollToElement(){
        this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments);
      },
    },
    watch: {
      data(){
        setTimeout(() => {
          this.refresh()
        }, this.refreshDelay)
      }
    }
  }
</script>

<style>

</style>

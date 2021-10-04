<template>
  <div :class="['chat__text',{'chat-img-wrap': msgData.content}]">
    <img ref="image" class="chat-img"  src="../assets/loading.gif"/>
    <div class="chat__text--arrow"></div>
  </div>
</template>

<script>
  export default {
    name: "chat-msg-image",
    props: {
      msgData: {
        type: Object,
        default: {}
      },
    },
    mounted: function() {
      this.preload()
    },
    methods: {
      /**
       * 图片预加载
       */
      preload:function () {
        let image = new Image()
        image.src =this.msgData.content;
        image.onload = () => {
          this.$refs.image.src=this.msgData.content;
        }
      },
    },
  }

</script>

<style scoped lang="scss">
  @import "chat";
</style>

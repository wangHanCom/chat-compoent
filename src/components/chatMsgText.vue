<template>
  <div class="chat__text">
    <span v-html="msgData.content"></span>
    <div class="chat__text--arrow"></div>
  </div>
</template>

<script>
  export default {
    name: "chat-msg-text",
    props: {
      msgData: {
        type: Object,
        default: {}
      },
    },
  }
</script>

<style scoped lang="scss">
  @import "chat";
</style>

<template>
  <div>
    <div class="chat-history" ref="chatHistory" @scroll="chatScroll">
      <div v-if="isNewMsg" class="new-msg" @click="readNewMsg">{{newMsgCount}}条新消息</div>
      <a href="javascript:;" class="look-more" @click="getHistoryChat" v-show="isShowGetHistoryBtn">查看更多聊天</a>
      <a v-show="isLoadingHistory" class="loading-more"><span class="loading-more__image"></span>加载中....</a>
      <ul class="chat-list">
        <li v-for="item in chatDataArr" :class="'chat__content--'+item.from">
          <!--时间戳-->
          <p class="timestamp" v-show="item.isShowTimestamp">{{item.timestamp}}</p>
          <component :is="currentComponent(item.type)" :msgData="item"></component>
        </li>
      </ul>
    </div>
    <div class="chat-input" @keydown="sendMessageKeydown">
      <textarea rows="3" cols="20"
                class="chat-textarea"
                placeholder="请输入内容"
                v-model="chatTextarea"></textarea>
    </div>
  </div>
</template>

<script>
  import chatMsgImage from './chatMsgImage';
  import chatMsgText from './chatMsgText';
  export default {
    components: { "chat-msg-image":chatMsgImage, "chat-msg-text":chatMsgText },
    name: 'chat',
    props: {
      chatDataArr: {
        type: Array,
        default: function () {
          return [];
        }
      },
      newMsgCount: {
        type: Number,
        default: 0
      },
      isNewMsg: {
        type: Boolean,
        default: false
      },
      isLoadingHistory: {
        type: Boolean,
        default: false
      },
      isShowGetHistoryBtn: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        chatTextarea: '',//输入框的值
      }
    },
    computed: {
      //当前加载的组件
      currentComponent() {
        return function (type) {
          return 'chat-msg-'+type
        }
      }
    },
    methods: {
      /**
       * 滚动监听
       */
      chatScroll() {
        let chatHistory = this.$refs.chatHistory;
        if (chatHistory.scrollTop >= chatHistory.scrollHeight - chatHistory.clientHeight) {
          this.$emit('update:isNewMsg', false)
          this.$emit('update:newMsgCount', 0)
        }
      },

      readNewMsg() {
        this.$emit('update:isNewMsg', false)
        this.$emit('update:newMsgCount', 0)
        let chatHistory = this.$refs.chatHistory;
        chatHistory.scrollTop = chatHistory.scrollHeight - chatHistory.clientHeight;
      },

      sendMessageKeydown(event) {
        let code = event.keyCode || event.which
        if (code === 13 && !event.shiftKey) {
          event.preventDefault();
          this.sendMessage(this.chatTextarea);
          this.clearMsg();
        }
      },

      sendMessage: function (msg) {
        this.$emit('sendMessage', msg);
      },

      clearMsg: function () {
        this.chatTextarea = "";
      },

      getHistoryChat() {
        this.$emit('getHistoryMsg');
      },
    },
  }
</script>

<style scoped lang="scss">
  @import "chat";
</style>

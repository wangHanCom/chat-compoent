<template>
  <div class="wrap">
    <h3>一个聊天组件</h3>
    <p>1、可以尝试在控制台输入 otherSendMsg("我来了") 模拟对方发送消息收到新消息的效果</p>
    <p>2、尝试将浏览器控制台 Network 设置为 slow 3g查看预加载效果</p>
    <chat
      ref="chatComponent"
      :chatDataArr="chatDataArr"
      :newMsgCount.sync="newMsgCount"
      :isNewMsg.sync="isNewMsg"
      :isShowGetHistoryBtn="isShowGetHistoryBtn"
      :isLoadingHistory="isLoadingHistory"
      @getHistoryMsg="getHIstoryMsg"
      @sendMessage="sendMessage"
    >
    </chat>
  </div>
</template>

<script>
  import chat from '../components/chat';

  export default {
    name: 'home',
    props: {},
    components: {
      'chat': chat,
    },
    data() {
      return {
        chatDataArr:[
          {"content": "今天天气真好", "from": "web", "timestamp": "15:21", "type": "text",},
          {"content": "https://ns-strategy.cdn.bcebos.com/ns-strategy/upload/fc_big_pic/part-00646-3920.jpg", "from": "phone", "timestamp": "19:22","type": "image"},
          {"content": "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1582296674050&di=8ebf8dd2168fe74778547302f9f6a2d1&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2F9%2F583654f37cf5c.jpg", "from": "phone", "timestamp": "19:22","type": "image",},
          {"content": "要不要去逛街", "from": "phone", "timestamp": "19:33", "type": "text",},
          {"content": "你看下多少度", "from": "phone", "timestamp": "19.45", "type": "text",},
          {"content":"你撤回了一条消息","from":"system","timestamp":"15:00","type": "text"},
          {"content": "今天天气真好", "from": "web", "timestamp": "15:21", "type": "text",},
          {"content": "要不要去逛街", "from": "phone", "timestamp": "19:33", "type": "text", },
          {"content": "你看下多少度", "from": "phone", "timestamp": "19.45", "type": "text", },
          {"content":"你撤回了一条消息","from":"system","timestamp":"15:00","type": "text"},
          {"content": "今天天气真好", "from": "web", "timestamp": "15:21", "type": "text",},
          {"content": "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1582296674050&di=8ebf8dd2168fe74778547302f9f6a2d1&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2F9%2F583654f37cf5c.jpg", "from": "phone", "timestamp": "19:22","type": "image"},
          {"content": "要不要去逛街", "from": "phone", "timestamp": "19:33", "type": "text",},
          {"content": "你看下多少度", "from": "phone", "timestamp": "19.45", "type": "text",},
          {"content":"你撤回了一条消息","from":"system","timestamp":"15:00","type": "text"},
          {"content": "今天天气真好", "from": "web", "timestamp": "15:21", "type": "text",},
          {"content": "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1582296674050&di=8ebf8dd2168fe74778547302f9f6a2d1&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2F9%2F583654f37cf5c.jpg", "from": "phone", "timestamp": "19:22","type": "image"},
          {"content": "要不要去逛街", "from": "phone", "timestamp": "19:33", "type": "text",},
          {"content": "你看下多少度", "from": "phone", "timestamp": "19.45", "type": "text",},
        ],
        chatHistory1:[{"content": "昨天的历史记录1", "from": "web", "timestamp": "01:21", "type": "text","isShowTimestamp": true},
          {"content": "昨天的历史记录2", "from": "web", "timestamp": "01:21", "type": "text","isShowTimestamp": true},
          {"content": "昨天的历史记录3", "from": "web", "timestamp": "01:21", "type": "text","isShowTimestamp": true},
          {"content": "昨天的历史记录4", "from": "web", "timestamp": "01:21", "type": "text","isShowTimestamp": true},],
        isNewMsg:false,//是否有新消息
        newMsgCount:0,//新消息数量
        historyCurrentPage: 0, //历史聊天记录当前页面
        historyChatArr: [], //历史聊天记录数组
        historyLargerPage:2,//历史记录最大页数
        isLoadingHistory:false,//是否正在加载更多
        isShowGetHistoryBtn: true, //是否显示"查看更多聊天"按钮，一开始显示，当查询到最后一条时隐藏该按钮
      }
    },
    mounted() {
      // 绑定到window下面，提供给外部调用。模拟对方发送消息
      window['otherSendMsg'] = data => {
        this.reciveMsg(data);
      }
    },
    methods:{

      /**
       * 发送新消息
       * @param msg
       */
      sendMessage:function (msg) {
        this.chatDataArr.push({"content": msg, "from": "phone", "timestamp": "19:33", "type": "text",});
        this.scrollPage('bottom');
      },

      /**
       * 消息撤回
       */
      handleUndo:function () {
        console.log("home--撤回消息")
      },

      /**
       * 获取历史消息
       */
      getHIstoryMsg:function () {
        if(this.isLoadingHistory){
          return;
        }
        if(this.historyCurrentPage<this.historyLargerPage){
          this.fetchData();
        }
      },

      /**
       * 模拟异步获取历史数据
       */
      fetchData:function () {
        this.isShowGetHistoryBtn=false;
        this.isLoadingHistory=true;
        this.historyCurrentPage+=1;
       setTimeout(()=>{
         this.isLoadingHistory=false;
         this.chatDataArr=this.chatHistory1.concat(this.chatDataArr);
         if(this.historyCurrentPage>=this.historyLargerPage){
           this.chatDataArr.unshift({"content":"没有更多消息了","from":"system","timestamp":"01:00","type": "text"},)
           this.scrollPage('top');
         }else{
           this.isShowGetHistoryBtn=true;
         }
       },3000)
      },

      /**
       * 页面滚动到顶部或者底部的方法
       * @param direction
       */
      scrollPage:function (direction) {
        let chatHistory = this.$refs.chatComponent.$refs.chatHistory;
        this.$nextTick(() => {
          if(direction==='top'){
            chatHistory.scrollTop = 0;
          }else if(direction==='bottom'){
            chatHistory.scrollTop = chatHistory.scrollHeight -chatHistory.clientHeight;
          }
        })
      },

      /**
       * 监听对方发送的新消息
       * @param msg
       */
      reciveMsg:function (msg) {
        this.chatDataArr.push({"content": msg, "from": "web", "timestamp": "19:33", "type": "text",});
        this.newMsgCount+=1;
        this.isNewMsg=true;
      }
    }
  }
</script>


<style lang="scss">
  .wrap {
    margin: 10px;
    .warp-title {
      text-align: center;
    }
    p{
      font-size: 12px;
    }
  }
</style>

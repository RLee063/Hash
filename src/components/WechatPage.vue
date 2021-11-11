<template>
  <div class="wechat-page">
    <div class="wechat-header">
      <div class="title">微信</div>
      <div class="trailing">
        <img class="icon-button" src="../assets/search.svg" />
        <img class="icon-button" src="../assets/add.svg"
          @click="showFloatTab = !showFloatTab"/>

        <transition name="float-in">
          <div class="float-tab" v-if="showFloatTab">
            <div class="float-mask" @click="showFloatTab = false"></div>
            <div class="float-tab-item" v-for="(ft, i) in floatTabs"
              v-bind:key="i" @click="scan">
              <img :src="ft.icon" alt="" class="float-tab-icon" />
              <div class="float-tab-name">{{ ft.name }}</div>
            </div>
          </div>
        </transition>
      </div>
    </div>
    <div class="wechat-content">
      <div class="message-item" v-for="(m, i) in messages" v-bind:key="i">
        <div :style="m.style" class="avatar"></div>
        <div class="message-body">
          <div class="header">
            <div class="name">{{ m.name }}</div>
            <div class="message-time">{{ m.time }}</div>
          </div>
          <div class="content">{{ m.content }}</div>
        </div>
      </div>
    </div>
    <div class="wechat-tab">
      <div class="tab-item" v-for="(t, i) in tabs" v-bind:key="i">
        <img :src="t.icon" class="tab-icon" />
        <div v-if="t.selected" class="tab-name selected">{{ t.name }}</div>
        <div v-else class="tab-name">{{ t.name }}</div>
      </div>
    </div>
  </div>
</template>

<script>
const fakeMessages = `Forgetting,来吃鸡
朝阳、小刚都、天丰富、k···,朝阳: 5555 手机尾号
微机原理,追梦人: 下周交作业怎么样？
402 项目组,论文.pdf
PDD,好
寝室乱斗,许昌:我明天就位
上海米源饮料,交易提醒
bird,这个你得自己研究
狂奔的蜗牛,嗯嗯 谢谢老师
2020 谢师兄,打卡`.split('\n').map((v, i) => {
  const [name, content] = v.split(',')
  const date = new Date(new Date() - parseInt(Math.random() * 3600 * 1000 * 12))
  const hour = date.getHours().toString().padStart(2, '0')
  const mins = date.getMinutes().toString().padStart(2, '0')
  const time = [hour, mins].join(':')
  const avatar = require('../assets/avatars.jpg')
  const bgStyle = `
    background: url(${avatar}) no-repeat 0 ${-i * 70}px;
    background-size: 47px 678px;`
  return {
    avatar,
    name,
    content,
    time,
    style: bgStyle
  }
})

export default {
  name: 'WechatPage',
  data() {
    return {
      showFloatTab: false,

      messages: fakeMessages,
      tabs: [{
        name: '微信',
        icon: require('../assets/chat-green.svg'),
        selected: true
      }, {
        name: '通讯录',
        icon: require('../assets/contact.svg'),
        selected: false
      }, {
        name: '发现',
        icon: require('../assets/compass.svg'),
        selected: false
      }, {
        name: '我',
        icon: require('../assets/user.svg'),
        selected: false
      }],
      floatTabs: [{
        name: '发起群聊',
        icon: require('../assets/chat.svg'),
        selected: true
      }, {
        name: '添加朋友',
        icon: require('../assets/friendadd-fill.svg'),
        selected: false
      }, {
        name: '扫一扫',
        icon: require('../assets/scan.svg'),
        selected: false
      }, {
        name: '收付款',
        icon: require('../assets/pay.svg'),
        selected: false
      }, {
        name: '帮助与反馈',
        icon: require('../assets/mail-fill.svg'),
        selected: false
      }]
    }
  },

  mounted() {
    console.log(this.messages)
  },

  methods: {
    scan () {
      this.showFloatTab = false
      setTimeout(() => {
        this.$router.push('./scan')
      }, 200)
    }
  }
}
</script>

<style lang="less" scoped>
.wechat-page {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  background: #EDEDED;
  // font-weight: bolder;
  user-select: none;

  .wechat-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 16px;
    padding: 15px;

    .title {
      font-weight: bolder;
    }

    .trailing {
      display: flex;
      position: relative;

      .icon-button {
        height: 20px;
        width: 20px;
        margin-left: 20px;
      }

      .float-tab {
        position: absolute;
        right: -7px;
        bottom: -300px;
        background: #4b4b4b;
        border-radius: 5px;
        transform-origin: top right;

        &:before {
          content: '';
          position: absolute;
          top: -7px;
          right: 10px;
          width: 0;
          height: 0;
          border-left: 8px solid transparent;
          border-right: 8px solid transparent;
          border-bottom: 8px solid #4b4b4b;
        }

        .float-mask {
          position: fixed;
          left: 0;
          top: 0;
          width: 100vw;
          height: 100vh;
        }

        .float-tab-item {
          display: flex;
          align-items: center;
          font-size: 16px;
          transform: scale(1);

          &:active {
            background: #3b3b3b;
          }

          .float-tab-icon {
            margin: 0 20px;
            margin-right: 15px;
            height: 23px;
            width: 23px;
          }

          .float-tab-name {
            color: white;
            padding: 18px 0;
            width: 130px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
          }

          &:last-child .float-tab-name {
            border: 0;
          }

          &:first-child {
            border-top-left-radius: 5px;
            border-top-right-radius: 5px;
          }

          &:last-child {
            border-bottom-left-radius: 5px;
            border-bottom-right-radius: 5px;
          }
        }
      }
    }
  }

  .wechat-content {
    flex: 1;
    background: #fff;
    overflow: scroll;

    .message-item {
      display: flex;
      align-items: center;

      &:active {
        background: rgba(0, 0, 0, 0.1);
      }

      .avatar {
        margin-left: 15px;
        margin-right: 10px;
        height: 47px;
        width: 47px;
        border-radius: 5px;
      }

      .message-body {
        padding: 16px 0;
        flex: 1;
        border-bottom: 1px solid #f7f7f7;

        .header {
          font-weight: bolder;
          display: flex;
          align-items: center;
          justify-content: space-between;
          padding-right: 15px;

          .name {
            font-size: 16px;
          }

          .message-time {
            color: #aaa;
            font-size: 12px;
          }
        }

        .content {
          font-size: 12px;
          color: #aaa;
          line-height: 1;
          margin-top: 5px;
        }
      }
    }
  }

  .wechat-tab {
    display: flex;
    justify-content: space-around;
    padding: 6px 0;
    border-top: 0.8px solid #eeeeee;
    background: #f7f7f7;

    .tab-item {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;

      .tab-icon {
        height: 20px;
        width: 20px;
        opacity: .7;
        margin-bottom: 3px;
      }

      .tab-name {
        font-weight: bolder;
        width: 100%;
        text-align: center;
        font-size: 12px;
      }

      .selected {
        color: #07C160;
      }
    }
  }

  .float-in-enter-active {
    animation: float-in-curve .15s ease;
  }

  .float-in-leave-active {
    animation: float-in-curve .15s ease reverse;
  }

  @keyframes float-in-curve {
    from {
      transform: scale(0.3);
      opacity: 0;
    }
    to {
      transform: scale(1);
      opacity: 1;
    }
  }
}
</style>
<template>
  <div class="botoom">
    <div class="chat-content" ref="chatContent">
      <div class="chat-wrapper" v-for="item in chatList" :key="item.id">
        <div class="chat-friend" v-if="item.sender !== username">
          <div class="chat-text">
            {{ item.content }}
          </div>
          <div class="info-time">
            <!--            <img :src="item.headImg" alt=""/>-->
            <span>{{ item.sender }}</span>
            <span>{{ item.time }}</span>
          </div>
        </div>

        <div class="chat-me" v-else>
          <div class="chat-text">
            {{ item.content }}
          </div>
          <div class="info-time">
            <span>{{ item.sender }}</span>
            <span>{{ item.time }}</span>
            <!--            <img :src="item.headImg" alt=""/>-->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {animation} from "../../assets/util/util";
import store from "../../store/index"

// import {getChatMsg} from "../../api/getData";

// import HeadPortrait from "@/components/HeadPortrait";
// import Emoji from "@/components/Emoji";
// import FileCard from "@/components/FileCard.vue";
export default {
  components: {},
  // props: ['index1'],
  watch: {
    '$store.state.currentRoom'() {
      this.getFriendChatMsg();
      // this.scrollBottom();
    }
  },
  data() {
    return {
      chatList: [],
      username: '',
      // friendInfo: {},
      // srcImgList: [],
    };
  },
  mounted() {
    this.username = store.state.username;
    this.getFriendChatMsg();

  },
  methods: {
    //获取聊天记录
    getFriendChatMsg() {
      this.chatList = store.state.rooms[store.state.currentRoom].history;
      this.scrollBottom();
    },
    //todo:获取窗口高度并滚动至最底层
    scrollBottom() {
      this.$nextTick(() => {
        const scrollDom = this.$refs.chatContent;
        animation(scrollDom, scrollDom.scrollHeight - scrollDom.offsetHeight);
      });
    },
  }
};
</script>

<style lang="scss" scoped>
//.chat-window {
//  width: 100%;
//  height: 100%;
//  margin-left: 20px;
//  position: relative;
#header {
  width: 100%;
  height: 10px;
  background-color:cornsilk;
  padding: 0px;
  display: flex;
  .head-portrait {
    width: 0px;
    height: 0px;
    // border-radius: 50%;
    overflow: hidden;
    img {
      width: 100%;
      height: 100%;
    }
  }
  .info {
    margin-left: 20px;
    .name {
      font-size: 20px;
      color: #fff;
    }
    .signature {
      font-size: 14px;
      color: #fff;
    }
  }
  .icon {
    width: 30px;
    height: 30px;
    cursor: pointer;
    img {
      width: 100%;
      height: 100%;
    }
  }
}

.botoom {
  width: 100%;
  height: 69vh;
  //background-color: rgb(50, 54, 68);
  border-radius: 20px;
  //padding: 20px;
  //margin-right: 20px;
  box-sizing: border-box;
  position: relative;

  .chat-content {
    width: 100%;
    height: 100%;
    overflow-y: scroll;
    //padding: 20px;
    box-sizing: border-box;

    &::-webkit-scrollbar {
      width: 0; /* Safari,Chrome 隐藏滚动条 */
      height: 0; /* Safari,Chrome 隐藏滚动条 */
      display: none; /* 移动端、pad 上Safari，Chrome，隐藏滚动条 */
    }

    .chat-wrapper {
      position: relative;
      word-break: break-all;

      .chat-friend {
        width: 100%;
        float: left;
        margin: 10px 0 10px 0;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;

        .chat-text {
          max-width: 90%;
          padding: 20px;
          border-radius: 20px 20px 20px 5px;
          background-color: rgb(56, 60, 75);
          color: #fff;

          &:hover {
            background-color: rgb(39, 42, 55);
          }
        }

        .chat-img {
          img {
            width: 100px;
            height: 100px;
          }
        }

        .info-time {
          margin: 0;
          color: #fff;
          font-size: 14px;

          img {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            vertical-align: middle;
            margin-right: 10px;
          }

          span {
            color: rgb(101, 104, 115);
            line-height: 30px;
          }

          span:first-child {
            color: rgb(101, 104, 115);
            margin-right: 10px;
            vertical-align: middle;
          }
        }
      }

      .chat-me {
        width: 100%;
        float: right;
        margin: 10px 20px 10px 0;
        position: relative;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-end;

        .chat-text {
          float: right;
          max-width: 90%;
          padding: 20px;
          border-radius: 20px 20px 5px 20px;
          background-color: rgb(29, 144, 245);
          color: #fff;

          &:hover {
            background-color: rgb(26, 129, 219);
          }
        }

        .chat-img {
          img {
            max-width: 300px;
            max-height: 200px;
            border-radius: 10px;
          }
        }

        .info-time {
          margin: 0px 0;
          color: #fff;
          font-size: 14px;
          display: flex;
          justify-content: flex-end;

          img {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            vertical-align: middle;
            margin-left: 10px;
          }

          span {
            color: rgb(101, 104, 115);
            line-height: 30px;
          }

          span:first-child {
            color: rgb(101, 104, 115);
            margin-right: 10px;
            vertical-align: middle;
          }
        }
      }
    }
  }
}

//
//    .chatInputs {
//      width: 90%;
//      position: absolute;
//      bottom: 0;
//      margin: 3%;
//      display: flex;
//
//      .boxinput {
//        width: 50px;
//        height: 50px;
//        background-color: rgb(66, 70, 86);
//        border-radius: 15px;
//        border: 1px solid rgb(80, 85, 103);
//        position: relative;
//        cursor: pointer;
//
//        img {
//          width: 30px;
//          height: 30px;
//          position: absolute;
//          left: 50%;
//          top: 50%;
//          transform: translate(-50%, -50%);
//        }
//      }
//
//      .emoji {
//        transition: 0.3s;
//
//        &:hover {
//          background-color: rgb(46, 49, 61);
//          border: 1px solid rgb(71, 73, 82);
//        }
//      }
//
//      .inputs {
//        width: 90%;
//        height: 50px;
//        background-color: rgb(66, 70, 86);
//        border-radius: 15px;
//        border: 2px solid rgb(34, 135, 225);
//        padding: 10px;
//        box-sizing: border-box;
//        transition: 0.2s;
//        font-size: 20px;
//        color: #fff;
//        font-weight: 100;
//        margin: 0 20px;
//
//        &:focus {
//          outline: none;
//        }
//      }
//
//      .send {
//        background-color: rgb(29, 144, 245);
//        border: 0;
//        transition: 0.3s;
//        box-shadow: 0px 0px 5px 0px rgba(0, 136, 255);
//
//        &:hover {
//          box-shadow: 0px 0px 10px 0px rgba(0, 136, 255);
//        }
//      }
//    }
//  }
//}
</style>
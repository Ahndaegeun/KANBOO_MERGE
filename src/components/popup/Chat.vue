<template>
  <button
    style="
      position: absolute;
      top: 30%;
      right: 50%;
      background: #eee;
      width: 100px;
      height: 100px;
    "
    @click="chatTest"
  >
    채팅 테스트
  </button>
  <div class="chat-container">
    <div
      class="chat-box"
      v-for="(chat, index) in chatData"
      :key="index"
      :id="`chatRoom-${index}`"
    >
      <div class="chat-header">
        <i
          v-if="chat.isMini == false"
          @click="setMini(index)"
          class="fas fa-minus"
        ></i>
        <i v-else @click="setMax(index)" class="far fa-square"></i>
      </div>
      <div class="chat-content">
        <ul>
          <li
            class="chat-line"
            v-for="(line, index) in chat.content"
            :key="index"
            :class="{ 'chat-myLine': line.id == userId }"
          >
            <!-- 상대방의 채팅에만 사진,닉네임 표시 시작 -->
            <div v-if="userId != line.id" class="chat-userInfo">
              <img :src="require(`@/assets/${line.img}`)" alt="img" />
              {{ line.id }}
            </div>
            <!-- 상대방의 채팅에만 사진,닉네임 표시 끝 -->

            <!-- 상대방의 채팅은 회색 배경 시작 -->
            <div class="chat-info" v-if="line.id != userId">
              <span class="chat-text chat-friend">
                {{ line.text }}
              </span>
              <span class="chat-date">{{ line.date }}</span>
            </div>
            <!-- 상대방의 채팅은 회색 배경 끝 -->

            <!-- 내 채팅은 주황색 배경 시작 -->
            <div class="chat-info" v-else>
              <span class="chat-date">{{ line.date }}</span>
              <span class="chat-text chat-my">
                {{ line.text }}
              </span>
            </div>
            <!-- 내 채팅은 주황색 배경 끝 -->
          </li>
        </ul>
      </div>
      <div class="chat-inputBox">
        <input
          :id="`chat-input-${index}`"
          @input="setText"
          @keyup.enter="sendMessage(index)"
          type="text"
        />
        <button @click="sendMessage(index)">전송</button>
      </div>
    </div>
  </div>
</template>

<script>
import chatData from "@/assets/chatData.js";
const moment = require("moment");
const today = moment();

export default {
  updated() {
    this.focus(this.lastChatRoom);
  },
  data() {
    return {
      chatData: chatData,
      inputText: "",
      userId: "zerochae",
      img: "con3.jpg",
      lastChatRoom: "",
    };
  },
  methods: {
    setText(e) {
      this.inputText = e.target.value;
    },
    // 여기다가 메시지 받기 기능 추가 해야함!!!!!!!!!!!!!!!!!!!!
    getMessage() {},
    sendMessage(index) {
      let date = today.format("HH:MM");

      if (this.inputText !== "") {
        this.chatData[index].content.push({
          id: this.userId,
          text: this.inputText,
          date: date,
        });
      }
      document.querySelector(`#chat-input-${index}`).value = "";
      this.inputText = "";
      this.lastChatRoom = index;
    },
    focus(index) {
      let chatRoom = document.querySelector("#chatRoom-0>.chat-content>ul");
      chatRoom.lastElementChild.scrollIntoView();
    },
    setMini(index) {
      let chatRoom = document.querySelector(`#chatRoom-${index}`);
      chatRoom.className = "chat-box chat-mini";
      this.chatData[index].isMini = true;
    },
    setMax(index) {
      let chatRoom = document.querySelector(`#chatRoom-${index}`);
      chatRoom.className = "chat-box";
      this.chatData[index].isMini = false;
    },
    // 채팅 메시지 받기 테스트
    chatTest() {
      let date = today.format("HH:MM");
      this.chatData[0].content.push({
        id: "kade",
        text: "머하냐~",
        img: "con1.jpg",
        date: date,
      });
    },
  },
};
</script>

<style>
.chat-container {
  position: absolute;
  right: 0;
  bottom: 0;
  display: flex;
  overflow: hidden;
  z-index: 1;
}
.chat-box {
  width: 20vw;
  min-width: 150px;
  height: 40vh;
  background: #2c2f3b;
  border-radius: 5px;
  position: relative;
  box-shadow: 3px 6px 10px rgba(255, 255, 255, 0.2) inset;
  margin: 5px;
  overflow: auto;
  display: flex;
}
.chat-header {
  position: absolute;
  top: 0;
  height: 18px;
  background: #414556;
  text-align: right;
  width: 100%;
  box-shadow: 3px 6px 6px rgba(255, 255, 255, 0.2) inset;
  z-index: 5;
}

.chat-header i {
  font-size: 18px;
  margin-right: 8px;
}

.chat-content {
  padding: 15px;
  font-size: 12px;
  overflow: auto;
  width: 100%;
}

.chat-content ul {
  margin-top: 6px;
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

.chat-line {
  display: flex;
  align-items: center;
  margin-bottom: 6px;
}

.chat-userInfo {
  display: flex;
  flex-direction: column;
  margin: 5px;
  align-items: center;
}

.chat-userInfo img {
  border-radius: 50%;
  width: 20px;
  margin-bottom: 7px;
}

.chat-info {
  display: flex;
}

.chat-text {
  margin-left: 10px;
  height: fit-content;
  width: fit-content;
  border-radius: 10px;
  display: flex;
  align-items: center;
  padding: 15px;
  position: relative;
}
.chat-friend::before {
  background: linear-gradient(
    -135deg,
    #414556 0%,
    #414556 50%,
    transparent 50%,
    transparent
  );
  content: "";
  position: absolute;
  top: 10px;
  left: -8px;
  width: 12px;
  height: 12px;
  z-index: 1;
}

.chat-friend {
  background: #414556;
  -webkit-filter: drop-shadow(0px 2px 2px rgba(10, 10, 10, 0.8));
}

.chat-myLine {
  justify-content: right;
}

.chat-my::before {
  background: linear-gradient(
    135deg,
    #ff8906 0%,
    #ff8906 50%,
    transparent 50%,
    transparent
  );
  content: "";
  position: absolute;
  top: 10px;
  right: -8px;
  width: 12px;
  height: 12px;
  z-index: 1;
}

.chat-my {
  background: #ff8906;
  -webkit-filter: drop-shadow(0px 2px 2px rgba(10, 10, 10, 0.8));
}

.chat-date {
  margin: 15px 0 0 10px;
}

.chat-inputBox {
  height: 30px;
  width: 100%;
  position: absolute;
  background: #414556;
  bottom: 0;
  display: flex;
  align-items: center;
  box-shadow: 3px -3px 6px rgba(255, 255, 255, 0.2) inset;
}
.chat-inputBox input {
  margin-left: 10px;
  background: none;
  color: white;
  border: none;
  outline: none;
  width: 100%;
}

.chat-inputBox button {
  background: #ff8906;
  border-radius: 10px;
  font-size: 12px;
  width: 50px;
  height: 20px;
  color: white;
  -webkit-filter: drop-shadow(0px 2px 2px rgba(0, 0, 0, 0.1));
  margin-right: 10px;
}

::-webkit-scrollbar {
  width: 0px;
}

.chat-mini {
  overflow: hidden;
  width: 100px;
  height: 18px;
  bottom: 0;
  position: relative;
  align-self: flex-end;
}

.chat-mini i {
  font-size: 12px;
}
</style>
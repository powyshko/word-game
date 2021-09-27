<template lang="pug">
  #app
    h1 Игра в слова
    // Play game btn
    BaseButton(@click="playGame" :disabled="disabled" :class="{disabled: disabled}") Начать

    //Reset game btn
    BaseButton.resetBtn(@click="resetGame" v-if="showResetBtn") Сбросить результат игры

    //Message
    Message(v-if="msg") Секунду ...

    // Tiles
    div.tiles-container(v-if="shuffleWord.length > 0")
      | {{wordObj.name}}
      img.tiles-img(:src="wordObj.img_src")
      div {{shuffleWord}}
      draggable.tiles-row(v-model="shuffleWord")
        div.tiles-item(v-for="(tile, index) in shuffleWord" :key="index")
          | {{tile}}

      //Message Erroe
      Message(v-if="msgError") Не правильно попробуйте еще раз

      // confirm btn
      BaseButton(@click="confirmAnswer" class="btnSucess") Готово

      //Modal
      Modal(v-if="showModal"  @close="showModal = false")
        div(slot="body")
          div Вы выиграли! Повторить?
          div.controls
            BaseButton(@click="playGame") Конечно
            BaseButton(@click="stopGame") Я уже устал
    //Table result

    Table(:resultGame="resultGame" @sort="onSort" v-if="showTable")

</template>

<script>
import axios from "axios";
import draggable from "vuedraggable";
import BaseButton from "@/components/UI/BaseButton";
import Message from "@/components/UI/Message";
import Modal from "@/components/UI/Modal";
import Table from "@/components/Table";

export default {
  name: "App",
  components: { BaseButton, Message, draggable, Modal, Table },
  data() {
    return {
      wordObj: {},
      shuffleWord: [],
      resultGame: [],
      startGame: false,
      minId: 2,
      maxId: 1368,
      msg: false,
      showModal: false,
      msgError: false,
      showTable: false,
      level: 0,
      startTime: "",
      sortOrder: "asc",
      disabled: false,
      showResetBtn: false,
    };
  },
  watch: {
    startGame() {
      this.playGame();
    },
  },
  methods: {
    playGame() {
      let id =
        Math.floor(Math.random() * (this.maxId - this.minId + 1)) + this.minId;
      this.msgError = false;
      this.showModal = false;
      this.startTime = Date.now();
      this.shuffleWord = [];
      this.msg = true;
      axios
        .get(`https://apidir.pfdo.ru/v1/directory-program-activities/${id}`)
        .then((res) => {
          if (res.data.result_message === "Запись не найдена") {
            this.startGame = !this.startGame;
          } else {
            this.msg = false;
            this.wordObj = res.data.data;
            this.shuffleWord = this.wordObj.name
              .toUpperCase()
              .split("")
              .sort(() => 0.5 - Math.random());
          }
        })
        .catch((e) => {
          console.log(e);
        });
    },
    confirmAnswer() {
      if (this.wordObj.name.toUpperCase() === this.shuffleWord.join("")) {
        this.showModal = true;
        this.msgError = false;
        let finishTime = Date.now() - this.startTime;
        let minutes = Math.floor(finishTime / 60000);
        let seconds = ((finishTime % 60000) / 1000).toFixed(0);
        let finishGame = {
          level: (this.level += 1),
          word: this.wordObj.name.toUpperCase(),
          time: `${minutes}:${seconds}`,
        };
        this.resultGame.push(finishGame);
      } else {
        this.msgError = true;
      }
    },
    stopGame() {
      this.showModal = false;
      this.shuffleWord = [];
      this.showTable = true;
      this.disabled = true;
      this.showResetBtn = true;
    },
    onSort(data) {
      this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
      this.resultGame.sort((a, b) => {
        let mod = 1;
        if (this.sortOrder === "desc") mod = -1;
        if (a[data] < b[data]) return -1 * mod;
        if (a[data] > b[data]) return 1 * mod;
        return 0;
      });
    },
    resetGame() {
      this.shuffleWord = [];
      this.showTable = false;
      this.disabled = false;
      this.showResetBtn = false;
      this.resultGame = [];
    },
  },
};
</script>

<style lang="scss">
.tiles-container {
  max-width: 1100px;
  margin: 50px auto;
  text-align: center;
}

.tiles-img {
  margin: 0px 0px 20px 0px;
  width: 100px;
  height: 100px;
}

.tiles-row {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}

.tiles-item {
  width: 50px;
  height: 50px;
  background: #005aff;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-weight: bold;
  margin: 0px 0px 20px 10px;
  cursor: pointer;
}

.controls {
  margin: 20px 0px 0px 0px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.disabled {
  cursor: not-allowed;
  opacity: 0.8;
}

.resetBtn {
  margin: 0px 0px 0px 20px;
}
</style>

<!-- pages/game.vue/ -->
<template>
    <div class="min-h-screen p-4 text-center bg-teal-300">
      <div v-if="round < 5">
        <h2 class="text-[3rem]">{{ emoji }}</h2>
        <div class="h-56 text-2xl">
          <h3 v-show="showHint" class="text-yellow-300">{{ hint1 }}</h3>
          <h3 v-show="showHint2" class="text-yellow-500">{{ hint2 }}</h3>
          <h3 v-show="showGiveUp" class="text-red-500">{{ giveUp }}</h3>
          <h3 v-show="showRestart" class="text-red-500">
            Press "Restart Game" if you want to restart game.
          </h3>
          <h3 v-show="showPressSubmit" class="text-black">
            Press "Submit" if you want to play next game.
          </h3>
          <!-- Conditionally render the answer -->
          <pre>............showAnswer....................</pre>
          <div v-if="showAnswer">
      <!-- Content to show when showAnswer is true -->
      The answer is... {{ answer }}
    </div>
          <pre>.............showAnswer...................</pre>
        </div>
        <div
          class="
            h-[20rem] p-3 rounded-t-3xl flex flex-col 
            items-center bg-teal-600 bottom-0 left-0 w-full"
        >
          <div class="">
            <input
              type="text"
              id="answer"
              required
              class="w-full h-10 px-4 md:mt-5 md:mb-6 text-sm peer bg-gray-100 outline-none focus:outline-none input-field"
              v-el:focusInput
              v-model="answer"
              v-bind:disabled="inputDisabled"
            />
            <label
              for="answer"
              class="transform transition-all absolute top-0 left-0 h-full flex items-center pl-2 text-sm group-focus-within:text-xs peer-valid:text-xs group-focus-within:h-1/2 peer-valid:h-1/2 group-focus-within:-translate-y-full peer-valid:-translate-y-full group-focus-within:pl-0 peer-valid:pl-0"
              >Answer here 👉</label
            >
          </div>
          <div
            class="mt-3 lg:mt-5 xl:mt-2 flex flex-col gap-y-3 md:gap-y-5 lg:gap-y-5 xl:gap-y-0"
          >
            <div class="md:flex md:flex-row md:justify-center md:gap-x-8">
<!--from pages/game.vue/ when click-->
              <button
                @click="checkAnswer"
                class="w-48 p-2 mb-3 md:mb-8 rounded-xl bg-teal-300 hover:bg-teal-500 text-2xl text-white"
              >
                Submit
              </button>
  <!-- set showAnswer to true -->

              <button
                @click="displayHint1"
                class="w-48 p-2 md:mb-8 rounded-xl bg-yellow-300 hover:bg-yellow-500 text-2xl text-white"
              >
                Hint 1
              </button>
            </div>
            <div class="md:flex md:flex-row md:justify-center md:gap-x-8">
              <button
                @click="displayHint2"
                class="w-48 p-2 mb-3 md:mb-8 rounded-xl bg-yellow-300 hover:bg-yellow-500 text-2xl text-white"
              >
                Hint 2
              </button>
              <button
                @click="giveUpFn"
                class="w-48 p-2 md:mb-8 rounded-xl bg-red-300 hover:bg-red-500 text-2xl text-white"
              >
                Give Up
              </button>
            </div>
            <div class="md:flex md:flex-row md:justify-center md:gap-x-8">
              <RouterLink to="/">
                <button
                  class="w-48 p-2 mb-3 rounded-xl bg-teal-300 hover:bg-teal-500 text-2xl text-white"
                >
                  How to play
                </button>
              </RouterLink>
              <button
                v-on:click="generateUniqueEmojis"
                v-show="showButton"
                class="w-48 p-2 mb-3 rounded-xl bg-white hover:bg-gray-500 text-2xl text-teal-500"
              >
                Restart Game
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { RouterLink, RouterView } from "vue-router";
  
  export default {
    data() {
      return {
        emojis: [],
        emoji: "",
        hint1: "",
        hint2: "",
        giveUp: "",
        answer: "",
        score: 0,
        round: 0,
        randomIndex: null,
        roundEmojis: [],
        inputDisabled: true,
        showButton: false,
        showAnswer: false,
        showHint: false,
        showHint2: false,
        showGiveUp: false,
        showRestart: false,
        showPressSubmit: false,
      };
    },
    methods: {
      correctAlert() {
        let timerInterval;
        this.$swal({
          title: "you're correct!",
          html: "I will close in <b></b> milliseconds.",
          timer: 1000,
          timerProgressBar: true,
          didOpen: () => {
            this.$swal.showLoading();
            const b = this.$swal.getHtmlContainer().querySelector("b");
            timerInterval = setInterval(() => {
              b.textContent = this.$swal.getTimerLeft();
            }, 100);
          },
          willClose: () => {
            clearInterval(timerInterval);
          },
        }).then((result) => {
          if (result.dismiss === this.$swal.DismissReason.timer) {
            console.log("I was closed by the timer");
          }
        });
      },
      // wrongAlert() {
      //   let timerInterval;
      //   this.$swal({
      //     title: "you're wrong!",
      //     html: "I will close in <b></b> milliseconds.",
      //     timer: 1000,
      //     timerProgressBar: true,
      //     didOpen: () => {
      //       this.$swal.showLoading();
      //       const b = this.$swal.getHtmlContainer().querySelector("b");
      //       timerInterval = setInterval(() => {
      //         b.textContent = this.$swal.getTimerLeft();
      //       }, 100);
      //     },
      //     willClose: () => {
      //       clearInterval(timerInterval);
      //     },
      //   }).then((result) => {
      //     if (result.dismiss === this.$swal.DismissReason.timer) {
      //       console.log("I was closed by the timer");
      //     }
      //   });
      // },
      scoreAlert() {
        let timerInterval;
        this.$swal({
          title: `You scored ${this.score} out of 5`,
          html: "I will close in <b></b> milliseconds.",
          timer: 3000,
          timerProgressBar: true,
          didOpen: () => {
            this.$swal.showLoading();
            const b = this.$swal.getHtmlContainer().querySelector("b");
            timerInterval = setInterval(() => {
              b.textContent = this.$swal.getTimerLeft();
            }, 100);
          },
          willClose: () => {
            clearInterval(timerInterval);
          },
        }).then((result) => {
          if (result.dismiss === this.$swal.DismissReason.timer) {
            console.log("I was closed by the timer");
          }
        });
      },
  
      async generateUniqueEmojis() {
        this.showRestart = false;
  
        let uniqueIndex = false;
  
        // Make HTTP request to fetch the data
        const response = await fetch(
          "https://warathepj.github.io/vuejs-emoji-convert.json"
        );
        const data = await response.json();
        console.log("//////////data: ", data);
        // array of object
        this.emojis = data.emojis;
  
        while (!uniqueIndex) {
          let randomIndex = Math.floor(Math.random() * data.emojis.length);
  
          if (!this.roundEmojis.includes(randomIndex)) {
            uniqueIndex = true;
            this.emoji = data.emojis[randomIndex].emo;
            this.randomIndex = randomIndex;
            this.roundEmojis.push(randomIndex);
          }
        }
        this.inputDisabled = false; // re-enable input field when done
        this.showButton = false;
        this.genEmoji = false;
  
        console.log("genEmoji from generateUniqueEmojis() : " + this.genEmoji);
      },
  
      displayHint1() {
        let currentEmojisIndex = this.emojis.findIndex(
          (item) => item.emo === this.emoji
        );
        this.hint1 = this.emojis[currentEmojisIndex].hint1;
        this.showHint = true;
        setTimeout(() => {
          this.showHint = false;
        }, 5000);
        console.log("hint1 : " + this.hint1);
        this.genEmoji = true;
        console.log("genEmoji from displayHint1() : " + this.genEmoji);
      },
      displayHint2() {
        let currentEmojisIndex2 = this.emojis.findIndex(
          (item) => item.emo === this.emoji
        );
        this.hint2 = this.emojis[currentEmojisIndex2].hint2;
        this.showHint2 = true;
        setTimeout(() => {
          this.showHint2 = false;
        }, 5000);
        console.log("hint2 : " + this.hint2);
        this.genEmoji = true;
        console.log("genEmoji from displayHint2() : " + this.genEmoji);
      },
  
      giveUpFn() {
        let currentEmojisIndex3 = this.emojis.findIndex(
          (item) => item.emo === this.emoji
        );
        this.giveUp = this.emojis[currentEmojisIndex3].giveUp;
        this.showGiveUp = true;
        setTimeout(() => {
          this.showGiveUp = false;
        }, 5000);
        this.inputDisabled = true; // disable input field
        this.showPressSubmit = true;
  
        console.log("hint2 : " + this.giveUp);
        this.genEmoji = true;
        console.log("genEmoji from giveUpFn() : " + this.genEmoji);
      },
  
      checkAnswer() {
        this.showAnswer = true;

        if (this.answer.toLowerCase() === this.emojiToWord(this.emoji)) {
          this.score++;
          this.correctAlert();
        } else {
          // this.wrongAlert();
          this.showPressSubmit = false;
        }
        this.answer = "";
        if (this.round < 4) {
          this.generateNextRound();
          console.log(this.roundEmojis);
        } else {
          console.log(this.roundEmojis);
  
          this.scoreAlert();
  
          this.roundEmojis = [];
          this.round = 0;
          this.score = 0;
  
          this.showButton = true;
          this.showRestart = true;
          this.inputDisabled = true; // disable input field
        }
      },
  
      emojiToWord(emoji) {
        for (let i = 0; i < this.emojis.length; i++) {
          if (this.emojis[i].emo === emoji) {
            return this.emojis[i].giveUp;
          }
        }
        return "";
      },
  
      generateNextRound() {
        this.round++;
        if (this.round < 5) {
          this.generateUniqueEmojis();
        } else {
          console.log(this.emojiIndexes); // log the array of emoji indexes to the console
        }
      },
  
      resetGame() {
        this.score = 0;
        this.round = 0;
        this.generateUniqueEmojis();
        this.emojiIndexes = []; // clear the array of emoji indexes when the game is reset
      },
  
      // Define a custom directive that focuses an input element
    },
    mounted() {
      this.generateUniqueEmojis();
    },
  };
  </script>
  
  <style scoped>
  .input-field {
    width: 300px;
    height: 40px;
  }
  </style>
  
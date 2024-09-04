<!-- pages/game.vue/ -->

<template>
  <div>
    <p>{{ answer }}</p>
    <input
      type="text"
      id="answer"
      required
      class="w-full h-10 px-4 text-sm peer bg-gray-100 outline-none focus:outline-none input-field"
      v-el:focusInput
      v-model="answer"
      v-bind:disabled="inputDisabled"
    />
<!--from pages/game.vue/, when click-->
    <button
      @click="checkAnswer"
      class="
        w-48 p-2 mb-3 rounded-xl bg-teal-300 
        hover:bg-teal-500 text-2xl text-white"
    >
      Submit
    </button>
    <!-- render answer in pre tag -->
    <pre v-if="showAnswer">{{ answer }}</pre>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const answer = ref("This is the answer!");
    const inputDisabled = ref(false);
    const showAnswer = ref(false);
    const checkAnswer = () => {
      // Your logic to determine the answer goes here
      showAnswer.value = true;
    };
    const data = ref(null);
    // //////////
    
    // <!--from pages/game.vue/ -->
    async function emojis() {
        // Make HTTP request to fetch the data
        const response = await fetch(
          "https://warathepj.github.io/vuejs-emoji-convert.json"
        );
        const data = await response.json();
        console.log("DDDDDDDDDDDDDDDDDdata: ", data);
    }

    onMounted(emojis); 
// change code to composition api
    // //////////

    return {
      answer,
      showAnswer, 
      checkAnswer,
      inputDisabled,
    };
</script>

<style>
</style>
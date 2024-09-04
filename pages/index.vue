<!-- pages/index.vue/ -->
<template>
    <div class="flex flex-col items-center">
      <h1 class="text-[2rem] mb-3">👋🌎👨‍👩‍👧‍👦💬</h1>
  <div class="w-72 p-2 self-center rounded-lg shadow-xl shadow-teal-900 bg-teal-600">

    <h3 class="text-xl mb-5 font-bold text-white">How to Play:</h3>
    <ul class="text-xs text-white">
      <li>
        Guess the meaning of the emoji. For example, 🌟⚔️🚀 means Star Wars.
      </li>
      <li>Type your answer in the input field.</li>
      <li>
        If you are sure that your answer is correct, press the Submit button.
      </li>
      <li>If you want a hint for the first time, press the Hint1 button.</li>
      <li>If you want a second hint, press the Hint2 button.</li>
      <li>
        If you give up, press the Give Up button. The answer will be displayed.
      </li>
      <li>
        If you give up and want to continue playing the game, press the Submit
        button.
      </li>
      <li>When you have played the game 5 times, the round will end.</li>
      <li>
        If you want to play another round of the game, press the Restart Game
        button.
      </li>
    </ul>
</div>
<!--add nuxtlink to pages/index.vue/ -->
<NuxtLink to="/game">
<button 
  class="
    mt-6 p-3 rounded-lg bg-white text-xl text-teal-600 
    shadow-md shadow-teal-900 hover:bg-teal-100">START GAME</button>
  </NuxtLink>
<!-- go to "/game" -->

  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>
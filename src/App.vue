<template>
  <div id="app">

    <div class="mb-6">
      <h1 class="text-center mt-4 page-title text-4xl sm:text-5xl tracking-wide">
        NMR Purity Calculator
      </h1>
      <div class="flex justify-center w-full">
        <img src="calculator.svg" width="230" height="90">
      </div>
    </div>

    <div class="text-center lg:mt-8 mb-12 hidden lg:block">
        Download the app for
        <a
            class="hover:text-green-500 text-orange-500 font-bold underline"
            href="https://github.com/Jhnbrn90/NMR-Purity-Calculator/releases/download/v0.1.0/NMRPurityCalculator-Setup-0.1.0.exe"
        >Windows</a> or
        <a
            class="hover:text-green-500 text-orange-500 font-bold underline"
            href="https://github.com/Jhnbrn90/NMR-Purity-Calculator/releases/download/v0.1.0/NMRPurityCalculator-0.1.0.dmg"
        >Mac OSX</a>
    </div>

    <div class="justify-center hidden" :class="hideCalculator ? 'lg:flex' : ''">
        <button
            @click="hideCalculator = false"
            class="mb-6 -mt-6 bg-green-500 hover:bg-green-400 text-white font-bold py-2 px-4 border-b-4 border-green-700 hover:border-green-500 rounded"
        >Use online</button>
    </div>

    <div id="calculator" class="flex justify-center" :class="hideCalculator ? 'lg:hidden' : ''">
      <purity-calculator />
    </div>

  </div>
</template>

<script>
import PurityCalculator from './components/PurityCalculator.vue';

export default {
  name: 'app',
  components: {
    PurityCalculator
  },
  data() {
      return {
          hideCalculator: true
      }
  }
};
</script>

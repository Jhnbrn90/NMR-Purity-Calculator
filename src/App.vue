<template>
  <div id="app">

    <div class="mb-6">
      <h1 class="text-center mt-4 page-title text-4xl sm:text-5xl tracking-wide">
        NMR Purity Calculator
      </h1>
      <center><img src="nmr_purity.gif" width="150" height="90"></center>
    </div>

    <div class="flex justify-center">
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
  }
};
</script>

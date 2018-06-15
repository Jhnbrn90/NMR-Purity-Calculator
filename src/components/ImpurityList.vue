<template>
  <div class="p-6 text-grey-darkest text-center" v-show="impurities.length > 0">

    <span class="text-xs">
      <a @click="showCalculation" class="text-grey-darkest cursor-pointer no-underline hover:underline tracking-wide">Show calculation?</a>
      <sup class="text-red uppercase"> New</sup>
    </span>

    <!-- <h3 class="step-heading mt-3 text-2xl text-grey-darkest text-center">List of impurities in sample:</h3> -->
    
    <ul class="list-reset mt-6 text-center text-grey-darker">
      <li v-bind:key="i" v-for="(impurity, i) in impurities" class="mb-4">
        <div>
          <span class="text-2xl text-black">&rsaquo;</span> <strong class="text-base tracking wide text-black"> {{ impurity.name }}</strong> &int; = {{ impurity.integral }}
          <span class="cursor-pointer text-red-darker hover:text-red-dark" @click="removeItem(i)">(remove)</span> </div>
        <div>
          <span class="text-sm">
            <span class="text-lg text-green-dark">{{ massPercent(impurity) }}</span> mass%
          &mdash; <span class="text-lg text-grey-darkest">{{ molePercent(impurity) }}</span> mol%</span></div>
      </li>
    </ul>


  </div>
</template>

<script>
export default {
  props: ['impurities', 'molmass', 'molfrac'],

  methods: {
    molePercent(impurity) {
      return (impurity.molfrac / this.molfrac * 100).toFixed(1);
    },

    massPercent(impurity) {
      let fraction = impurity.molfrac / this.molfrac;
      let mass = fraction * impurity.mass;

      return (Number(mass / this.molmass) * 100).toFixed(1);
    },

    removeItem(i) {
      this.$emit('remove-impurity', i);
    },

    showCalculation() {
      this.$modal.show('show-calculation');
    }
  }
};
</script>


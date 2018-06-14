<template>
  <div class="p-6 text-grey-darkest text-center" v-show="impurities.length > 0">
    <h3 class="step-heading text-2xl text-black text-center">List of impurities in sample:</h3>
    <span class="mb-6 text-xs">
      (<a href="#" @click="showCalculation" class="text-grey-darkest no-underline hover:underline">show calculation?</a>)
    </span>
    <ul class="list-reset mt-2 text-center text-grey-darker">
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
  props: ["impurities", "molmass", "molfrac"],

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
      this.$emit("remove-impurity", i);
    },

    showCalculation() {
      this.$modal.show("show-calculation");
    }
  }
};
</script>


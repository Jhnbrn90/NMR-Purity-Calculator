<template>
  <div class="p-6 text-grey-darkest text-center" v-show="impurities.length > 0">
    <h3 class="step-heading text-2xl text-black text-center mb-4">List of impurities in sample:</h3>
    <ul class="list-reset mt-2 text-center text-grey-darkest">
      <li v-bind:key="i" v-for="(impurity, i) in impurities" class="mb-4">
        <div>&bull;
          <strong>{{ impurity.name }}</strong> ( &int; = {{ impurity.integral }})</div>
        <div>
          <span class=" underline">
            <span class="text-lg">{{ massPercent(impurity) }}</span> mass%</span>
          &mdash; {{ molePercent(impurity) }} mol%</div>
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
    }
  }
};
</script>


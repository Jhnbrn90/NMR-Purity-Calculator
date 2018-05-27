<template>
<div>
    <div class="flex justify-center items-center" v-show="impurities.length > 0">
        <div class="masspercentage mr-2">
            <div class="flex justify-center items-center h-100 rounded-full w-32 h-32 border border-green-dark shadow-md mr-2">
                <div class="flex items-top font-hairline text-green-dark tracking-wide">
                    <div class="text-4xl">{{ productMassFraction }}</div>

                </div>
            </div>
            <div class="text-green-dark text-center mt-2 text-lg">mass%</div>
        </div>

        <div class="molpercentage">
            <div class="flex justify-center items-center h-100 rounded-full w-24 h-24 border border-grey-darker shadow-md mr-2">
                <div class="flex items-top font-thin tracking-wide">
                    <div class="text-3xl">{{ productMolFraction }}</div>
                </div>
            </div>
            <div class="text-base text-center mt-2">mol%</div>
        </div>

    </div>

    <impurity-list @remove-impurity="removeItem" :impurities="impurities" :molmass="sumOfMolMasses" :molfrac="sumOfMolfractions" />

</div>
</template>

<script>
import ImpurityList from './ImpurityList';

export default {
  props: ['impurities', 'productMass'],

  components: {
    ImpurityList
  },

  methods: {
    removeItem(i) {
      this.$emit('remove-impurity', i);
    }
  },

  computed: {
    sumOfMolfractions() {
      let sum = 0;

      Object.keys(this.impurities).forEach(impurity => {
        sum += this.impurities[impurity].molfrac;
      });

      return sum + 1;
    },

    sumOfMolMasses() {
      let sum = 0;

      Object.keys(this.impurities).forEach(impurity => {
        let fraction =
          this.impurities[impurity].molfrac / this.sumOfMolfractions;
        sum += fraction * this.impurities[impurity].mass;
      });

      let productMassFraction =
        Number(this.productMass) / this.sumOfMolfractions;

      return sum + productMassFraction;
    },

    productMassFraction() {
      return (
        this.productMass /
        this.sumOfMolfractions /
        this.sumOfMolMasses *
        100
      ).toFixed(1);
    },

    productMolFraction() {
      return (1 / this.sumOfMolfractions * 100).toFixed(1);
    }
  }
};
</script>


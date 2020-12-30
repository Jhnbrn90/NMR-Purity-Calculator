<template>
<div class="sans-serif tracking-normal tracking-wide container mx-auto p-6">
  <div class="flex justify-center">
    <button
      @click="reset"
      class="mb-6 -mt-6 bg-green-500 hover:bg-green-400 text-white font-bold py-2 px-4 border-b-4 border-green-700 hover:border-green-500 rounded"
      :class="productMass ? '' : 'opacity-50 cursor-not-allowed'"
    >Reset Calculation</button>
  </div>

  <product-input @input="updateProductMass" :value="productMass" />

  <purity-output @remove-impurity="removeItem" :productMass="productMass" :impurities="addedComponents" />

  <add-impurity @added-impurity="addImpurity" :impurities="addedComponents" v-show="productMass !== null" />

</div>

</template>

<script>
import ProductInput from "./ProductInput";
import PurityOutput from "./PurityOutput";
import AddImpurity from "./AddImpurity";

export default {
  name: "PurityCalculator",
  components: {
    ProductInput,
    PurityOutput,
    AddImpurity
  },

  data() {
    return {
      productMass: null,
      addedComponents: []
    };
  },

  methods: {
    updateProductMass(value) {
      this.productMass = value;
    },

    addImpurity(entry) {
      this.addedComponents.push(entry);
    },

    removeItem(i) {
      this.addedComponents.splice(i, 1);
    },

    reset() {
      this.addedComponents = [];
      this.productMass = null;
    }
  }
};
</script>

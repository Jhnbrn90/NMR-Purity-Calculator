<template>
<div class="sans-serif tracking-normal tracking-wide container mx-auto p-6">

  <span
  @click="reset"
  class="block text-center mb-6 -mt-6 hover:underline cursor-pointer"
  >Start over?</span>

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

<template>
<div class="sans-serif tracking-normal tracking-wide container mx-auto p-6">

  <product-input @input="updateProductMass" />

  <purity-output :productMass="productMass" :impurities="addedComponents" />

  <add-impurity @added-impurity="addImpurity" :impurities="addedComponents" v-show="productMass !== null" />

</div>

</template>

<script>
import ProductInput from './ProductInput';
import PurityOutput from './PurityOutput';
import AddImpurity from './AddImpurity';

export default {
  name: 'PurityCalculator',
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
    }
  }
};
</script>

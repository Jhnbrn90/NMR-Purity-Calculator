<template>
    <div class="w-screen sm:w-full sm:mt-2 mt-4 text-grey-darkest">
        <div class="py-1 px-3 text-center">
            <h1>Show Calculation</h1>
            <button @click="closeModal" class="hover:text-blue hover:underline text-sm">&times; close</button>

            <p class="mt-4 mb-2">
                <button 
                    @click="toggleFormulas" 
                    :title="showFormulas ? 'Hide formula\'s' : 'Show formula\'s'" 
                    alt="Toggle formula's" 
                    class="hover:text-blue no-underline text-lg font-semibold"
                    display="outline: none;"
                >
                    {{ showFormulas ? '&minus;' : '&plus;' }} Formula's:
                </button> 
            </p>

            <div v-if="showFormulas" class="rounded shadow-md border px-2 md:text-sm md:flex md:flex-wrap md:items-center md:text-left mb-6">
                <div class="text-blue md:w-1/2">
                    <h4 class="inline-block">Mol frac. </h4>

                    <p class="inline-block">$$\ =\ {observed\ integral \over represented\ protons} $$</p>
                </div>            

                <div class="text-orange md:w-1/2">
                    <h4 class="inline-block">Mol% </h4>  

                    <p class="inline-block">$$\ =\ {mol\ fraction \over sum\ of\ mol\ fractions} $$</p>
                </div>            

                <div class="text-pink md:w-1/2">
                    <h4 class="inline-block">Mass frac.</h4>  

                    <p class="inline-block">$$\ =\ {mol\% \times mol.\ weight } $$</p>
                </div>

                <div class="text-green md:w-1/2">
                    <h4 class="inline-block">Mass%</h4>  

                    <p class="inline-block">$$\ =\ {mass\ fraction \over sum\ of\ mass\ fractions } $$</p>
                </div>
            </div> 

            <div class="mb-3 md:flex md:justify-around">
                <div class="mb-6">
                    <span class="text-blue font-semibold block">Sum of mol fractions:</span> 
                    1 (product) + {{ sumOfFractionsMath }} &equals; <span class="text-blue">{{  totalSumOfFractions.toFixed(2) }}</span>
                </div>

                <div class="mb-2">
                    <span class="text-pink font-semibold block">Sum of mass fractions: </span>
                    {{ (this.productmass / this.totalSumOfFractions).toFixed(2) }} (product) + {{ sumOfMassFractionsMath }} &equals; <span class="text-pink">{{ sumOfMassFractions.toFixed(2) }}</span>
                </div>
            </div>
            
            <div class="mb-6 border border-grey-darkest rounded py-2">
                <strong>Product mass:</strong> {{ productmass }} g/mol <br>
                <span class="text-orange">Mol%</span> &equals; 1 / <span class="text-blue">{{ totalSumOfFractions.toFixed(2) }}</span> &equals; <span class="text-orange">{{ (1 / totalSumOfFractions).toFixed(3) *100 }} mol%</span>  <br>
                <span class="text-pink">Mass frac.</span> &equals; <span class="text-orange">{{ (1 / totalSumOfFractions).toFixed(2) }}</span> &times; {{ productmass }} &equals; <span class="text-pink">{{ (productmass * 1 / totalSumOfFractions).toFixed(2) }}</span> <br>
                <span class="text-green">Mass%</span> &equals; <span class="text-pink">{{ (productmass * 1 / totalSumOfFractions).toFixed(2) }}</span> / <span class="">{{ sumOfMassFractions.toFixed(2) }}</span> &equals; <span class="text-green">{{ (((productmass * 1 / totalSumOfFractions)) / sumOfMassFractions).toFixed(3) * 100 }}%</span>
            </div>
        
            You've added the following impurities:
            <ul class="list-reset mt-4">
                <li v-for="(impurity, index) in impurities" :key="index" class="mb-4">
                    <p class="border-t border-black py-2">
                        <strong class="block">{{ impurity.name }}</strong>
                        MW: <span class="text-grey-dark">{{ impurity.mass }}</span> g/mol,
                        representing <span class="text-red">{{ impurity.protons }}</span> proton(s)
                    </p>

                    <p>
                        &int; = 
                        <span class="">{{ impurity.integral }}</span> 
                        protons
                    </p>

                    <p class="mt-2">
                        Mol frac. =
                        <span class="">
                            {{ impurity.integral }}
                        </span> 
                        / 
                        <span class="text-red">
                            {{ impurity.protons }}
                        </span> 

                        &equals; 

                        <span class="text-blue">
                            {{ (impurity.integral / impurity.protons).toFixed(2) }}
                        </span>
                    </p>

                    <p class="mt-1">
                        <span class="text-orange">Mol% =</span>
                        <span class="">
                            {{ (impurity.integral / impurity.protons).toFixed(2) }} 
                        </span>
                        
                        /

                        <span class="text-blue">
                            {{ Number(totalSumOfFractions).toFixed(2) }}
                        </span>
                        
                        &equals;
                        <span class="text-orange">
                            {{ (((impurity.integral / impurity.protons) / totalSumOfFractions) * 100).toFixed(1) }}  mol%
                        </span>
                    </p>

                    <p class="mt-1">
                        <span class="text-pink">Mass frac.</span> &equals; 
                        <span class="">
                            {{ ((impurity.integral / impurity.protons) / totalSumOfFractions).toFixed(3) }}
                        </span>
                        
                        &times;

                        <span class="text-grey-dark">
                            {{ impurity.mass }}
                        </span>
                        
                        &equals;
                        <span class="text-pink">
                            {{ ((((impurity.integral / impurity.protons) / totalSumOfFractions)) * impurity.mass).toFixed(2) }}
                        </span>

                    </p>

                    <p class="mt-1">
                        <span class="text-green">Mass%</span> &equals; 
                        <span class="text-pink">
                            {{ ((((impurity.integral / impurity.protons) / totalSumOfFractions)) * impurity.mass).toFixed(2) }}
                        </span>
                        
                        /

                        <span class="">
                            {{ sumOfMassFractions.toFixed(2) }}
                        </span>
                        
                        &equals;
                        <span class="text-green">
                            {{ (((((impurity.integral / impurity.protons) / totalSumOfFractions)) * impurity.mass).toFixed(2) / sumOfMassFractions.toFixed(2) *100).toFixed(1) }}%
                        </span>

                    </p>


                </li>
            </ul>

            <div class="w-full border border border-grey-lighter"></div>

            <div class="text-xs py-4 w-3/4 mx-auto mb-1 px-2">
                <span class="block mb-1">Disclaimer</span>
                <p class="italic">
                    The above mentioned values may deviate from the actual values due to rounding of the numbers.
                </p>
            </div>

        </div>

    </div>
</template>

<script>
export default {
  props: ['productmass', 'impurities'],

  data() {
    return {
      showFormulas: true
    };
  },

  computed: {
    sumOfFractionsMath() {
      return this.impurities
        .map(impurity => (impurity.integral / impurity.protons).toFixed(2))
        .join(' + ');
    },

    totalSumOfFractions() {
      const sum = this.impurities
        .map(impurity => impurity.integral / impurity.protons)
        .reduce((acc, next) => acc + next);

      return sum * 1 + 1;
    },

    sumOfMassFractionsMath() {
      return this.impurities
        .map(impurity =>
          (
            impurity.integral /
            impurity.protons /
            this.totalSumOfFractions *
            impurity.mass
          ).toFixed(2)
        )
        .join(' + ');
    },

    sumOfMassFractions() {
      let sum = this.impurities
        .map(
          impurity =>
            impurity.integral /
            impurity.protons /
            this.totalSumOfFractions *
            impurity.mass
        )
        .reduce((acc, next) => acc + next);

      return sum * 1 + this.productmass / this.totalSumOfFractions * 1;
    }
  },

  methods: {
    closeModal() {
      this.$modal.hide('show-calculation');
    },

    toggleFormulas() {
      this.showFormulas = !this.showFormulas;
      this.loadMathJax();
    },

    loadMathJax() {
      this.$nextTick(function() {
        MathJax.Hub.Queue(['Typeset', MathJax.Hub]);
      });
    }
  },

  mounted() {
    this.loadMathJax();
  }
};
</script>

<style>
button {
  outline: none !important;
}
</style>

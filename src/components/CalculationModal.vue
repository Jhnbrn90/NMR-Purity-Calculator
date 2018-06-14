<template>
    <div class="w-screen sm:w-full sm:mt-8 mt-4">
        <div class="py-1 px-3 text-center">
            <h1>Show Calculation</h1>
            <p class="mb-6">Mass of your product: {{ productmass }} </p>
        
            You've added the following impurities:
            <ul class="list-reset mt-4">
                <li v-for="(impurity, index) in impurities" :key="index" class="mb-4">
                    <p>
                    &mdash; <strong class="block">{{ impurity.name }}</strong>
                        MW: <span class="text-pink-light">{{ impurity.mass }}</span>,
                        representing <span class="text-red">{{ impurity.protons }}</span> proton(s)
                    </p>

                    <p>
                        &int; = 
                        <span class="text-blue">{{ impurity.integral }}</span> 
                        protons
                    </p>

                    <p class="mt-2">
                        Molfrac:
                        <span class="text-blue">
                            {{ impurity.integral }}
                        </span> 
                        / 
                        <span class="text-red">
                            {{ impurity.protons }}
                        </span> 

                        &equals; 

                        <span class="text-indigo-dark">
                            {{ (impurity.integral / impurity.protons).toFixed(2) }}
                        </span>
                    </p>

                    <p class="mt-1">
                        Molpercent:
                        <span class="text-indigo-dark">
                            {{ (impurity.integral / impurity.protons).toFixed(2) }} 
                        </span>
                        
                        /

                        <span class="text-orange">
                            {{ Number(totalSumOfFractions).toFixed(2) }}
                        </span>
                        
                        &equals;

                       {{ ((impurity.integral / impurity.protons) / totalSumOfFractions).toFixed(3) }} 

                      ({{ (((impurity.integral / impurity.protons) / totalSumOfFractions) * 100).toFixed(1) }}  mol%)
                    </p>

                    <p class="mt-1">
                        MassFrac:
                        <span class="">
                            {{ ((impurity.integral / impurity.protons) / totalSumOfFractions).toFixed(3) }}
                        </span>
                        
                        &times;

                        <span class="text-pink-light">
                            {{ impurity.mass }}
                        </span>
                        
                        &equals;

                        {{ (((impurity.integral / impurity.protons) / totalSumOfFractions).toFixed(3)) * impurity.mass }}

                    </p>


                </li>
            </ul>

            <div class="mb-4 mt-8">
                <u class="block"><strong class="text-orange underline">Sum of the fractions:</strong></u> 
                <span class="text-indigo-dark"> 1 (product) + {{ sumOfFractionsMath }} </span> 
                <span class="text-xl"> &equals; </span> 
                <span class="text-orange text-semibold">
                     {{ Number(totalSumOfFractions).toFixed(2) }} 
                </span>
            </div>

        </div>

    </div>
</template>

<script>
export default {
  props: ["productmass", "impurities"],

  computed: {
    sumOfFractionsMath() {
      return this.impurities
        .map(impurity => (impurity.integral / impurity.protons).toFixed(2))
        .join(" + ");
    },

    totalSumOfFractions() {
      const sum = this.impurities
        .map(impurity => (impurity.integral / impurity.protons).toFixed(2))
        .reduce((acc, next) => Number(acc) + Number(next));

      return Number(sum) + Number(1);
    }
  }
};
</script>

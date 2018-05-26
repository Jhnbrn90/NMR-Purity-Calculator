<template>
  <div class="sans-serif tracking-normal tracking-wide container mx-auto p-6">

    <div class="w-full pt-4 pb-1">
      <h3 class="step-heading text-center text-black mb-4 text-2xl">
        Step 1. What is the Molecular Weight of your product?
      </h3>

      <p class="mb-2 text-xl text-grey-darkest text-center mb-6">
        <input
          v-model="productMass"
          type="number"
          placeholder="123.45"
          class="border-b border-b-2 border-grey-darker w-24 text-center" autofocus>
        g / mol
      </p>
    </div>

    <div class="flex justify-center items-center" v-if="productMass !== null && addedComponents.length > 0">
        <div class="masspercentage mr-2">
          <div class="flex justify-center items-center h-100 rounded-full w-32 h-32 shadow-md mr-2">
              <div class="flex items-top font-thin text-green-dark tracking-wide">
                <div class="text-4xl">{{ productMassPercent | units }}.</div>
                  <div class="text-lg">
                    {{ productMassPercent | decimals }}
                  </div>
                </div>
                </div>
            <div class="text-green-dark text-center mt-2 text-lg">mass%</div>
        </div>

                <div class="molpercentage">
          <div class="flex justify-center items-center h-100 rounded-full w-32 h-32 shadow-md mr-2">
              <div class="flex items-top font-thin tracking-wide">
                <div class="text-4xl">{{ productMolPercent | units }}.</div>
                  <div class="text-lg">
                    {{ productMolPercent | decimals }}
                  </div>
                </div>
                </div>
            <div class="text-lg text-center mt-2">mol%</div>
        </div>

      </div>


       <div class="p-6 text-grey-darkest text-center" v-show="showResults">
        <h3 class="step-heading text-2xl text-black text-center mb-4">List of impurities in sample:</h3>
          <ul class="list-reset mt-2 text-center text-blue">
            <li v-for="impurity in addedComponents" class="mb-4">
              <div>&bull; <strong>{{ impurity.name }}</strong> ( &int; = {{ impurity.integral }})</div>
              <div><span class=" underline"><span class="text-lg">{{ massPercent(impurity) }}</span> mass%</span>
              &mdash; {{ molePercent(impurity) }} mol%</div>
            </li>
          </ul>
      </div>

      <div class="mb-8 -mt-4 flex justify-center py-6 border-t border-grey-light" v-show="productMass !== null">
          <div class="flex flex-col">
            <h3 class="step-heading text-2xl mb-3">Step 2. Add impurities</h3>
            <center>
              <button
                @click="showModal"
                class="min-w-3/5 px-2 text-xl bg-white shadow tracking-wide font-medium rounded-lg py-2 hover:text-white text-grey-darker border border-grey-darkest hover:bg-grey-darkest"
                :disabled="productMass == null || productMass == ''" :class="productMass == null || productMass == '' ? 'opacity-50 cursor-not-allowed' : ''"
                >&plus; Add component</button>
                <p class="pt-2 text-blue-light text-sm tracking-tight" v-show="addedComponents.length === 1">&uparrow; If you need to add more, repeat this step &uparrow;</p>
              </center>
          </div>
      </div>

      <modal name="add-component">

            <div class="h-full max-w-sm sm:max-w-md items-center flex justify-center">
              <div>
                <h3 class="text-center">Add an impurity</h3>
              <div class="flex justify-center p-6">

                <div class="text-center">
                  Select impurity:
                  <select v-model="selectedImpurity" class="border mb-6">
                    <option v-bind:key="i" v-for="(impurity, i) in commonImpurities">{{ impurity }}</option>
                  </select>

                  <div v-if="selectedImpurity === 'Starting Material'">
                    <p class="text-normal">
                      Mass: <input v-model="form.mass" type="text" placeholder="123" class="border-b border-b-2 border-blue-dark w-16 text-center"> g / mol. <br><br>
                      The signal represents <input v-model="form.protons" type="text" placeholder="1" class="border-b border-b-2 border-blue-dark w-10 text-center"> proton(s). <br><br>
                      Integrates for <input v-model="form.integral" type="text" placeholder="0.15" class="border-b border-b-2 border-blue-dark w-12 text-center"> proton(s).
                    </p>
                  </div>

                  <div v-else>
                    <p class="text-normal">The <em class="font-semibold">{{ selectedPeaktype }}</em>
                    (<span v-html="selectedSignal"></span>) integrates for
                      <input @keydown.enter.prevent="addComponent" v-model="form.integral" type="text" placeholder="0.22" class="border-b border-b-2 border-blue-dark w-12 text-center" autofocus>
                    proton(s).</p>
                  </div>

                  <button
                    @click='addComponent'
                    class="mt-8 rounded bg-blue-dark text-white py-2 px-4 w-48 hover:bg-blue"
                    >&plus; Add component</button>

                </div>

              </div>
              </div>
            </div>
      </modal>
        </div>

  </div>
</template>

<script>
export default {
  name: 'PurityCalculator',
  data() {
    return {
      productMass: null,

      form: {},

      selectedImpurity: 'EtOAc',

      lookupImpurities: {
        'Starting Material': {
          protons: null,
          mass: null
        },

        EtOAc: {
          protons: 2,
          mass: 88.105,
          peakType: 'q',
          signal: 'CH2'
        },
        DCM: {
          protons: 2,
          mass: 84.93,
          peakType: 's',
          signal: 'CH2'
        },
        H2O: {
          protons: 2,
          mass: 18.02,
          peakType: 's',
          signal: 'H2O'
        },
        Cyclohexane: {
          protons: 12,
          mass: 84.16,
          peakType: 's',
          signal: 'CH2'
        },
        PhMe: {
          protons: 3,
          mass: 92.14,
          peakType: 's',
          signal: 'CH3'
        },
        Diethylether: {
          protons: 4,
          mass: 74.12,
          peakType: 'q',
          signal: 'CH2'
        }
      },

      addedComponents: []
    };
  },

  computed: {
    commonImpurities() {
      return Object.keys(this.lookupImpurities);
    },
    selectedPeaktype() {
      return this.lookupImpurities[this.selectedImpurity].peakType;
    },

    selectedSignal() {
      let signal = this.lookupImpurities[this.selectedImpurity].signal;
      if (signal == 'H2O') {
        return 'H<sub>2</sub>O';
      }
      let atoms = signal.match(/[a-zA-Z]+/g);
      let stoichiometry = signal.match(/\d+/g);
      return atoms + '<sub>' + stoichiometry + '</sub>';
    },

    protons() {
      return this.lookupImpurities[this.selectedImpurity].protons;
    },

    mass() {
      return this.lookupImpurities[this.selectedImpurity].mass;
    },

    sumOfMolfractions() {
      let sum = 0;

      Object.keys(this.addedComponents).forEach(impurity => {
        sum += this.addedComponents[impurity].molfrac;
      });

      return sum + 1;
    },

    sumOfMolMasses() {
      let sum = 0;

      Object.keys(this.addedComponents).forEach(impurity => {
        let fraction =
          this.addedComponents[impurity].molfrac / this.sumOfMolfractions;
        sum += fraction * this.addedComponents[impurity].mass;
      });

      let productMassFraction =
        Number(this.productMass) / this.sumOfMolfractions;

      return sum + productMassFraction;
    },

    productMassPercent() {
      return (
        this.productMass /
        this.sumOfMolfractions /
        this.sumOfMolMasses *
        100
      ).toFixed(1);
    },

    productMolPercent() {
      return (1 / this.sumOfMolfractions * 100).toFixed(1);
    },

    showResults() {
      return this.addedComponents.length > 0 && this.productMass !== null;
    }
  },

  methods: {
    showModal() {
      this.$modal.show('add-component');
    },
    hideModal() {
      this.$modal.hide('add-component');
    },

    clearForm() {
      (this.form = {}), (this.selectedImpurity = 'EtOAc');
    },

    molFraction(protons, integral) {
      return integral / protons;
    },

    convertCommas(value) {
      return Number(value.replace(',', '.'));
    },

    addComponent() {
      let protons =
        this.selectedImpurity === 'Starting Material'
          ? this.convertCommas(this.form.protons)
          : this.protons;

      let mass =
        this.selectedImpurity === 'Starting Material'
          ? this.convertCommas(this.form.mass)
          : this.mass;

      let entry = {
        name: this.selectedImpurity,
        integral: Number(this.convertCommas(this.form.integral)).toFixed(2),
        protons: protons,
        mass: mass,
        molfrac: this.molFraction(
          protons,
          this.convertCommas(this.form.integral)
        )
      };

      this.addedComponents.push(entry);
      this.hideModal();
      this.clearForm();
    },

    molePercent(impurity) {
      return (impurity.molfrac / this.sumOfMolfractions * 100).toFixed(1);
    },

    massPercent(impurity) {
      let fraction = impurity.molfrac / this.sumOfMolfractions;
      let mass = fraction * impurity.mass;

      return (Number(mass / this.sumOfMolMasses) * 100).toFixed(1);
    }
  },

  filters: {
    decimals(value) {
      return value.split('.')[1];
    },

    units(value) {
      return value.split('.')[0];
    }
  }
};
</script>

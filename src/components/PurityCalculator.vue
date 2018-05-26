<template>
<div class="sans-serif tracking-normal tracking-wide container mx-auto p-6">

  <product-input @input="updateProductMass" />

  <purity-output :molfraction="productMolPercent" :massfraction="productMassPercent" v-if="productMass !== null && addedComponents.length > 0" />




  <div class="p-6 text-grey-darkest text-center" v-show="showResults">
    <h3 class="step-heading text-2xl text-black text-center mb-4">List of impurities in sample:</h3>
    <ul class="list-reset mt-2 text-center text-grey-darkest">
      <li v-bind:key="i" v-for="(impurity, i) in addedComponents" class="mb-4">
        <div>&bull;
          <strong>{{ impurity.name }}</strong> ( &int; = {{ impurity.integral }})</div>
        <div>
          <span class=" underline">
            <span class="text-lg">{{ massPercent(impurity) }}</span> mass%</span>
          &mdash; {{ molePercent(impurity) }} mol%</div>
      </li>
    </ul>
  </div>

  <div class="mb-8 -mt-4 flex justify-center py-6 border-t border-grey-light" v-show="productMass !== null">
    <div class="flex flex-col">
      <h3 class="step-heading text-2xl mb-3">Step 2. Add impurities</h3>
      <center>
        <button @click="showModal" class="min-w-3/5 px-2 text-xl bg-white shadow tracking-wide font-medium rounded-lg py-2 hover:text-white text-grey-darker border border-grey-darkest hover:bg-grey-darkest"
          :disabled="productMass == null || productMass == ''" :class="productMass == null || productMass == '' ? 'opacity-50 cursor-not-allowed' : ''">&plus; Add Impurity</button>
        <p class="pt-2 text-blue-light text-sm tracking-tight" v-show="addedComponents.length === 1">&uparrow; If you need to add more, repeat this step &uparrow;</p>
      </center>
    </div>
  </div>

  <modal name="add-impurity">
    <div class="h-full max-w-sm sm:max-w-md items-center flex -mt-2 justify-center">
      <div>
        <h3 class="text-center text-2xl">Add an impurity</h3>
        <div class="flex justify-center p-6">

          <div class="text-center">
            Compound:
            <select v-model="selectedImpurity" class="border mb-6">
              <option v-bind:key="i" v-for="(impurity, i) in commonImpurities">{{ impurity }}</option>
            </select>

            <div v-if="selectedImpurity === 'Starting Material'">
              <p class="text-normal">
                Mass:
                <input v-model="form.mass" type="text" placeholder="123" class="border-b border-b-2 border-blue-dark w-16 text-center"> g / mol.
                <br>
                <br> The signal represents
                <input v-model="form.protons" type="text" placeholder="1" class="border-b border-b-2 border-blue-dark w-10 text-center"> proton(s).
                <br>
                <br> Integrates for
                <input v-model="form.integral" type="text" placeholder="0.15" class="border-b border-b-2 border-blue-dark w-12 text-center"> proton(s).
              </p>
            </div>

            <div v-else>
              <p class="text-normal">The
                <em class="font-semibold">{{ selectedPeaktype }}</em>
                (<span v-html="selectedSignal"></span>) integrates for
                <input @keydown.enter.prevent="addComponent" v-model="form.integral" type="text" placeholder="0.22" class="border-b border-b-2 border-blue-dark w-12 text-center"
                  autofocus> proton(s).
              </p>
            </div>

            <button @click='addComponent' class="mt-8 rounded-lg bg-blue-dark text-white py-2 px-4 w-48 hover:bg-blue">&plus; Add impurity</button>

          </div>

        </div>
      </div>
    </div>
  </modal>

</div>

</template>

<script>
import ProductInput from './ProductInput';

export default {
  name: 'PurityCalculator',
  components: {
    ProductInput
  },

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

        'Acetic Acid': {
          protons: 3,
          mass: 60.05,
          peakType: 's',
          signal: 'CH3'
        },

        Acetone: {
          protons: 6,
          mass: 58.08,
          peakType: 's',
          signal: 'CH3'
        },

        Acetonitrile: {
          protons: 3,
          mass: 41.06,
          peakType: 's',
          signal: 'CH3'
        },

        'tert-Butanol': {
          protons: 3,
          mass: 74.12,
          peakType: 's',
          signal: 'CH3'
        },

        Cyclohexane: {
          protons: 12,
          mass: 84.16,
          peakType: 's',
          signal: 'CH2'
        },

        DCM: {
          protons: 2,
          mass: 84.93,
          peakType: 's',
          signal: 'CH2'
        },

        Diethylether: {
          protons: 4,
          mass: 74.12,
          peakType: 'q',
          signal: 'CH2'
        },

        DMF: {
          protons: 1,
          mass: 73.09,
          peakType: 's',
          signal: 'CH'
        },

        DMSO: {
          protons: 6,
          mass: 78.13,
          peakType: 's',
          signal: 'CH3'
        },

        Dioxane: {
          protons: 8,
          mass: 88.12,
          peakType: 's',
          signal: 'CH2'
        },

        Ethanol: {
          protons: 2,
          mass: 46.07,
          peakType: 'q',
          signal: 'CH2'
        },

        EtOAc: {
          protons: 2,
          mass: 88.105,
          peakType: 'q',
          signal: 'CH2'
        },

        Hexane: {
          protons: 6,
          mass: 86.17,
          peakType: 't',
          signal: 'CH3'
        },

        Methanol: {
          protons: 3,
          mass: 32.04,
          peakType: 's',
          signal: 'CH3'
        },

        Pentane: {
          protons: 6,
          mass: 72.15,
          peakType: 't',
          signal: 'CH3'
        },

        '2-Propanol': {
          protons: 6,
          mass: 60.09,
          peakType: 'd',
          signal: 'CH3'
        },

        THF: {
          protons: 4,
          mass: 72.1,
          peakType: 'm',
          signal: 'CH2'
        },

        H2O: {
          protons: 2,
          mass: 18.02,
          peakType: 's',
          signal: 'H2O'
        },

        PhMe: {
          protons: 3,
          mass: 92.14,
          peakType: 's',
          signal: 'CH3'
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

      if (signal.match(/\d+/g) == null) {
        return signal;
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
    // Open and Close the Vue modal
    showModal() {
      this.$modal.show('add-impurity');
    },
    hideModal() {
      this.$modal.hide('add-impurity');
    },

    updateProductMass(value) {
      this.productMass = value;
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
  }
};
</script>

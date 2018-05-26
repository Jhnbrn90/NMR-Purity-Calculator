<template>
  <div class="sans-serif tracking-normal tracking-wide">
    <p class="mb-2 text-xl">
      Mass of product:
        <input v-model="productMass" type="text" placeholder="123" class="border-b border-b-2 border-blue-dark w-16 text-center" autofocus>
        g / mol
      </p>

      <p class="mb-8" v-if="productMass !== null && addedComponents.length > 0">
        Purity: <span class="text-2xl text-blue">{{ productMassPercent }}</span> mass% ({{ productMolPercent }} mol%)
      </p>

      <p class="mb-6">
        The product contains the following components:
          <ul class="list-reset justify-around mt-4">
            <li class="mb-2" v-for="impurity in addedComponents">
              &mdash; <strong>{{ impurity.name }}</strong> (<span class="text-grey-darkest"> &int; = {{ impurity.integral }}</span>)
              &bull; <span class="text-lg">{{ massPercent(impurity) }}</span> mass%
              &bull; {{ molePercent(impurity) }} mol%
            </li>
          </ul>
      </p>

      <p class="text-center">
        <button @click="showModal" class="text-white bg-blue rounded px-4 py-2 hover:bg-blue-dark">Add component</button>
      </p>

      <modal name="add-component">
          <div class="h-full items-center flex justify-center">
            <div>
              <h3 class="text-center">Add a new component</h3>
            <div class="flex justify-center p-6">

              <div class="text-center">
                Select component:
                <select v-model="selectedImpurity" class="border mb-6">
                  <option v-bind:key="i" v-for="(impurity, i) in commonImpurities">{{ impurity }}</option>
                </select>

                <div v-if="selectedImpurity === 'Starting Material'">
                  <p class="text-normal">
                    Mass: <input v-model="form.mass" type="text" placeholder="123" class="border-b border-b-2 border-blue-dark w-12 text-center"> g / mol. <br><br>
                    The signal represents <input v-model="form.protons" type="text" placeholder="1" class="border-b border-b-2 border-blue-dark w-6 text-center"> proton(s). <br>
                    Integrates for <input v-model="form.integral" type="text" placeholder="0.15" class="border-b border-b-2 border-blue-dark w-10 text-center"> proton(s).
                  </p>
                </div>

                <div v-else>
                  <p class="text-normal">The <em class="font-semibold">{{ selectedPeaktype }}</em>
                  (<span v-html="selectedSignal"></span>) integrates for
                    <input @enter="addComponent" v-model="form.integral" type="text" placeholder="0.22" class="border-b border-b-2 border-blue-dark w-10 text-center" required autofocus>
                  proton(s).</p>
                </div>

                <button @click='addComponent' class="mt-8 rounded bg-green text-white py-2 px-4 w-48 hover:bg-green-dark">Add to list</button>

              </div>

            </div>
            </div>
          </div>
      </modal>

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

    addComponent() {
      let protons =
        this.selectedImpurity === 'Starting Material'
          ? this.form.protons
          : this.protons;

      let mass =
        this.selectedImpurity === 'Starting Material'
          ? this.form.mass
          : this.mass;

      let entry = {
        name: this.selectedImpurity,
        integral: this.form.integral,
        protons: protons,
        mass: mass,
        molfrac: this.molFraction(protons, this.form.integral)
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
      console.log(mass);
      return (Number(mass / this.sumOfMolMasses) * 100).toFixed(1);
    }
  }
};
</script>

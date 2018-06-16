<template>
    <div class="w-screen sm:w-full sm:mt-8 mt-6">
        <h3 class="text-center text-2xl mb-2">Add new component</h3>
        <div class="flex sm:flex-row flex-col justify-center w-screen sm:w-full p-4 items-center">
            <label for="impurityselector" class="mr-2 sm:mb-0 mb-2">Select impurity:</label>
            <select id="impurityselector" v-model="selectedImpurity" class="border w-2/5 sm:flex-initial sm:w-2/5">
                <option v-bind:key="i" v-for="(impurity, i) in commonImpurities">{{ impurity }}</option>
            </select>
        </div>
            <center><span class="w-1/2 md:w-full text-grey-darker block -mt-2 text-center text-xs">Add your own impurities, choose <em>Custom Input</em> &#10548;</span></center>

        <div
        v-if="selectedImpurity === '=== Custom Input ==='"
        class="flex flex-col justify-center w-screen sm:w-full p-4 items-center">
          <p class="mb-6 text-center">Define a custom input, <em>e.g.</em> your starting material.</p>
            <div class="text-center mb-2">
                Name: <input v-model="form.name" type="text" placeholder="1,4-Diiodobutane" class="border-b border-b-2 border-blue-dark w-48 text-center">
            </div>

            <div class="text-center mb-2">
                Mass: <input v-model="form.mass" type="number" placeholder="123.45" class="border-b border-b-2 border-blue-dark w-24 text-center"> g / mol.
            </div>

            <div class="text-center mb-2">
                The signal represents
                <input v-model="form.protons" type="number" placeholder="1" class="border-b border-b-2 border-blue-dark w-10 text-center"> proton(s).
            </div>

            <div class="text-center mb-1">
                Integrates for
                <input v-model="form.integral" type="number" placeholder="0.15" class="border-b border-b-2 border-blue-dark w-16 text-center"> proton(s).
            </div>
        </div>

        <div class="flex text-center justify-center w-screen sm:w-full p-4 items-center" v-else>
            <p>
                The <em class="font-semibold">{{ selectedPeaktype }}</em>  (<span v-html="selectedSignal"></span>)
                integrates for
                <input
                    @keydown.enter.prevent="formSubmitted"
                    v-model="form.integral"
                    type="number"
                    placeholder="0.15"
                    class="border-b border-b-2 border-blue-dark w-16 text-center"
                    autofocus
                > proton(s).
            </p>
        </div>

        <div class="flex justify-center mb-8">
            <button @click='formSubmitted' class="mt-4 rounded-lg text-lg tracking-wide bg-blue-dark text-white py-2 px-4 w-48 hover:bg-blue">&plus; Add impurity</button>
        </div>

    </div>
</template>

<script>
export default {
  data() {
    return {
      lookupImpurities: {
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

        'EtOAc (q)': {
          protons: 2,
          mass: 88.105,
          peakType: 'q',
          signal: 'CH2'
        },

        'EtOAc (s)': {
          protons: 3,
          mass: 88.105,
          peakType: 's',
          signal: 'CH3'
        },

        '=== Custom Input ===': {
          protons: null,
          mass: null
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

        Pyridine: {
          protons: 1,
          mass: 79.1,
          peakType: 'm',
          signal: 'CH'
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

        Toluene: {
          protons: 3,
          mass: 92.14,
          peakType: 's',
          signal: 'CH3'
        },

        Triethylamine: {
          protons: 2,
          mass: 101.19,
          peakType: 'q',
          signal: 'CH2'
        },

        TPPO: {
          protons: 15,
          mass: 278.28,
          peakType: 'm',
          signal: 'CH'
        }
      },
      form: {},
      selectedImpurity: 'EtOAc (s)'
    };
  },

  methods: {
    hideModal() {
      this.$modal.hide('add-impurity');
    },
    convertCommas(value) {
      return Number(value.replace(',', '.'));
    },

    molFraction(protons, integral) {
      return integral / protons;
    },

    clearForm() {
      (this.form = {}), (this.selectedImpurity = 'EtOAc');
    },

    formSubmitted() {
      let protons =
        this.selectedImpurity === '=== Custom Input ==='
          ? this.convertCommas(this.form.protons)
          : this.protons;

      let mass =
        this.selectedImpurity === '=== Custom Input ==='
          ? this.convertCommas(this.form.mass)
          : this.mass;

      let name =
        this.selectedImpurity === '=== Custom Input ==='
          ? this.form.name
          : this.selectedImpurity;

      let entry = {
        name: name,
        integral: Number(this.convertCommas(this.form.integral)).toFixed(2),
        protons: protons,
        mass: mass,
        molfrac: this.molFraction(
          protons,
          this.convertCommas(this.form.integral)
        )
      };
      this.$emit('added-impurity', entry);
      this.hideModal();
      this.clearForm();
      setTimeout(() => {
        window.scrollTo(0, document.body.scrollHeight);
      }, 50);
    }
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
    }
  }
};
</script>

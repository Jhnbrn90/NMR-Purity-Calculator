<template>
<div>
    <div class="mb-8 -mt-8 flex justify-center py-6 ">
        <div class="flex flex-col">
            <h3 class="step-heading text-2xl mb-3" v-if="impurities.length === 0">
              Step 2. Add impurities
            </h3>
            <center>
                <button
                    @click="showModal"
                    class="min-w-3/5 px-2 text-xl bg-white shadow tracking-wide font-medium rounded-lg py-2 hover:text-white text-gray-600 border-2 border-gray-600 hover:bg-gray-600 focus:border-blue-700 focus:shadow-lg focus:border-blue-800 focus:text-blue-500"
                    >&plus; Add Impurity</button>
                <p class="pt-2 text-blue-400 text-sm tracking-tight" v-show="impurities.length === 1">&uparrow; If you need to add more, repeat this step &uparrow;</p>
            </center>
        </div>
    </div>

      <modal name="add-impurity" :scrollable="true" height="auto">
        <impurity-modal @added-impurity="addedImpurity" />
      </modal>
</div>

</template>

<script>
import ImpurityModal from './ImpurityModal.vue';

export default {
  props: ['impurities'],

  components: { ImpurityModal },

  methods: {
    showModal() {
      this.$modal.show('add-impurity');
    },

    addedImpurity(entry) {
      this.$emit('added-impurity', entry);
    }
  }
};
</script>

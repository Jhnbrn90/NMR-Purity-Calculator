<template>
<div>
    <div class="mb-8 -mt-8 flex justify-center py-6 ">
        <div class="flex flex-col">
            <h3 class="step-heading text-2xl mb-3" v-if="impurities.length === 0">Step 2. Add impurities</h3>
            <center>
                <button
                    @click="showModal"
                    class="min-w-3/5 px-2 text-xl bg-white shadow tracking-wide font-medium rounded-lg py-2 hover:text-white text-grey-darker border border-grey-darkest hover:bg-grey-darkest"
                    >&plus; Add Impurity</button>
                <p class="pt-2 text-blue-light text-sm tracking-tight" v-show="impurities.length === 1">&uparrow; If you need to add more, repeat this step &uparrow;</p>
            </center>
        </div>
    </div>

      <modal name="add-impurity" height="auto">
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

<template>
<div class="w-full pt-1 pb-1">
    <h3 class="step-heading text-center text-black mb-4 text-2xl">
        Step 1. What is the Molecular Weight of your product?
    </h3>

    <p class="mb-2 text-xl text-gray-700 text-center mb-6">
        <input v-on:input="$emit('input', $event.target.value)" :value="value" placeholder="123.45" class="border-b border-b border-gray-700 w-24 text-center"
            autofocus> g / mol
    </p>
</div>
</template>

<script>
export default {
  props: ['value']
};
</script>

<template>
<div class="w-full pt-1 pb-1">
    <h3 class="step-heading text-center text-black mb-4 text-2xl">
        Step 1. What is the Molecular Weight of your product?
    </h3>

    <p class="mb-2 text-xl text-grey-darkest text-center mb-6">
        <input v-on:input="$emit('input', $event.target.value)" type="number" placeholder="123.45" class="border-b border-b border-grey-darker w-24 text-center"
            autofocus> g / mol
    </p>
</div>
</template>

<script>
export default {};
</script>

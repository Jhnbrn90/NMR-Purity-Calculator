<template>
    <div class="flex justify-center items-center" >
    <div class="masspercentage mr-2">
      <div class="flex justify-center items-center h-100 rounded-full w-32 h-32 border border-green-dark shadow-md mr-2">
        <div class="flex items-top font-hairline text-green-dark tracking-wide">
          <div class="text-4xl">{{ massfraction }}</div>

        </div>
      </div>
      <div class="text-green-dark text-center mt-2 text-lg">mass%</div>
    </div>

    <div class="molpercentage">
      <div class="flex justify-center items-center h-100 rounded-full w-24 h-24 border border-grey-darker shadow-md mr-2">
        <div class="flex items-top font-thin tracking-wide">
          <div class="text-3xl">{{ molfraction }}</div>
        </div>
      </div>
      <div class="text-base text-center mt-2">mol%</div>
    </div>

  </div>
</template>

<script>
export default {
  props: ['molfraction', 'massfraction']
};
</script>


<template>
  <button
    class="border-2 border-darkBlue py-3 rounded-lg w-[100%] 3xl:text-2xl h-10 3xl:h-[4rem] flex justify-center items-center bg-darkBlue "
    :class="[
      iconName ? 'capitalize' : 'uppercase',
      loading
        ? 'cursor-not-allowed hover:bg-darkBlue'
        : 'text-white hover:text-darkBlue hover:bg-transparent',
    ]"
    @click.prevent="$emit('onSubmit')"
    :disabled="loading"
  >
    <span v-if="loading"><spinning-loader /></span>
    <span
      v-else
      class="flex justify-center items-center gap-4"
      :class="[alignLeft ? 'flex-row-reverse' : '']"
    >
      <Icon v-if="iconName" :icon="iconName" class="w-6 h-6" />{{
        buttonText
      }}</span
    >
  </button>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import SpinningLoader from "@/components/SpinningLoader.vue";
import { Icon } from "@iconify/vue";

defineProps(["buttonText", "loading", "iconName", "alignLeft"]);
defineEmits(["onSubmit"]);
</script>
<template>
  <div
    class="fixed inset-0 bg-[rgba(0,0,0,0.7)] flex justify-center items-center z-10"
    @click.self="isClosable ? $emit('closeModal') : null"
    :class="[isClosable ? 'cursor-pointer' : null]"
  >
    <div
      class="bg-[white] p-12 flex flex-col items-center gap-6 relative"
      :class="[isClosable ? 'cursor-default' : null]"
    >
      <h1
        class="text-xl font-bold capitalize"
        :class="[isClosable ? 'text-[red]' : 'text-[green]']"
      >
        {{ title }}
      </h1>
      <p>{{ description }}</p>
      <div class="flex gap-4">
        <button
          class="border-2 text-white px-12 py-2 text-xl rounded-lg hover:bg-[white] mt-4"
          :class="[
            isClosable
              ? 'bg-[red] border-[red] hover:text-[red]'
              : 'bg-darkBlue border-darkBlue  hover:text-darkBlue',
          ]"
          @click="doConfirm"
        >
          {{ confirmButtonText }}
        </button>
        <button
          v-if="isClosable"
          class="border-2 text-white px-12 py-2 text-xl rounded-lg hover:bg-[white] mt-4 bg-darkBlue border-darkBlue hover:text-darkBlue"
          @click="$emit('closeModal')"
        >
          Cancel
        </button>
      </div>
      <button
        v-if="isClosable"
        class="absolute top-4 right-4"
        @click="$emit('closeModal')"
      >
        <Icon icon="akar-icons:cross" class="text-[black] w-5 h-5" />
      </button>
    </div>
  </div>
</template>

<script setup>
import { Icon } from "@iconify/vue";
import { defineProps, defineEmits } from "vue";
const props = defineProps([
  "isClosable",
  "title",
  "description",
  "confirmButtonText",
  "deleteTagUser",
]);
const emit = defineEmits(["onConfirmButton", "closeModal"]);

const doConfirm = () => {
  if (props.deleteTagUser) {
    emit("onConfirmButton", props.deleteTagUser);
    emit("closeModal");
  } else {
    emit("onConfirmButton");
  }
};
</script>
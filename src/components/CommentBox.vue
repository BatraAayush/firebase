<template>
  <div
    class="fixed inset-0 w-full h-full bg-[rgba(0,0,0,0.6)] flex justify-center items-center cursor-pointer overflow-hidden"
    @click.self="$emit('manageCommentBoxVisibility')"
  >
    <div class="bg-white py-4 rounded-lg shadow-lg cursor-default w-[95%] md:w-[50%] lg:w-[40%] xl:w-[40%]">
      <h3 class="text-center font-bold text-xl mb-4 text-darkBlue">Comments</h3>
      <div v-if="loadingPost" class="h-[15rem]">
        <spinning-loader :large="true" />
      </div>
      <div v-else class="h-[15rem] overflow-y-auto">
        <div
          v-if="singlePost?.comments.length === 0"
          class="text-center text-gray-500"
        >
          <p class="font-bold">No Comments Yet!!!</p>
          <p>Be the first one to Comment...</p>
        </div>
        <div v-else>
          <div v-for="comment in singlePost?.comments" :key="comment.createdAt">
            <single-comment :comment="comment" :single-post="singlePost"/>
          </div>
        </div>
      </div>
      <div class="flex items-center gap-2 mt-4 mx-4">
        <input
          type="text"
          class="border border-darkBlue px-2 py-1 rounded-lg w-[90%]"
          placeholder="Add a Comment"
          v-model="postComment"
          @input="$emit('validate')"
        />{{ console.log() }}
        <button @click="$emit('addComment') ">
          <Icon icon="pepicons-pop:send" class="w-6 h-6 text-darkBlue" />
        </button>
      </div>
      <div class="ml-4 text-red-500 mt-2">{{ addCommentErrorMessage }}</div>
    </div>
  </div>
</template>

<script setup>
import { usePostStore } from "@/stores/postStore";
import { Icon } from "@iconify/vue";
import { storeToRefs } from "pinia";
import { defineEmits, defineProps } from "vue";
import SpinningLoader from "./SpinningLoader.vue";
import SingleComment from "./SingleComment.vue";

defineProps(["addCommentErrorMessage"]);
defineEmits(["manageCommentBoxVisibility", "addComment", "validate"]);

const { postComment, singlePost, loadingPost } = storeToRefs(usePostStore());
</script>
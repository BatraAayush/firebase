<template>
  <div class="bg-lightGray mt-[3rem] 3xl:mt-[5rem]">
    <div
      class="w-[100%] sm:w-[60%] md:w-[50%] m-auto pt-2 pb-8 3xl:pt-4 3xl:pb-16 text-sm md:text-base 3xl:text-2xl"
      v-if="postList.length !== 0"
    >
      <div
        v-for="(post, index) in postList"
        :key="post?.createdAt"
        class="mt-8 bg-white shadow-lg py-4 rounded-lg"
      >
        <div class="m-4 3xl:m-8 flex gap-4 3xl:gap-8 items-center">
          <img
            :src="post?.userDetails?.profilePhoto"
            class="w-8 3xl:w-12 h-8 3xl:h-12 rounded-full object-cover"
          />
          <div class="font-bold capitalize">
            {{ post?.userDetails?.firstName }}
            {{ post?.userDetails?.lastName }}
          </div>

          <div class="text-gray-700 flex items-center gap-2 3xl:gap-4">
            <Icon icon="icon-park-outline:dot" class="text-gray-500" />{{
              getUploadTime(post?.createdAt)
            }}
          </div>
        </div>
        <div
          class="text-center font-bold text-xl 3xl:text-3xl m-4 3xl:m-8 break-words"
        >
          {{ post?.title }}
        </div>
        <img
          :src="post?.postImageUrl"
          class="w-full object-cover max-h-[25rem] 3xl:max-h-[30rem]"
        />
        <div class="description">
          <div
            v-html="post?.description"
            :class="[
              'p-4 3xl:p-8 overflow-hidden max-h-[8.5rem]',
              { 'max-h-none': isExpanded[index] },
            ]"
            :ref="setContentRef(index)"
          ></div>
          <button
            v-if="showButton[index]"
            class="m-4 3xl:m-8 text-blue-500"
            :class="[
              buttonText[index] !== 'Show less...'
                ? 'mt-2 3xl:mt-4'
                : 'mt-0 3xl:mt-0',
            ]"
            @click="() => toggleDescription(index)"
          >
            {{ buttonText[index] }}
          </button>
        </div>
        <div class="ml-4 mb-4">
          <div
            @click="manageCommentBoxVisibility(post?.id)"
            class="flex gap-2 3xl:gap-4 items-center cursor-pointer"
          >
            <Icon icon="uil:comment" class="w-6 h-6" />
            <p>{{ post?.comments.length }} comment{{ post?.comments?.length === 1 ? '' : 's' }}</p>
          </div>
        </div>
        <div v-if="post?.taggedUsers.length !== 0" class="px-4 3xl:px-8">
          <h3 class="font-bold">Tagged Users:</h3>
          <div class="flex gap-2 3xl:gap-4 mt-2 3xl:mt-4 flex-wrap">
            <div
              v-for="user in post?.taggedUsers"
              :key="user?.id"
              class="flex gap-2 3xl:gap-4 items-center border-2 border-darkGray px-3 3xl:px-6 py-1 3xl:py-2 rounded-full"
            >
              <img
                :src="user?.profilePhoto"
                class="w-8 3xl:w-12 h-8 3xl:h-12 rounded-full object-cover"
              />
              <div
                class="font-bold text-xs flex gap-1 sm:text-sm md:text-base 3xl:text-2xl"
              >
                <span>{{ user?.firstName }}</span>
                <span>{{ user?.lastName }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="loadingPosts"
      :class="[
        postList.length === 0
          ? 'mt-[7rem]'
          : 'mt-[4rem] bg-lightGray h-[5rem] m-auto pb-8',
      ]"
    >
      <spinning-loader :large="true" />
    </div>
    <comment-box
      v-if="isCommentBoxVisible"
      @manage-comment-box-visibility="manageCommentBoxVisibility"
      @add-comment="addComment"
      :add-comment-error-message="addCommentErrorMessage"
      @validate="validate"
    />
  </div>
</template>

<script setup>
import { Icon } from "@iconify/vue";
import { storeToRefs } from "pinia";
import SpinningLoader from "@/components/SpinningLoader.vue";
import { usePostList } from "@/composables/postList";
import { usePostStore } from "@/stores/postStore";
import CommentBox from "@/components/CommentBox.vue";
import { onMounted } from "vue";

const { postList, loadingPosts, lastVisible } = storeToRefs(usePostStore());
const { getAllPosts } = usePostStore();

const {
  getUploadTime,
  isExpanded,
  toggleDescription,
  buttonText,
  showButton,
  setContentRef,
  isCommentBoxVisible,
  manageCommentBoxVisibility,
  addComment,
  addCommentErrorMessage,
  validate,
  contentRef,
} = usePostList();

onMounted(async () => {
  postList.value = [];
  lastVisible.value = null;
  await getAllPosts();
  postList.value = usePostStore().postList;
  console.log(postList.value)
  isExpanded.value = new Array(postList.value.length).fill(false);
  showButton.value = new Array(postList.value.length).fill(false);

  contentRef.value.forEach((el, index) => {
    if (el && el.scrollHeight > el.offsetHeight) {
      showButton.value[index] = true;
    }
  });
});
</script>

<style>
.description img {
  width: 5rem !important;
  height: 5rem !important;
  display: inline-block;
}

.description figure {
  display: inline;
}

.description li {
  margin-left: 1.5rem;
}

.description ul {
  list-style: disc;
}

.description ol {
  list-style: decimal;
}
</style>

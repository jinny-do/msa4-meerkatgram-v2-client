<script setup>
import { onBeforeMount, onBeforeUnmount } from "vue";
import { useRoute, useRouter } from "vue-router";
import { usePostShowStore } from "../../store/post/usePostShowStore";
import { useAuthStore } from "../../store/auth/useAuthStore";
import { usePostStatisticsStore } from "../../store/post/usePostStatisticsStore";
import { useMyErrorStore } from "../../store/error/useMyErrorStore";

const route = useRoute();
const router = useRouter();
const postShowStore = usePostShowStore();
const authStore = useAuthStore();
const myErrorStore = useMyErrorStore();
const postStatisticsStore = usePostStatisticsStore();

onBeforeMount(async () => {
  try {
    await postShowStore.getPost(route.params.id);
  } catch (error) {
    myErrorStore.setErrorInfo(error);
    router.replace("/error");
  }
});
onBeforeUnmount(postShowStore.clearPostShow);

const deleteHandle = async () => {
  try {
    await postShowStore.deletePost(route.params.id);
    postStatisticsStore.decrementPostCount();
    alert("게시글 삭제에 성공했습니다.");

    router.replace("/");
  } catch (error) {
    alert("게시글 삭제 실패했습니다.");
  }
};
</script>

<template>
  <div class="container" v-if="postShowStore.post">
    <div
      class="image"
      :style="{ backgroundImage: `url(${postShowStore.post.image})` }"
    ></div>
    <div class="option-box">
      <div class="delete-box">
        <div
          class="delete-icon"
          v-if="
            authStore.userInfo?.role === 'SUPER' &&
            postShowStore.post.userId === authStore.userInfo.id
          "
          @click="deleteHandle"
        ></div>
      </div>
      <div class="like-box">
        <span>1919</span>
        <div class="like-icon"></div>
      </div>
    </div>
    <p class="content">{{ postShowStore.post.content }}</p>
  </div>
</template>

<style scoped>
.container {
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.image {
  padding-top: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.option-box {
  padding: 0 15px;
  display: flex;
  justify-content: space-between;
}

.like-box {
  display: flex;
  gap: 10px;
}

.delete-icon {
  width: 40px;
  height: 50px;
  background-image: url("/icons/trash-can.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.like-icon {
  width: 40px;
  height: 40px;
  background-image: url("/icons/heart-fill.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.content {
  white-space: pre-wrap;
}
</style>

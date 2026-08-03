<script setup>
import { onBeforeMount } from "vue";
import { useRoute, useRouter } from "vue-router";
import { useAuthStore } from "../../store/auth/useAuthStore";
import { usePostStatisticsStore } from "../../store/post/usePostStatisticsStore";
import { useMyErrorStore } from "../../store/error/useMyErrorStore";

// http://localhost:5173/oauth2/callback 이 url로 왔을 때의 처리 / 파라미터에 코드가 있는지 없는지 판별
// 코드가 있으면 에러 처리, 없으면 리이슈 처리로 토큰 새로 받아와 home으로 보내기

const authStore = useAuthStore();
const postStatisticsStore = usePostStatisticsStore();
const myErrorStore = useMyErrorStore();
const route = useRoute();
const router = useRouter();

onBeforeMount(async () => {
  try {
    const { code, message } = route.query;

    // code, message가 없을 때 정상, 있으면 에러
    if (!code) {
      await authStore.reissue();
      postStatisticsStore.getUserPostCount();
      return router.replace("/");
    } else if (code === "E02") {
      alert("다른 방식으로 이미 가입된 회원 입니다.");
      return router.replace("/login");
    } else {
      throw myErrorStore.createErrorRedirection(code, message); // catch로 감
    }
  } catch (error) {
    myErrorStore.setErrorInfo(error);
    return router.replace("/error");
  }
});
</script>
<template></template>

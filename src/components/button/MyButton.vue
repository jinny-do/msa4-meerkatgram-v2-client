<script setup>
const props = defineProps({
  btnType: {
    type: String, // 'button', 'submit', 'reset'
    default: "button",
  },
  btnSize: {
    type: String, // 'big', 'middle', 'small'
    default: "big",
  },
  btnStyle: {
    type: String, // `black`, `gray`, `white`, `kakao`, `google`, `naver`
    default: "",
  },
  btnContent: {
    type: String,
    default: "",
  },
});
</script>

<template>
  <button :type="props.btnType" :class="[props.btnStyle, props.btnSize]">
    {{ props.btnContent }}
  </button>
</template>

<style scoped>
button {
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  cursor: pointer;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.big {
  width: 350px;
  height: 45px;
  font-size: 20px;
}

.middle {
  width: 300px;
  height: 70px;
  font-size: 40px;
}

.small {
  width: 90px;
  height: 45px;
  font-size: 20px;
}

.black {
  background-color: var(--personal-color-black);
  color: var(--personal-color-white);
}

.gray {
  background-color: var(--personal-color-gray);
  color: var(--personal-color-white);
}

.white {
  background-color: var(--personal-color-white);
  color: var(--personal-color-black);
}

.kakao {
  background-image: url("/icons/kakao_login_large_narrow.png");
}
</style>

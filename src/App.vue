<template>
  <div class="main" :class="{ animate: animateApp }">
    <div class="header">
      <h2 :class="{ animate: animateTitle }">{{ titles[currentIndex] }}</h2>
      <button class="btn" @click="showModal = true">Click me 😊</button>
    </div>
    <IOSModal v-model:show="showModal"> </IOSModal>
  </div>

  <div :class="['loading-screen', { 'fade-out': !loading }]">
    <div class="heart"></div>
    <h3>Gimme a sec baby just loading</h3>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import IOSModal from './components/IOSModal.vue'
import titles from './titles.json'

let currentIndex = ref(0)
let loading = ref(true)

const startTitleInterval = () => {
  setInterval(() => {
    if (currentIndex.value === titles.length - 1) {
      currentIndex.value = titles.length - 1
    } else {
      currentIndex.value++
    }
  }, 3000)
}

const showModal = ref(false)
const animateTitle = ref(false)

watch(currentIndex, () => {
  animateTitle.value = true
  setTimeout(() => {
    animateTitle.value = false
  }, 1000)
})

const disableScroll = () => {
  document.body.classList.add('modal-open')
}

const enableScroll = () => {
  document.body.classList.remove('modal-open')
}

watch(showModal, (newValue) => {
  if (newValue) {
    disableScroll()
  } else {
    enableScroll()
  }
})

onMounted(() => {
  setTimeout(() => {
    loading.value = false
    startTitleInterval()
  }, 5000)
})
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  gap: 1rem;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 100%;
  top: 10%;
  right: 50%;
  transform: translate(50%, 10%);

  h2 {
    font-size: 1.7rem;
    font-weight: 700;
    text-align: center;
    color: #fff;
    transition: color 0.5s;
    padding: 1rem;
    line-height: 1;
  }

  .animate {
    animation: titleAnimation 1s;
  }
}

@keyframes titleAnimation {
  0% {
    transform: translateY(40px);
    transform: scale(1.4);
    opacity: 0;
  }
  100% {
    transform: translateY(0px);
    transform: scale(1);
    opacity: 1;
  }
}

.loading-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #161616;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  transition: opacity 0.5s; /* Added transition property */

  .heart {
    position: relative;
    width: 100px;
    height: 90px;
    margin-top: 10px;
    animation: heartbeat 1s infinite;
  }

  .heart::before,
  .heart::after {
    content: '';
    position: absolute;
    top: 0;
    width: 52px;
    height: 80px;
    border-radius: 50px 50px 0 0;
    background: red;
  }

  .heart::before {
    left: 50px;
    transform: rotate(-45deg);
    transform-origin: 0 100%;
  }

  .heart::after {
    left: 0;
    transform: rotate(45deg);
    transform-origin: 100% 100%;
  }

  @keyframes heartbeat {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.2);
    }
    100% {
      transform: scale(1);
    }
  }

  h3 {
    color: #fff;
    font-size: 1.5rem;
    margin-top: 1rem;
  }

  /* Added new class to fade out the loading screen */
  &.fade-out {
    opacity: 0;
    z-index: -1;
  }
}
</style>

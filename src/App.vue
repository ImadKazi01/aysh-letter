<template>
  <div>
    <button class="btn" @click="showModal = true">Open Message</button>
    <IOSModal v-model:show="showModal"> </IOSModal>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import IOSModal from './components/IOSModal.vue'

const showModal = ref(false)
</script>

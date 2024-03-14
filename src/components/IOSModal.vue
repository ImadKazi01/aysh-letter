<template>
  <div v-if="isVisible" class="modal-backdrop" @click.self="closeModal">
    <div class="modal-content">
      <h2 class="modal-title">{{ data.title }}</h2>
      <p class="modal-copy" :style="{ 'white-space': 'pre-wrap' }">
        {{ data.copy }}
      </p>

      <button class="modal-btn btn" @click="closeModal">✕</button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, defineProps, defineEmits } from 'vue'
import data from '../data.json'

// Props for controlling visibility from outside (optional)
const props = defineProps({
  show: Boolean
})

const isVisible = ref(props.show)
const emits = defineEmits(['update:show'])

const closeModal = () => {
  isVisible.value = false
  emits('update:show', false) // Notify parent component (if "show" prop is used)
}

// Watch for external changes to "show" prop
watch(
  () => props.show,
  (newVal) => {
    isVisible.value = newVal
  }
)
</script>

<style lang="scss" scoped>
@import '../assets/base.scss';

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 1000;
}

.modal-content {
  background: white;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  gap: 20px;
  width: 100%;
  max-height: 90vh;
  border-radius: 1rem 1rem 0 0;
  padding: 2rem;
  padding-bottom: 3rem;
  box-sizing: border-box;
  animation: slideUp 0.3s ease-out;
  overflow-y: auto;
}

.modal-title {
  font-weight: bold;
  margin: 0;
}

.modal-copy {
  margin: 0;
  width: 100%;
  max-height: 75vh;
  margin-bottom: 1rem;
}

.modal-btn {
  border-radius: 50%;
  background-color: var(--vt-c-lilac-dark);
  color: white;
  align-self: flex-end;
  position: fixed;
  bottom: 1rem;
}

@keyframes slideUp {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}
</style>

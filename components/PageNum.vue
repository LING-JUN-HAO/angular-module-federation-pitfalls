<script setup lang="ts">
import { useSlideContext } from '@slidev/client'

const { $page } = useSlideContext()
</script>

<template>
  <div class="page-num">{{ $page }}</div>
</template>

<style scoped>
.page-num {
  position: absolute;
  bottom: 0.6rem;
  right: 1rem;
  font-size: 0.6rem;
  letter-spacing: 0.05em;
  color: #64748b;
  font-family: 'Fira Code', monospace;
  z-index: 50;
  pointer-events: none;
}
</style>

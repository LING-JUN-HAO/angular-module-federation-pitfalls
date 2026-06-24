<script setup lang="ts">
defineProps<{
  steps: string[]
  variant?: 'default' | 'error' | 'success'
}>()
</script>

<template>
  <div class="flex items-start gap-0 flex-wrap mt-6">
    <template v-for="(step, i) in steps" :key="i">
      <v-click>
        <div class="flex items-start gap-0">
          <div
            class="flow-step"
            :class="{
              'step-error': variant === 'error' && i === steps.length - 1,
              'step-success': variant === 'success' && i === steps.length - 1,
            }"
          >
            <span class="step-num">{{ i + 1 }}</span>
            <span class="step-text">{{ step }}</span>
          </div>
          <div v-if="i < steps.length - 1" class="flow-arrow self-center">
            →
          </div>
        </div>
      </v-click>
    </template>
  </div>
</template>

<style scoped>
.flow-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #1e293b;
  border: 1px solid #334155;
  border-radius: 8px;
  padding: 0.5rem 0.8rem;
  min-width: 140px;
  max-width: 180px;
  text-align: center;
  gap: 0.3rem;
}

.step-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: #7dd3fc;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.step-text {
  font-size: 0.78rem;
  color: #cbd5e1;
  line-height: 1.4;
}

.step-error {
  border-color: #f87171;
  background-color: #2d0f0f;
}

.step-error .step-num {
  color: #f87171;
}

.step-success {
  border-color: #86efac;
  background-color: #0d2d1a;
}

.step-success .step-num {
  color: #86efac;
}

.flow-arrow {
  color: #94a3b8;
  font-size: 1.3rem;
  padding: 0 0.3rem;
  align-self: center;
  padding-bottom: 0.2rem;
}
</style>

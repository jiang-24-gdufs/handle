<script setup lang="ts">
import { answers } from '~/answers/list'
import { answer, dayNo } from '~/state'
import { t } from '~/i18n'

function goToRandomNext() {
  const currentDay = dayNo.value
  let nextDay
  do
    nextDay = Math.floor(Math.random() * answers.length) + 1
  while (nextDay === currentDay)

  window.location.href = `/?d=${nextDay}`
}
</script>

<template>
  <button
    btn flex="~ wrap gap-x-2 center" ws-nowrap text-lg font-serif
    p="x3 y1"
    @click="goToRandomNext()"
  >
    <div i-carbon-shuffle text-base />
    {{ t('random-next') }}
  </button>
</template>

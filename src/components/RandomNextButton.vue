<script setup lang="ts">
import { dayNo } from '~/state'
import { answers } from '~/answers/list'
import { t } from '~/i18n'

function goToRandomNext() {
  const currentDay = dayNo.value
  let nextDay
  let attempts = 0
  const maxAttempts = 10 // 防止无限循环

  do {
    nextDay = Math.floor(Math.random() * answers.length) + 1
    attempts++
    // 确保新的一天:
    // 1. 不是当前天
    // 2. 在答案范围内
    // 3. 对应的答案存在
  } while ((nextDay === currentDay
           || nextDay >= answers.length
           || !answers[nextDay]
           || !answers[nextDay][0])
           && attempts < maxAttempts)

  // 如果找到有效答案就跳转
  if (attempts < maxAttempts && answers[nextDay] && answers[nextDay][0]) {
    window.location.href = `/?d=${nextDay}`
  }
  else {
    // 如果没找到,跳转到第一个有效答案
    for (let i = 0; i < answers.length; i++) {
      if (answers[i] && answers[i][0] && i !== currentDay) {
        window.location.href = `/?d=${i}`
        break
      }
    }
  }
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

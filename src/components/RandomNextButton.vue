<script setup lang="ts">
import { dayNo, daySince, isDev } from '~/state'
import { answers } from '~/answers/list'
import { t } from '~/i18n'
import { DAYS_PLAY_BACK } from '~/logic/constants'

function goToRandomNext() {
  const currentDay = dayNo.value

  // 获取所有有效答案的索引
  const validDays = answers.reduce((acc, answer, index) => {
    // 在非开发环境下,只选择在允许范围内的题目
    const isInRange = isDev || (daySince.value - index <= DAYS_PLAY_BACK)
    if (answer && answer[0] && index !== currentDay && isInRange)
      acc.push(index)

    return acc
  }, [] as number[])

  if (validDays.length === 0) {
    console.warn('No valid answers found')
    return
  }

  // 随机选择一个有效的日期
  const randomIndex = Math.floor(Math.random() * validDays.length)
  const nextDay = validDays[randomIndex]

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

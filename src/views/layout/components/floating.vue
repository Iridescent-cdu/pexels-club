<script setup>
import { onMounted } from 'vue'
// 1.导入
import Driver from 'driver.js'
import 'driver.js/dist/driver.min.css'
import steps from './steps'
import { FEEDBACK_URL } from '@/constants'

// 2.初始化
let driver = null
onMounted(() => {
  driver = new Driver({
    // 点击蒙版关闭引导
    allowClose: false,
    // 关闭按钮的文本
    closeBtnText: '关闭',
    // 下一步按钮的文本
    nextBtnText: '下一步',
    // 上一步按钮的文本
    prevBtnText: '上一步',
    // 完成按钮的文本
    doneBtnText: '完成',
  })
})

/**
 * 开始引导
 */
const onGuideClick = () => {
  // 指定引导步骤
  driver.defineSteps(steps)
  // 开始引导
  driver.start()
}

/**
 * 用户反馈
 */
const onToFeedback = () => {
  window.open(FEEDBACK_URL, '_blank')
}
</script>

<template>
  <div class="fixed bottom-10 right-2">
    <!-- 引导页 -->
    <div
      class="guide-start group w-4 h-4 mb-1 bg-white dark:bg-zinc-900 border dark:border-0 border-zinc-200 rounded-full flex justify-center items-center cursor-pointer duration-200 hover:shadow-lg"
      @click="onGuideClick">
      <m-svg-icon
        name="guide"
        class="w-2 h-2"
        fill-class="fill-zinc-900 dark:fill-zinc-200 group-hover:fill-main "></m-svg-icon>
    </div>
    <!-- 反馈 -->
    <m-popover
      class="guide-feedback flex items-center"
      placement="top-left">
      <template #reference>
        <div
          class="group w-4 h-4 mb-1 bg-white dark:bg-zinc-900 border dark:border-0 border-zinc-200 rounded-full flex justify-center items-center cursor-pointer duration-200 hover:shadow-lg">
          <m-svg-icon
            name="feedback"
            class="w-2 h-2"
            fill-class="fill-zinc-900 dark:fill-zinc-200 group-hover:fill-main "></m-svg-icon>
        </div>
      </template>

      <div class="w-[140px] overflow-hidden">
        <div
          class="flex items-center p-1 cursor-pointer rounded hover:bg-zinc-100/60 dark:hover:bg-zinc-800"
          @click="onToFeedback">
          <m-svg-icon
            name="feedback"
            class="w-1.5 h-1.5 mr-1"
            fill-class="fill-zinc-900 dark:fill-zinc-300"></m-svg-icon>
          <span class="text-zinc-800 dark:text-zinc-300 text-sm">立即吐槽</span>
        </div>
      </div>
    </m-popover>
  </div>
</template>

<style scoped lang="scss">
.driver-fix-stacking {
  position: fixed;
  z-index: 100004 !important;
}
</style>

<!-- eslint-disable vue/component-tags-order -->
<script>
const EMITS_CLICK = 'click'
// type可选项：表示按钮风格
const typeEnum = {
  primary:
    'text-white dark:bg-zinc-900 bg-zinc-800 hover:bg-zinc-900 active:bg-zinc-800 dark:hover:bg-zinc-700 dark:active:bg-zinc-700',
  main: 'text-white  bg-main dark:bg-zinc-900 hover:bg-hover-main hover:bg-zinc-900 active:bg-main dark:active:bg-zinc-700',
  info: 'text-zinc-800 bg-zinc-200 hover:bg-zinc-300 active:bg-zinc-200 dark:text-zinc-300 dark:bg-zinc-700 dark:hover:bg-zinc-600 dark:active:bg-zinc-700',
}
// size可选项：表示按钮的大小，区分文字按钮和icon按钮
const sizeEnum = {
  // 文字按钮
  default: {
    button: 'w-8 h-4 text-base',
    icon: '',
  },
  // icon按钮
  'icon-default': {
    button: 'w-4 h-4',
    icon: 'w-1.5 h-1.5',
  },
  small: {
    button: 'w-7 h-3 text-base',
    icon: '',
  },
  'icon-small': {
    button: 'w-3 h-3',
    icon: 'w-1.5 h-1.5',
  },
}
</script>

<script setup>
// eslint-disable-next-line import/first
import { computed } from 'vue'
// eslint-disable-next-line import/first
import mSvgIcon from '../svg-icon/index.vue'

/**
 * 1.构建type风格可选项和size大小可选项
 * 2.通过props让开发者控制按钮
 * 3.区分icon button和text button
 * 4.依据当前的数据，实现视图
 * 5.处理点击事件
 */
const props = defineProps({
  // icon图标
  icon: String,
  // icon颜色
  iconColor: String,
  // icon图标类名（tailwind）
  iconClass: String,
  // 按钮风格
  type: {
    type: String,
    default: 'main',
    validator(val) {
      // 1.获取所有的可选项
      const keys = Object.keys(typeEnum)
      // 2.开发者指定的风格在可选项中吗
      const result = keys.includes(val)
      if (!result) {
        throw new Error(`你的type必须是${keys.join('、')}中的一个`)
      }
      return result
    },
  },
  // 大小风格
  size: {
    type: String,
    default: 'default',
    validator(val) {
      // 1.获取所有的可选项
      const keys = Object.keys(sizeEnum).filter((key) => {
        return !key.includes('icon-')
      })
      // 2.开发者指定的风格在可选项中吗
      const result = keys.includes(val)
      if (!result) {
        throw new Error(`你的size必须是${keys.join('、')}中的一个`)
      }
      return result
    },
  },
  // 按钮点击时，是否需要动画
  isActiveAnim: {
    type: Boolean,
    default: true,
  },
  // 加载状态
  loading: {
    type: Boolean,
    default: false,
  },
})

const emits = defineEmits([EMITS_CLICK])

// 处理props.size
const sizeKey = computed(() => {
  return (props.icon && `icon-${props.size}`) || props.size
})

/**
 * 点击事件处理,相当于实现一个组件身上的点击事件
 */
const onBtnClick = () => {
  if (props.loading) {
    return
  }
  emits(EMITS_CLICK)
}
</script>

<template>
  <button
    class="text-sm text-center rounded duration-150 flex justify-center items-center"
    :class="[typeEnum[type], sizeEnum[sizeKey].button, { 'active:scale-105': isActiveAnim }]"
    @click.stop="onBtnClick">
    <!-- loading -->
    <m-svg-icon
      v-if="loading"
      name="loading"
      class="w-2 h-2 animate-spin mr-1"></m-svg-icon>
    <!-- icon按钮 -->
    <m-svg-icon
      v-if="icon"
      :name="icon"
      class="m-auto"
      :class="sizeEnum[sizeKey].icon"
      :color="iconColor"
      :fill-class="iconClass">
    </m-svg-icon>

    <!-- 文字按钮 -->
    <slot v-else></slot>
  </button>
</template>

<style scoped lang="scss"></style>

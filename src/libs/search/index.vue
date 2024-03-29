<script>
// 双向绑定
const EMIT_UPDATE_MODELVALUE = 'update:modelValue'
// search搜索
const EMIT_SEARCH = 'search'
// 删除所有文本
const EMIT_CLEAR = 'clear'
// 输入事件
const EMIT_INPUT = 'input'
// 获取焦点事件
const EMIT_FOCUS = 'focus'
// 失去焦点事件
const EMIT_BLUR = 'blur'
</script>

<script setup>
// eslint-disable-next-line import/first
import { onClickOutside, useVModel } from '@vueuse/core'
// eslint-disable-next-line import/first
import { ref, watch } from 'vue'

/**
 * 1.输入内容实现双向绑定
 * 2.搜索按钮在hover时展示
 * 3.一键清空文本功能
 * 4.触发搜索
 * 5.控制下拉展示区的展示
 * 6.事件处理
 */

const props = defineProps({
  modelValue: {
    require: true,
    type: String
  }
})

const emits = defineEmits([
  EMIT_UPDATE_MODELVALUE,
  EMIT_SEARCH,
  EMIT_CLEAR,
  EMIT_INPUT,
  EMIT_FOCUS,
  EMIT_BLUR
])

const inputValue = useVModel(props)

/**
 * 监听用户输入行为
 */
watch(inputValue, (val) => {
  emits(EMIT_INPUT, val)
})

/**
 * 删除文本
 */
const onclearClick = () => {
  inputValue.value = ''
  emits(EMIT_CLEAR, '')
}

/**
 * 搜索
 */
const onSearchHandler = () => {
  emits(EMIT_SEARCH, inputValue.value)
}

// input是否获取到焦点
const isFocus = ref(false)
const containerTarget = ref(null)

/**
 * 获取到焦点
 */
const onfocusHandler = () => {
  isFocus.value = true
  emits(EMIT_INPUT)
}

/**
 * 失去焦点
 */
const onBlurHandler = () => {
  emits(EMIT_BLUR)
}
/**
 * 点击区域外隐藏
 */
onClickOutside(containerTarget, () => {
  isFocus.value = false
})
</script>

<template>
  <div
    ref="containerTarget"
    class="group w-full relative p-0.5 rounded-xl border-white dark:border-zin-200 duration-500 hover:bg-red-100/40"
  >
    <div>
      <!-- 搜索图标 -->
      <m-svg-icon
        class="w-1.5 h-1.5 absolute translate-y-[-50%] top-[50%] left-2"
        name="search"
        color="#707070"
      ></m-svg-icon>
      <!-- 输入框 -->
      <input
        v-model="inputValue"
        class="block w-full h-[44px] pl-4 outline-0 bg-zinc-100 caret-zinc-400 rounded-xl text-zinc-900 tracking-wide text-sm font-semibold border border-zinc-100 duration-500 focus:border-red-300 group-hover:bg-white group-hover:border-zinc-200 dark:bg-zinc-800 dark:text-zinc-200 dark:border-zinc-700 dark:group-hover:border-zinc-700"
        type="text"
        placeholder="搜索"
        @keyup.enter="onSearchHandler"
        @focus="onfocusHandler"
      />
      <!-- 删除按钮 -->
      <m-svg-icon
        v-show="inputValue"
        name="input-delete"
        class="h-1.5 w-1.5 absolute translate-y-[-50%] top-[50%] right-9 cursor-pointer duration-500"
        @click="onclearClick"
      ></m-svg-icon>
      <!-- 分割线 -->
      <div
        class="opacity-0 h-1.5 w-[1px] absolute translate-y-[-50%] top-[50%] right-[62px] duration-500 bg-zinc-200 group-hover:opacity-100"
      ></div>
      <!-- 搜索按钮（通用组件） -->
      <m-button
        class="opacity-0 absolute tr translate-y-[-50%] top-[50%] right-1 rounded-full duration-500 group-hover:opacity-100"
        icon="search"
        icon-color="#ffffff"
        @click="onSearchHandler"
      ></m-button>
    </div>
    <!-- 下拉区 -->
    <transition name="slide">
      <div
        v-if="$slots.dropdown"
        v-show="isFocus"
        class="max-h-[368px] w-full text-base overflow-auto bg-white dark:bg-zinc-800 absolute z-20 left-0 top-[56px] p-2 rounted border border-zinc-200 dark-border-zinc-600 duration-200 hover:shadow-2xl scrollbar-thin scrollbar-thumb-transparent xl:scrollbar-thumb-zinc-200 xl:dark:scrollbar-thumb-zinc-900 scrollbar-track-transparent"
      >
        <slot name="dropdown"></slot>
      </div>
    </transition>
  </div>
</template>

<style scoped lang="scss">
.slide-enter-active,
.slide-leave-active {
  transition: all 0.5s;
}
.slide-enter-from,
.slide-leave-to {
  opacity: 0;
  transform: translateY(40px);
}
</style>

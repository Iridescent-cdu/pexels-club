<script setup>
import { ref } from 'vue'
import { getThemes } from '@/api/mock/pexels'
import { randomRGB } from '@/utils/color.js'

const themeData = ref({
  big: {},
  list: []
})
/**
 * 获取themes数据
 */
const getThemeData = async () => {
  const { themes } = await getThemes()
  // themeData.value.big = themes[0]
  // themeData.value.list = themes.slice(1, themes.length)
  themeData.value = {
    big: themes[0],
    list: themes.slice(1, themes.length)
  }
}
getThemeData()
</script>

<template>
  <div>
    <div class="text-xs mb-1 text-zinc-400">热门精选</div>
    <div class="flex h-[140px]" v-if="themeData.list.length">
      <!-- 大图 -->
      <div
        class="relative rounded w-[260px] cursor-pointer"
        :style="{
          backgroundColor: randomRGB()
        }"
      >
        <img
          v-lazy
          class="h-full w-full object-cover rounded"
          :src="themeData.big.photo"
          alt=""
        />
        <p
          class="absolute bottom-0 left-0 w-full h-[45%] flex items-center backdrop-blur rounded px-1 text-white duration-300 hover:backdrop-blur-none"
        >
          #{{ themeData.big.title }}
        </p>
      </div>
      <!-- 小图 -->
      <div class="flex flex-wrap flex-1 max-w-[860px]">
        <div
          v-for="item in themeData.list"
          :key="item.id"
          class="h-[45%] w-[260px] text-white text-xs relative ml-1.5 mb-1.5 rounded"
          :style="{
            backgroundColor: randomRGB()
          }"
        >
          <img
            v-lazy
            class="w-full h-full object-cover rounded"
            :src="item.photo"
            alt=""
          />
          <p
            class="backdrop-blur absolute top-0 left-0 w-full h-full flex items-center px-1 rounded cursor-pointer duration-300 hover:backdrop-blur-none"
          >
            #{{ item.title }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss"></style>

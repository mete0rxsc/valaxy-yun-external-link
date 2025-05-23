<script lang="ts" setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { useThemeConfig } from '../composables'
import { useYunAppStore } from '../stores'
import LinkInterceptor from '../components/LinkInterceptor.vue' // 引入链接检测栏组件

const yun = useYunAppStore()
const route = useRoute()
const themeConfig = useThemeConfig()

const isPage = computed(() => route.path.startsWith('/page'))

const showNotice = computed(() => {
  const notice = themeConfig.value.notice
  return notice.enable && (isPage.value ? !notice.hideInPages : true)
})
</script>

<template>
  <LinkInterceptor /> <!-- 使用LinkInterceptor -->
  <YunLayoutWrapper
    class="items-center flex-col"
    :class="{
      'mt-0!': !isPage,
    }"
  >
    <template v-if="themeConfig.banner.enable">
      <template v-if="!isPage">
        <div class="w-full">
          <YunBanner />
          <Transition
            v-if="yun.isNimbo"
            enter-from-class="scale-60"
            enter-to-class="scale-100"
            enter-active-class="transition-300 transition-cubic-bezier-ease-in-out delay-1000"
            appear
          >
            <div class="absolute top-0 left-5 right-5 bottom-0 flex-center">
              <Transition
                enter-from-class="op-0"
                enter-to-class="op-100"
                enter-active-class="transition-800 delay-1000"
                appear
              >
                <YunPrologueSquare class="z-1" />
              </Transition>
            </div>
          </Transition>
          <YunSay v-if="themeConfig.say.enable" w="full" />
        </div>
        <YunPrologue v-if="yun.isNimbo" class="absolute left-0 top-0 right-0 bottom-0" />
      </template>
    </template>
    <div v-else class="h-$yun-nav-height" />

    <YunNotice
      v-if="showNotice"
      class="mb-2 md:mb-6"
      :class="{
        'mt-4': !isPage,
      }"
      :content="themeConfig.notice.content"
    />

    <slot name="board" />

    <slot>
      <RouterView />
    </slot>
  </YunLayoutWrapper>
  <YunFooter />
</template>
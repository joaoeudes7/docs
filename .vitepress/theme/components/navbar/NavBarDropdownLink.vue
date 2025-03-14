<template>
  <div class="nav-dropdown-link" :class="{ open }" @mouseenter="open = true" @mouseleave="open = false">
    <button class="button" :aria-label="item.ariaLabel">
      <span class="button-text">{{ item.text }}</span>
      <uil:angle-down class="h-4 ml-1 text-gray-500 w-4" />
    </button>

    <div class="dialog">
      <div v-for="item in item.items" :key="item.text">
        <NavBarDropdownLinkItem v-if="item.text !== 'separator'" :item="item" />
        <div v-else class="py-1 separator">
          <div class="bg-blue-gray-200 h-1px w-full dark:bg-dark-300"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, ref, watch } from 'vue'
import { useRoute } from 'vitepress'
import type { DefaultTheme } from '../../config'

defineProps<{
  item: DefaultTheme.NavItemWithChildren
}>()

const route = useRoute()
const open = ref(false)

watch(
  () => route.path,
  () => {
    open.value = false
  },
)
</script>

<style scoped lang="postcss">
.nav-dropdown-link {
  @apply relative overflow-visible;
}

.nav-dropdown-link.open .dialog {
  display: flex;
}

.button {
  @apply
    block border-0 px-3 py-2 w-full flex flex-row items-center
    text-left font-$font-family-base font-semibold text-$c-text whitespace-nowrap bg-transparent cursor-pointer
    lg:(px-0 py-5 font-normal text-0.9rem)
    focus:outline-none;
}

.dialog {
  @apply
    hidden flex-col overflow-y-auto py-1.5 max-h-[90vh]
    absolute top-12 left-0 min-w-36 transform -translate-x-4
    bg-white dark:bg-dark-700
    rounded-md border border-blue-gray-200 dark:border-dark-300
    list-none;
}
</style>

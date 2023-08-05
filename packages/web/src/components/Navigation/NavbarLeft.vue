<template>
  <aside class="navbarLeft">
    <div
      class="h-screen w-navbarLeft pt-6 pb-5 flex flex-col bg-primary flex-shrink-0"
    >
      <div class="relative pb-2 flex items-center justify-center">
        <div
          @click="menuHandler"
          class="itemIcon w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0 select-none transition-colors duration-100 cursor-pointer"
        >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
          <path stroke-linecap="round" color="white" stroke-linejoin="round" d="M3.75 6A2.25 2.25 0 016 3.75h2.25A2.25 2.25 0 0110.5 6v2.25a2.25 2.25 0 01-2.25 2.25H6a2.25 2.25 0 01-2.25-2.25V6zM3.75 15.75A2.25 2.25 0 016 13.5h2.25a2.25 2.25 0 012.25 2.25V18a2.25 2.25 0 01-2.25 2.25H6A2.25 2.25 0 013.75 18v-2.25zM13.5 6a2.25 2.25 0 012.25-2.25H18A2.25 2.25 0 0120.25 6v2.25A2.25 2.25 0 0118 10.5h-2.25a2.25 2.25 0 01-2.25-2.25V6zM13.5 15.75a2.25 2.25 0 012.25-2.25H18a2.25 2.25 0 012.25 2.25V18A2.25 2.25 0 0118 20.25h-2.25A2.25 2.25 0 0113.5 18v-2.25z" />
        </svg>
        </div>
      </div>

      <div
        v-for="(item, index) in items"
        :key="index"
        class="relative w-full flex items-center justify-center min-h-[42px]"
      >
        <div
          @click="item.handler"
          v-tippy="item.tooltip"
          class="itemIcon w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0 select-none transition-colors duration-100 cursor-pointer"
        >
          <j-icon class="text-white" :name="item.icon" :size="24"></j-icon>
        </div>
      </div>
      <div class="flex-auto"></div>
      <div
        class="relative w-full flex items-center justify-center min-h-[42px]"
      >
        <div
          v-tippy="currentUser.name"
          class="itemIcon w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0 select-none transition-colors duration-100 cursor-pointer"
        >
          <j-avatar
            :name="currentUser.name"
            :avatarUrl="currentUser.avatarUrl"
            :size="26"
          ></j-avatar>
        </div>
      </div>
      <div
        class="relative w-full flex items-center justify-center min-h-[42px]"
      >
        <div
          v-tippy="`About`"
          class="itemIcon w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0 select-none transition-colors duration-100 cursor-pointer"
        >
          <j-icon
            fill="transparant"
            class="text-white"
            name="help"
            :size="24"
          ></j-icon>
        </div>
      </div>
    </div>
  </aside>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
import eventBus from '@/utils/eventBus'
import { getters } from '../../store'
export default defineComponent({
  setup() {
    const currentUser = computed(getters.currentUser)
    const items = [
      {
        icon: 'search',
        tooltip: 'Search issue',
        handler: () => {
          eventBus.emit('toggle-issue-search', {
            isOpen: true
          })
        }
      },
      {
        icon: 'plus',
        tooltip: 'Create issue',
        handler: () => {
          eventBus.emit('toggle-issue-create', {
            isOpen: true
          })
        }
      }
    ]
    
    const menuHandler = () => {
      eventBus.emit('toggle-app-menu', {
        isOpen: true
      })
    }

    return {
      items,
      currentUser,
      menuHandler
    }
  }
})
</script>

<style lang="scss" scoped>
.itemIcon:hover {
  background: rgb(28, 99, 206);
}
</style>

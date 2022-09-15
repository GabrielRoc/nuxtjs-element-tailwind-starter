<template>
  <div class="bg-gray-100 h-screen overflow-hidden relative lg:p-4">
    <div class="flex items-start">
      <Overlay />
      <side-navigation mobile-position="right" />
      <div
        class="flex flex-col w-full h-screen pl-0 lg:space-y-4"
        :class="state.open ? 'lg:w-full' : 'lg:pl-4 lg:w-99'"
      >
        <TopNavigation />
        <main
          class="h-screen overflow-auto pb-36 pt-8 px-2 md:pb-8 md:pt-4 lg:pt-0"
        >
          <slot />
        </main>
      </div>
    </div>
  </div>
</template>

<script>
import Overlay from './provider/Overlay.vue'
import TopNavigation from './topnavigation/Index.vue'
import SideNavigation from './sidenavigation/Index.vue'

export default {
  name: 'DashboardLayout',
  components: { Overlay, SideNavigation, TopNavigation },
  inject: ['state'],
}
</script>

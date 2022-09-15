<template>
  <li>
    <router-link :to="to">
      <span
        class="
          duration-200
          flex
          font-thin
          items-center
          justify-start
          my-2
          p-4
          transition-colors
          text-gray-500
          uppercase
          w-full
          lg:hover:text-blue-500
        "
        :class="
          $route.path === to &&
          'bg-gradient-to-r border-r-4 border-blue-500 border-r-4 border-blue-500 from-white to-blue-100 text-blue-500'
        "
      >
        <span>
          <slot />
        </span>
        <span class="font-normal mx-4 text-sm">
          {{ title }}
        </span>
      </span>
    </router-link>
  </li>
</template>

<script>
export default {
  name: 'SidenavItem',
  props: {
    to: {
      type: String,
      default: '',
    },
    title: {
      type: String,
      default: '',
    },
  },
}
</script>

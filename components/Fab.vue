<template>
  <v-fab-transition>
    <v-btn
      v-show="fab"
      v-scroll="onScroll"
      :style="{
        bottom: $vuetify.breakpoint.smOnly ? '64px' : ''
      }"
      fab
      medium
      dark
      fixed
      bottom
      right
      color="pink"
      @click="toTop"
    >
      <v-icon large>
        mdi-chevron-double-up
      </v-icon>
    </v-btn>
  </v-fab-transition>
</template>

<script>
export default {
  name: 'AppFab',
  data: () => ({
    fab: false
  }),
  methods: {
    onScroll() {
      if (typeof window === 'undefined') return
      const top = window.pageYOffset || document.documentElement.offsetTop || 0
      this.fab = top > 300
    },
    toTop() {
      this.$router.push({ hash: '' })
      this.$vuetify.goTo(0)
    }
  }
}
</script>

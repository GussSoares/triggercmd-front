<template>
  <v-app-bar
    app
    color="#006784"
    dark
  >
    <v-app-bar-nav-icon @click.stop="showDrawer = !showDrawer"></v-app-bar-nav-icon>
    <div class="d-flex align-center">
      <v-img
        alt="Vuetify Name"
        class="shrink mt-1"
        contain
        min-width="100"
        src="https://www.triggercmd.com/user/images/logoColor.png"
        width="200"
      />
    </div>

    <v-spacer></v-spacer>

    <!-- <v-btn
      href="https://github.com/vuetifyjs/vuetify/releases/latest"
      target="_blank"
      text
    >
      <span class="mr-2">Latest Release</span>
      <v-icon>mdi-open-in-new</v-icon>
    </v-btn> -->
  </v-app-bar>
</template>

<script>

export default {
  props: {
    value: Boolean
  },
  data: () => ({

  }),
  computed: {
    showDrawer: {
      get () {
        return this.value
      },
      set (newValue) {
        this.$emit('input', newValue)
      }
    }
  }
}
</script>

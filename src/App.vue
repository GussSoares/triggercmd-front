<template>
  <v-app>
    <appbar></appbar>
    <drawer></drawer>
    <v-main >
      <router-view />
      <v-btn
        v-show="!hidden"
        color="pink"
        style="margin-bottom: 50px"
        dark
        fixed
        absolute
        bottom
        right
        fab
      >
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-main>
  </v-app>
</template>

<script>
import Drawer from './components/Drawer.vue'
import Appbar from './components/Appbar.vue'
export default {
  name: 'App',
  components: {
    Drawer,
    Appbar
  },
  data: () => ({
    hidden: false
    //
  })
}
</script>

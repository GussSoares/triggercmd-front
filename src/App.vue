<template>
  <v-app>
    <appbar v-model="drawer"></appbar>
    <drawer v-model="drawer"></drawer>
    <v-main>
      <v-container fluid>
        <router-view />
        <v-btn
          v-show="!hidden"
          color="pink"
          dark
          fixed
          bottom
          right
          fab
          elevation="24"
          @click="showModal=true"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-container>

    </v-main>
    <Dialog
      v-model="showModal"
      :dialogTitle="'New Command'"
      :trigger="form.trigger"
      :command="form.command"
      :ground="form.ground"
      :allowParams="form.allowParams"
      :voice="form.voice"
    />
  </v-app>
</template>

<script>
import Drawer from './components/Drawer.vue'
import Appbar from './components/Appbar.vue'
import Dialog from './components/Dialog.vue'
export default {
  name: 'App',
  components: {
    Drawer,
    Appbar,
    Dialog
  },
  data: () => ({
    hidden: false,
    showModal: false,
    drawer: false,
    form: {
      trigger: null,
      command: null,
      ground: null,
      allowParams: false,
      voice: null
    }
    //
  })
}
</script>

<style>
v-alert {
  z-index: 100000;
  padding-top: 90px;
  display: flex;
  position: fixed;
  left: 50%;
  bottom: 50px;
  transform: translate(-50%, -50%);
  margin: 0 auto; /* Without this the box extends the width of the page */
}
</style>

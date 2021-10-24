<template>
  <!-- <hello-world /> -->
  <div style="margin: 30px">
    <v-snackbar
      :timeout="-1"
      :value="teste"
      color="blue-grey"
      absolute
      right
      rounded="pill"
      top
    >
      Lorem ipsum dolor sit amet consectetur.
    </v-snackbar>
    <h1
      class="text-xl"
      v-show="!transiction"
    >
      My Commands
      <v-btn
        icon
        color="secondary"
        class="float-right"
        style="margin-top: 10px"
        @click.stop="transiction = !transiction"
      >
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </h1>
    <v-expand-x-transition>
      <v-text-field
        v-model="search"
        v-show="transiction"
        autofocus
        width="100"
        class="mx-auto"
        placeholder="Search a command..."
        @blur="transiction = !transiction"
      >
        <template v-slot:append>
          <v-btn
            icon
            @click="searchCommands()"
          >
            <v-icon>mdi-magnify</v-icon>
          </v-btn>
        </template>
      </v-text-field>
    </v-expand-x-transition>
    <v-overlay :value="loading">
      <v-progress-circular
        indeterminate
        size="64"
      >
      </v-progress-circular>
    </v-overlay>
    <v-row :loading="loading">
      <v-col
        style="margin: 10px"
        v-for="(item, index) in items"
        :key="index"
      >
        <card
          :avatar="'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png'"
          :ground="item.ground"
          :trigger="item.trigger"
          :command="item.command"
          :voice="item.voice"
          :allowParams="item.allowParams == 'true'"
        ></card>
      </v-col>
    </v-row>
    <div class="text-center ma-2">
      <v-snackbar
        v-model="snackbar.show"
        top
        right
        :timeout="5000"
        :color="snackbar.color"
        elevation="24"
      >
        {{ snackbar.message }}
      </v-snackbar>
    </div>
  </div>
</template>

<script>
import Card from '../components/Card.vue'

const debounce = function debounce (fn, delay) {
  var timeoutID = null
  return function () {
    clearTimeout(timeoutID)
    var args = arguments
    var that = this
    timeoutID = setTimeout(function () {
      fn.apply(that, args)
    }, delay)
  }
}

export default {
  name: 'Home',
  components: {
    Card
  },
  data () {
    return {
      items: [],
      transiction: false,
      search: '',
      loading: false,
      teste: false,
      snackbar: {
        color: '',
        message: '',
        show: false
      }
    }
  },
  created () {
    this.getCommands()

    // evento que é executado ao emitir um reload
    this.$root.$on('reload', () => {
      this.getCommands()
    })
    this.$root.$on('snackbar', (event) => {
      this.snackbar.message = event.message
      this.snackbar.color = event.color
      this.snackbar.show = true
    })
  },
  methods: {
    debounce (fn, delay) {
      var timeoutID = null
      return function () {
        clearTimeout(timeoutID)
        var args = arguments
        var that = this
        timeoutID = setTimeout(function () {
          fn.apply(that, args)
        }, delay)
      }
    },
    getCommands () {
      this.loading = true
      return this.$http('/commands')
        .then(response => {
          this.items = response.data
          this.loading = false
        })
        .catch(err => {
          this.loading = false
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    },
    searchCommands () {
      this.loading = true
      return this.$http(`/command?search=${this.search}`)
        .then(response => {
          this.items = response.data
          this.loading = false
        })
        .catch(err => {
          this.loading = false
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    }
  },
  watch: {
    search: debounce(function (newValue) {
      this.searchCommands()
    }, 500)
  }
}
</script>

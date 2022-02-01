<template>
  <v-card
    outlined
    elevation="3"
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div
          class="text-overline command-wrapper"
          style="white-space: nowrap;"
        >
          <span class="command">{{trigger}}</span>
        </div>
        <v-list-item-title class=" mb-1">
          <span>
            <v-icon>mdi-microphone</v-icon> <i>"{{voice}}"</i>
          </span>
        </v-list-item-title>
        <v-list-item-subtitle>
          <v-chip
            v-if="ground == 'foreground'"
            x-small
            color="success"
          >foreground
          </v-chip>
          <v-chip
            v-else-if="ground == 'background'"
            x-small
            color="dark"
          >background
          </v-chip>
        </v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="44"
        color="grey"
      >
        <v-img :src="avatar"></v-img>
      </v-list-item-avatar>
    </v-list-item>

    <v-card-actions>
      <v-btn
        icon
        small
        color="warning"
        @click.stop="showModal=true"
      >
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn
        icon
        small
        color="error"
        @click.stop="removeCommand()"
      >
        <v-icon>mdi-delete</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn
        icon
        small
        color="success"
        :loading="loading"
        @click.stop="testCommandService()"
      >
        <v-icon>mdi-play</v-icon>
      </v-btn>
    </v-card-actions>
    <Dialog
      v-model="showModal"
      :edit="true"
      :dialogTitle="'Edit Command'"
      :trigger="trigger"
      :command="command"
      :ground="ground"
      :allowParams="allowParams"
      :voice="voice"
    />
    <DialogConfirm ref="confirm" />
  </v-card>
</template>

<script>
import Dialog from './Dialog.vue'
import DialogConfirm from './DialogConfirm.vue'

export default {
  name: 'MaterialCard',
  components: {
    Dialog,
    DialogConfirm
  },
  data () {
    return {
      showModal: false,
      loading: false
    }
  },
  props: {
    avatar: {
      type: String,
      default: ''
    },
    ground: {
      type: String,
      default: ''
    },
    command: {
      type: String,
      default: ''
    },
    trigger: {
      type: String,
      default: ''
    },
    voice: {
      type: String,
      default: ''
    },
    allowParams: {
      type: Boolean,
      default: false
    }
  },
  created () {
    // this.getCommandSize()
    // window.addEventListener('resize', this.getCommandSize)
  },
  destroyed () {
    // window.removeEventListener('resize', this.getCommandSize)
  },
  methods: {
    getCommandSize () {
      const wrapperElement = [...document.getElementsByClassName('command-wrapper')]
      const commandElement = [...document.getElementsByClassName('command')]

      wrapperElement.map((element, index) => {
        console.log(commandElement[index].offsetWidth)
        console.log(wrapperElement[index].offsetWidth)
        if (commandElement[index].offsetWidth > wrapperElement[index].offsetWidth) {
          wrapperElement.classList.add('animated')
        }
      })
    },
    async removeCommand () {
      if (
        await this.$refs.confirm.open(
          'Confirm',
          'Are you sure you want to delete this record?'
        )
      ) {
        this.removeCommandService()
      }
    },
    removeCommandService () {
      return this.$http.delete('/command', {
        data: {
          trigger: this.trigger,
          command: this.command,
          ground: this.ground,
          allowParams: this.allowParams.toString(),
          voice: this.voice
        }
      })
        .then(response => {
          this.show = false
          this.$root.$emit('reload', {})
          this.$root.$emit('snackbar', {
            color: 'success',
            message: `"${this.trigger}" removed successfuly!`
          })
        })
        .catch(err => {
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    },
    testCommandService () {
      this.loading = true
      return this.$http.get(`/test-command?trigger=${this.trigger}`)
        .then(response => {
          this.loading = false
          this.$root.$emit('snackbar', {
            color: 'success',
            message: response.data.msg
          })
        })
        .catch(err => {
          this.loading = false
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    }
  }
}
</script>

<style lang="sass">
.v-card--material
  &__avatar
    position: relative
    top: -64px
    margin-bottom: -32px

    &__heading
      position: relative
      top: -40px
      transition: .3s ease
      z-index: 1

    &.v-card--material--hover-reveal:hover
      .v-card--material__heading
        transform: translateY(-40px)
</style>
<style lang="css">
.animated {
  overflow: hidden;
  width: 11rem;
  white-space: nowrap;
}

.animated > * {
  display: inline-block;
  position: relative;
  animation: 3s linear 0s infinite alternate move;
}

.animated > *.min {
  min-width: 100%;
}

@keyframes move {
  0%,
  25% {
    transform: translateX(0%);
    left: 0%;
  }
  75%,
  100% {
    transform: translateX(-100%);
    left: 100%;
  }
}
</style>

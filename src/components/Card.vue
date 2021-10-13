<template>
  <v-card
    class="mx-auto"
    max-width="344"
    outlined
    elevation="3"
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4 command-wrapper" style="white-space: nowrap;">
          <span class="command">{{command}}</span>
        </div>
        <v-list-item-title class=" mb-1">
          <span>Voice: alexa</span>
        </v-list-item-title>
        <br>
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
      >
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn
        icon
        small
        color="error"
      >
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: 'MaterialCard',

  props: {
    avatar: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: 'success'
    },
    hoverReveal: {
      type: Boolean,
      default: false
    },
    icon: {
      type: String,
      default: undefined
    },
    image: {
      type: Boolean,
      default: false
    },
    inline: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      default: ''
    },
    title: {
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
    }
  },
  computed: {
    classes () {
      return {
        'v-card--material--has-heading': this.hasHeading,
        'v-card--material--hover-reveal': this.hoverReveal
      }
    },
    hasHeading () {
      return Boolean(this.$slots.heading || this.title || this.icon)
    },
    hasAltHeading () {
      return Boolean(this.$slots.heading || (this.title && this.icon))
    }
  },
  created () {
    this.getCommandSize()
    window.addEventListener('resize', this.getCommandSize)
  },
  destroyed () {
    window.removeEventListener('resize', this.getCommandSize)
  },
  methods: {
    getCommandSize () {
      const wrapperElement = [...document.getElementsByClassName('command-wrapper')]
      const commandElement = [...document.getElementsByClassName('command')]

      wrapperElement.map((element, index) => {
        console.log(commandElement[index].offsetWidth)
        console.log(wrapperElement[index].offsetWidth)
        if (commandElement[index].offsetWidth > wrapperElement[index].offsetWidth) {
          console.log('entrou')
          wrapperElement.classList.add('animated')
        }
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

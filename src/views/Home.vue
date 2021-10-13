<template>
  <!-- <hello-world /> -->
  <div style="margin: 30px">
    <h1 class="text-xl">
      OVERLINE
    </h1>
    <v-row>
      <v-col
        style="margin: 10px"
        v-for="item in items"
        :key="item.id"
        xs="6"
        sm="6"
        md="3"
        xl="2"
      >
        <card
          :avatar="item.avatar"
          :ground="item.ground"
          :command="item.command"
        ></card>
      </v-col>
    </v-row>
  </div>

</template>

<script>
import Card from '../components/Card.vue'

export default {
  name: 'Home',
  components: {
    Card
  },
  data () {
    return {
      items: [
        { id: 1, command: 'steam', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'foreground' },
        { id: 2, command: 'set headset', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'background' },
        { id: 3, command: 'desligar notebook', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'background' },
        { id: 4, command: 'set echo dot', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'foreground' },
        { id: 5, command: 'Visualboy', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'foreground' },
        { id: 6, command: 'code', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'foreground' },
        { id: 7, command: 'telegram', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'background' },
        { id: 8, command: 'discord', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'background' },
        { id: 9, command: 'backup terraria', text: 'asdkn asdnasdnajkn', avatar: 'https://connectoricons-prod.azureedge.net/releases/v1.0.1426/1.0.1426.2268/triggercmd/icon.png', ground: 'background' }
      ]
    }
  }
}
</script>

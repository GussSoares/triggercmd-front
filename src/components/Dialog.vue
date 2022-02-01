<template>
  <v-dialog
    v-model="show"
    max-width="290"
  >
    <v-card>
      <v-card-title
        class="text-h5"
        v-if="edit"
      >
        {{dialogTitle}}: <i>{{title}}</i>
      </v-card-title>
      <v-card-title
        class="text-h5"
        v-else
      >
        {{dialogTitle}}
      </v-card-title>

      <v-card-text>
        <v-form
          ref="form"
          v-model="valid"
          lazy-validation
        >
          <v-row>
            <v-col cols>
              <v-text-field
                label="Trigger"
                hide-details="auto"
                v-model="form.trigger"
                :rules="rules"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols>
              <v-text-field
                label="Command"
                hide-details="auto"
                v-model="form.command"
                :rules="rules"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols>
              <v-select
                label="Ground"
                hide-details="auto"
                :items="['foreground', 'background']"
                v-model="form.ground"
                :rules="rules"
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols>
              <v-switch
                v-model="form.allowParams"
                :label="`Allow Params: ${form.allowParams}`"
              ></v-switch>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols>
              <v-text-field
                label="Voice"
                hide-details="auto"
                v-model="form.voice"
                :rules="rules"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn
          color="green darken-1"
          text
          @click="confirm()"
        >
          Save
        </v-btn>

        <v-btn
          color="red darken-1"
          text
          @click="show=false"
        >
          Cancel
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  props: {
    edit: Boolean,
    dialogTitle: String,
    value: Boolean,
    trigger: String,
    command: String,
    ground: String,
    allowParams: Boolean,
    voice: String,
    confirmFunction: Function
  },
  data () {
    return {
      title: this.trigger,
      valid: true,
      form: {
        trigger: this.trigger,
        command: this.command,
        ground: this.ground,
        allowParams: this.allowParams,
        voice: this.voice
      },
      rules: [
        v => !!v || 'Input is required'
      ]
    }
  },
  computed: {
    show: {
      get () {
        return this.value
      },
      set (value) {
        this.$refs.form.resetValidation()
        this.$emit('input', value)
      }
    }
  },
  methods: {
    confirm () {
      if (this.$refs.form.validate()) {
        if (this.edit) {
          this.editCommand()
        } else {
          this.createCommand()
        }
      }
    },
    editCommand () {
      return this.$http.patch(`/command?old_title=${this.trigger}`, {
        trigger: this.form.trigger,
        command: this.form.command,
        ground: this.form.ground,
        allowParams: this.form.allowParams.toString(),
        voice: this.form.voice
      })
        .then(response => {
          this.show = false
          this.$root.$emit('reload', {})
          this.$root.$emit('snackbar', {
            color: 'success',
            message: `"${this.form.trigger}" edited successfuly!`
          })
        })
        .catch(err => {
          this.show = false
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    },
    createCommand () {
      return this.$http.post('/command', {
        trigger: this.form.trigger,
        command: this.form.command,
        ground: this.form.ground,
        allowParams: this.form.allowParams.toString(),
        voice: this.form.voice
      })
        .then(response => {
          this.show = false
          this.$root.$emit('reload', {})
          this.$root.$emit('snackbar', {
            color: 'success',
            message: `"${this.form.trigger}" created successfuly!`
          })
          this.$refs.form.reset()
        })
        .catch(err => {
          this.show = false
          this.$root.$emit('snackbar', {
            color: 'error',
            message: err.toString()
          })
        })
    }
  }
}
</script>

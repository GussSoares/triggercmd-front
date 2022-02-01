<template>
  <div id="app">
    <v-main>
      <v-container >
        <v-layout
          align-center
          justify-center
        >
          <div class="login-form text-xs-center">
            <div class="display-1 mb-3">
              Login
            </div>
            <v-card
              light
              max-width="300px"
            >
              <v-card-text>
                <div class="subheading">
                  <template v-if="options.isLoggingIn">Log in to your customer portal</template>
                  <template v-else>Crate a new account</template>
                </div>
                <v-form>
                  <v-text-field
                    v-if="!options.isLoggingIn"
                    v-model="user.name"
                    light
                    prepend-icon="mdi-account"
                    label="Name"
                  ></v-text-field>
                  <v-text-field
                    v-model="user.email"
                    light
                    prepend-icon="mdi-email"
                    label="Email"
                    type="email"
                  ></v-text-field>
                  <v-text-field
                    v-model="user.password"
                    light
                    prepend-icon="mdi-lock"
                    label="Password"
                    type="password"
                  ></v-text-field>
                  <v-checkbox
                    v-if="options.isLoggingIn"
                    v-model="options.shouldStayLoggedIn"
                    light
                    label="Stay logged in?"
                    hide-details
                  ></v-checkbox>
                  <v-btn
                    dark
                    v-if="options.isLoggingIn"
                    @click.prevent=""
                    block
                    type="submit"
                  >Sign in</v-btn>
                  <v-btn
                    dark
                    v-else
                    block
                    type="submit"
                    @click.prevent="options.isLoggingIn = true"
                  >Sign up</v-btn>
                </v-form>
              </v-card-text>
            </v-card>
            <br>
            <div v-if="options.isLoggingIn">Don't have an account?
              <v-btn
                light
                @click="options.isLoggingIn = false"
              >Sign up</v-btn>
            </div>
          </div>
        </v-layout>
      </v-container>
    </v-main>
    <v-footer app>
      <v-flex class="text-xs-center">© 2021. All rights reserved.</v-flex>
    </v-footer>
  </div>
</template>
<script>
export default {
  name: 'Login',
  data: function () {
    return {
      user: {
        // email: 'admin@example.com',
        // password: 'admin',
        // name: 'John Doe',
      },
      options: {
        isLoggingIn: true,
        shouldStayLoggedIn: true
      }
    }
  }

}
</script>
<style>
</style>

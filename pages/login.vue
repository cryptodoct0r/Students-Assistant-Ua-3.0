<template>
  <div class="mt-5 pt-5">
    <h1 class="text-center">
      Login functionality coming soon!
    </h1>
    <hr>
    <img src="https://www.cpcurling.ca/wp-content/uploads/2017/09/under-construction-gif-15.gif" class="img-fluid ${3|rounded-top,rounded-right,rounded-bottom,rounded-left,rounded-circle,|}" alt="Page under Construction.">
    <b-alert v-if="error" show variant="danger">
      {{ error + '' }}
    </b-alert>
    <b-alert v-if="$auth.$state.redirect" show>
      You have to login before accessing to <strong>{{ $auth.$state.redirect }}</strong>
    </b-alert>
    <b-row align-h="center" class="center pt-4">
      <b-col md="4">
        <b-card bg-variant="light">
          <busy-overlay />
          <form @keydown.enter="login">
            <b-form-group label="Username">
              <b-input ref="username" v-model="username" placeholder="anything" />
            </b-form-group>

            <b-form-group label="Password">
              <b-input v-model="password" type="password" placeholder="123" />
            </b-form-group>

            <div class="text-center">
              <b-btn variant="primary" block @click="login">
                Login
              </b-btn>
            </div>
          </form>
        </b-card>
      </b-col>
      <b-col md="1" align-self="center">
        <div class="text-center">
          <b-badge pill>
            OR
          </b-badge>
        </div>
      </b-col>
      <b-col md="4" class="text-center">
        <b-card title="Social Login" bg-variant="light">
          <div v-for="s in strategies" :key="s.key" class="mb-2">
            <b-btn block :style="{background: s.color}" class="login-button" @click="$auth.loginWith(s.key)">
              Login with {{ s.name }}
            </b-btn>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<style scoped>
.login-button {
  border: 0;
};
</style>

<script>
import busyOverlay from '~/components/busy-overlay'

export default {
  middleware: ['auth'],
  components: { busyOverlay },
  data () {
    return {
      username: '',
      password: '123',
      error: null
    }
  },
  computed: {
    strategies: () => ([
      { key: 'auth0', name: 'Auth0', color: '#ec5425' },
      { key: 'google', name: 'Google', color: '#4284f4' },
      { key: 'facebook', name: 'Facebook', color: '#3c65c4' },
      { key: 'github', name: 'GitHub', color: '#202326' }
    ]),
    redirect () {
      return (
        this.$route.query.redirect &&
        decodeURIComponent(this.$route.query.redirect)
      )
    },
    isCallback () {
      return Boolean(this.$route.query.callback)
    }
  },
  methods: {
    // eslint-disable-next-line require-await
    async login () {
      this.error = null

      return this.$auth.loginWith('local', {
        data: {
          username: this.username,
          password: this.password
        }
      })
        .catch((e) => {
          this.error = e + ''
        })
    }
  }
}
</script>

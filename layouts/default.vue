<template>
  <v-app>
    <v-navigation-drawer disable-resize-watcher v-model="sideDrawer" fixed app>
      <v-list>
        <v-list-item
          v-for="(item, i) in sideMenu"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container fill-height fluid>
        <Nuxt />
      </v-container>
    </v-main>

    <v-bottom-navigation horizontal height="10vh" fixed color="primary" app>
      <v-app-bar-nav-icon
        @click.stop="sideDrawer = !sideDrawer"
        v-ripple="false"
        plain
      />
      <v-btn
        v-for="(item, i) in bottomMenu"
        :key="i"
        :to="item.to"
        v-ripple="false"
        plain
      >
        <span>
          {{ item.title }}
        </span>
        <v-icon>
          {{ item.icon }}
        </v-icon>
      </v-btn>
      <v-spacer />
    </v-bottom-navigation>
  </v-app>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'DefaultLayout',
  data() {
    return {
      sideDrawer: false,
      sideMenu: [
        {
          icon: 'mdi-account',
          title: 'Account',
          to: '/account',
          middleware: 'authenticated',
        },
        {
          icon: 'mdi-bell',
          title: 'Notification',
          to: '/notification',
          middleware: 'authenticated',
        },
        {
          icon: 'mdi-login',
          title: 'Login',
          to: '/login',
          middleware: 'unauthenticated',
        },
        {
          icon: 'mdi-logout',
          title: 'logout',
          to: '/notification',
          middleware: 'authenticated',
        },
      ],
      bottomMenu: [
        {
          icon: 'mdi-application',
          title: 'App',
          to: '/',
        },
      ],
    }
  },
  // computed: {
  //   ...mapGetters('auth', {
  //     authenticated: 'authenticated',
  //   }),
  // },
  methods: {
    isWelcomeScreen() {
      if (!localStorage.welcomeScreen) {
        if (
          this.$router.currentRoute.path != '/register' &&
          this.$router.currentRoute.path != '/login'
        ) {
          this.$router.push('/register')
        }
      }
    },
    // filterSideMenu(){
    //   this.sideMenu(){
    //     this.sideMenu = this.sideMenu.filter(item => {

    //     })
    //   }
    // }
  },
  watch: {
    $route() {
      this.isWelcomeScreen()
    },
  },
  mounted() {
    // localStorage.setItem('welcomeScreen', true)
    this.isWelcomeScreen()
  },
}
</script>

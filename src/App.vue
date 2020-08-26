<template>
  <v-app>
    <v-navigation-drawer
        app
    >
      <v-img
          :src="require('@/assets/logo/gray.svg')"
          max-width="512"
          class="mx-auto logo"
      />
      <v-list>
        <v-list-item link :to="{path: '/'}">
          <v-list-item-icon>
            <v-icon>
              mdi-chevron-left
            </v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="=itemTitle font-weight-bold">
              回到欢迎界面
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item>

        </v-list-item>


      </v-list>
    </v-navigation-drawer>

    <v-app-bar
        height="50"
    >
      <v-app-bar-nav-icon
          class="black-text"
      />
      <v-toolbar-title>
        <div class="toolbarTitle"></div>
      </v-toolbar-title>
      <v-icon
          right
          class="logoIcon"
      >
        wsicon wsicon-logo-icon
      </v-icon>
    </v-app-bar>

    <v-content
        class="mb-8"
    >
      <v-container>
        <transition
            name="slide-fade"
            mode="out-in"
        >

        </transition>
      </v-container>
    </v-content>

    <v-footer
        padless
    >

    </v-footer>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {},

  data: () => ({}),
};
</script>

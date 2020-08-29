<template>
  <v-app
      id="__app_root"
      :dark="dark"
      :style="{'background': $route.name === 'home' ? '#f0f0f0' : null }"
  >
    <v-navigation-drawer
        app
        v-model="drawer"
        color="white"
        clipped
        width="254"
        class="navigationDrawer"
        :disable-resize-watcher="$route.meta.hideDrawer"
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
        <template v-for="route in routes">
          <v-list-item
              v-if="!route.children"
              :key="route.name"
              link
              :to="{name: route.name}"
              class="listItem"
              active-class="listItemActive font-weight-bold white--text"
          >
            <v-list-item-icon
                class="listItemIcon"
                :class="route.meta.classes"
            >
              <v-icon
                  size="14"
                  v-text="route.meta.icon"
              />
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="itemTitle font-weight-bold">
                {{ route.meta.name }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-group
              v-else
              :key="route.name"
              :value="route.meta.active"
              :prepend-icon="route.meta.icon"
              no-action
          >
            <template v-slot:activator>
              <v-list-item-title>{{ route.meta.title }}</v-list-item-title>
            </template>

            <v-list-item
                v-for="child in route.children.filter(el => !el.meta.hide)"
                :key="child.name"
                :to="{name: child.name}"
                exact
                link
                class="listItem"
                active-class="listItemActive font-weight-bold white--text"
            >
              <v-list-item-title>{{ child.meta.title }}</v-list-item-title>

              <v-list-item-icon>
                <v-icon
                    class="listItemIcon"
                    :class="route.meta.classes"
                    v-text="child.meta.icon"
                />
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>
        </template>


      </v-list>
    </v-navigation-drawer>

    <v-app-bar
        v-if="!$route.meta.hideDrawer"
        app
        fixed
        clipped-left
        height="50"
        color="white"
        class="appBar"
    >
      <v-app-bar-nav-icon
          class="black-text"
          @click.stop="drawer = !drawer"
      />
      <v-toolbar-title
          dark
          class="toolbarTitle"
      >
        <div class="toolbarTitle">
          {{ $router.currentRoute.meta.title }}
        </div>
      </v-toolbar-title>
      <v-icon
          class="logoIcon"
          right
          size="30"
          color="#a20002"
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
          <router-view/>
        </transition>
      </v-container>
    </v-content>

    <v-footer
        padless
    >
      <v-card
          flat
          tile
          class="text-center pb-6"
          :class="{'grey lighten-2': $route.name !== 'psychologicalPlatform', 'green lighten-1': $route.name === 'psychologicalPlatform'}"
          style="width: 100%"
      >
        <v-card-text>
          <template v-if="$route.name === 'psychologicalPlatform'">
            <h1 class="overline mb-4">
              这个世界并不完美<br>
              但我们正在让她更好
            </h1>
          </template>
          <template v-else>
            <h1 class="overline mb-4">
              李文亮医生千古
            </h1>
          </template>

          <v-row
              align="center"
              justify="center"
              class="mb-1"
          >
            <a
                href="https://wuhan.support/eula/"
                target="_blank"
                class="font-weight-bold secondary--text text--darken-2"
                style="text-decoration: none;"
            >
              最终用户许可协议
            </a>

            <v-divider
                vertical
                class="mx-3"
            />

            <a
                href="https://wuhan.support/privacy/"
                target="_blank"
                class="font-weight-bold secondary--text text--darken-2"
                style="text-decoration: none;"
            >
              隐私声明
            </a>
          </v-row>

          <span><a
              href="https://wuhan.support/"
              target="_blank"
              style="text-decoration: none;"
          ><strong>wuhan.support 团队</strong></a></span>

          <br>
          <v-btn
              small
              outlined
              class="mt-1"
              href="https://github.com/wuhan-support/"
          >
            <v-icon left>
              mdi-github-circle
            </v-icon>
            项目源代码
          </v-btn>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      routes: [],
      rawDrawer: !this.$vuetify.breakpoint.smAndDown,
    };
  },
  computed: {
    dark: {
      get() {
        return this.$store.state.settings.dark;
      },
      set(value) {
        this.$store.commit("switchDark", value);
        this.$vuetify.theme.dark = value;
      },
    },
    drawer: {
      get() {
        return this.rawDrawer && !this.$route.meta.hideDrawer
      },
      set(value) {
        this.rawDrawer = value
      }
    }
  },
  watch: {
    dark: ["onDarkChange"]
  },
  beforeMount() {
    // console.log(this.$router)
    this.routes = this.$router.options.routes.filter(el => !el.meta.hide);
  },
  mounted() {
    this.onDarkChange(this.$store.state.settings.dark);
  },
  methods: {
    onDarkChange(newValue) {
      if (newValue) {
        document.body.style.backgroundColor = "#303030";
      } else {
        document.body.style.backgroundColor = "#fafafa";
      }
    }
  }
};
</script>

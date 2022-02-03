<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      width="300px"
    >
      <v-list-item two-line>
        <v-list-item-avatar>
          <v-img src="/bksc_logo.png"></v-img>
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title> <h3>โรงเรียนบึงกาฬ</h3> </v-list-item-title>
          <v-list-item-subtitle>
            ระบบลงทะเบียนชุมนุมออนไลน์ ( Unofficial )</v-list-item-subtitle
          >
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <b-icon :icon="item.icon"></b-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>{{ title }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        fab
        depressed
        small
        @click="$vuetify.theme.dark = !$vuetify.theme.dark"
        ><b-icon
          :icon="$vuetify.theme.dark ? 'sun' : 'moon'"
          class="v-icon notranslate"
        ></b-icon
      ></v-btn>
    </v-app-bar>
    <v-main class="mb-5">
      <Nuxt />
    </v-main>
    <v-footer fixed>
      <v-spacer></v-spacer>
      design and coding with 💖 by kimbius
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: true,
      drawer: false,
      items: [
        {
          icon: "house-fill",
          title: "หน้าหลัก",
          to: "/",
        },
        {
          icon: "patch-question-fill",
          title: "การใช้งานระบบ",
          to: "/guide",
        },
      ],
      miniVariant: false,
      title: "โรงเรียนบึงกาฬ",
    };
  },
};
</script>

<style>
* {
  font-family: "Kanit", sans-serif;
}
.v-navigation-drawer__border {
  opacity: 0;
}
</style>
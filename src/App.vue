<template>
  <v-app>
    <system-bar
      :theme="currentTheme"
      :platform="platform"
      :on-close="close"
      :on-maximize="toggleMaximize"
      :on-minimize="minimize"
      :is-maximizable="isMaximizable"
      :is-closable="isClosable"
      :is-minimizable="isMinimizable"
      :show-icon="showIcon"
      :show-title="showTitle"
      :menu="menu"
    >
      <template slot="icon">
        <img
          src="@/assets/logo_white.svg"
          alt="logo_white"
          v-if="$vuetify.theme.dark"
        />
        <img src="@/assets/logo.svg" alt="logo" v-else />
      </template>

      <template slot="title">
        {{ $t("app-name") }}
      </template>
    </system-bar>
    <preloader />
    <notifier />
    <v-main>
      <transition name="fade" mode="out-in">
        <router-view />
      </transition>
    </v-main>
  </v-app>
</template>

<script>
import { remote } from "electron";
import Notifier from "./components/partials/Notifier";
import SystemBar from "./components/partials/SystemBar";
import Preloader from "@/components/partials/Preloader";
export default {
  name: "App",
  components: { Preloader, SystemBar, Notifier },
  data: function() {
    return {
      platform: process.platform,
      isMaximizable: remote.getCurrentWindow().isMaximizable(),
      isMinimizable: remote.getCurrentWindow().isMinimizable(),
      isClosable: remote.getCurrentWindow().isClosable(),
      showTitle: true,
      showIcon: true,
      menu: []
    };
  },
  created() {
    this.$vuetify.theme.dark = true;
  },
  computed: {
    currentTheme() {
      if (this.$vuetify.theme.dark) {
        return "dark";
      } else {
        return "light";
      }
    }
  },
  methods: {
    close() {
      remote.getCurrentWindow().close();
    },
    toggleMaximize() {
      let win = remote.getCurrentWindow();
      if (win.isMaximized()) win.unmaximize();
      else win.maximize();
    },
    minimize() {
      remote.getCurrentWindow().minimize();
    }
  }
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

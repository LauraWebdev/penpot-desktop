<script>
import '@mdi/font/css/materialdesignicons.min.css';
import Tabs from "./components/Tabs.vue";
import PageView from "./components/PageView.vue";
import PageEmpty from "./components/PageEmpty.vue";
import PageSettings from "./components/PageSettings.vue";

export default {
  name: 'App',
  components: {
    Tabs,
    PageView,
    PageEmpty,
    PageSettings,
  },
  data() {
    return {
      currentTabType: 'empty',
      isSettingsOpen: false,
      settingsServer: window.localStorage.getItem('settingsServer') ?? 'https://design.penpot.app',
      settingsTheme: window.localStorage.getItem('settingsTheme') ?? 'light',
    };
  },
  mounted() {
    this.emitter.on('tab-changed', (newTab) => {
      this.currentTabType = newTab.type;
    });

    this.emitter.on('open-settings', () => {
      this.isSettingsOpen = true;
    });

    this.emitter.on('close-settings', () => {
      this.isSettingsOpen = false;
    });

    this.emitter.on('save-settings', (settings) => {
      this.settingsServer = settings.server;
      this.settingsTheme = settings.theme;

      window.localStorage.setItem('settingsServer', settings.server);
      window.localStorage.setItem('settingsTheme', settings.theme);
    });
  },
};
</script>

<template>
  <div
      :class="['app', 'theme-' + settingsTheme]"
  >
    <Tabs
        v-show="!isSettingsOpen"
        :settings-server="settingsServer"
        :settings-theme="settingsTheme"
    />

    <main>
      <PageView
          v-show="currentTabType === 'page' && !isSettingsOpen"
      />
      <PageEmpty
          v-show="currentTabType === 'empty' && !isSettingsOpen"
      />
      <PageSettings
          v-show="isSettingsOpen"
      />
    </main>
  </div>
</template>

<style>
:root {
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  font-size: 14px;
  line-height: 1em;
  font-weight: 400;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-text-size-adjust: 100%;
}

html, body {
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.app {
  user-select: none;
  display: grid;
  grid-template-rows: auto 1fr;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  transition: 0.2s ease-in-out background, 0.2s ease-in-out color;
}
.app.theme-light {
  background: #fff;
  color: #333;
}
.app.theme-dark {
  background: #333;
  color: #fff;
}
main {
  display: grid;
}
</style>

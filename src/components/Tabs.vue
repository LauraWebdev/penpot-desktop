<template setup>
  <Vue3TabsChrome
      :class="['theme-' + settingsTheme]"
      ref="tabsRef"
      :tabs="tabs"
      v-model="tab"
      @click="onTabClick"
      @remove="onTabClose"
  >
    <template v-slot:after>
      <div
          class="tab-button"
          tabindex="-1"
          @click="addTab"
      >
        <span class="mdi mdi-plus"></span>
      </div>
      <div
          class="tab-button"
          tabindex="-1"
          @click="goToSettings"
      >
        <span class="mdi mdi-cog"></span>
      </div>
    </template>
  </Vue3TabsChrome>
</template>

<script>
import Vue3TabsChrome from 'vue3-tabs-chrome';
import 'vue3-tabs-chrome/dist/vue3-tabs-chrome.css';

export default {
  name: 'Tabs',
  components: {
    Vue3TabsChrome
  },
  props: [
      'settingsServer',
      'settingsTheme'
  ],
  data() {
    return {
      tab: null,
      tabs: [],
    };
  },
  mounted() {
    this.emitter.on('page-update-meta', (meta) => {
      let currentTab = this.tabs.find(x => x.key === this.tab);

      if(currentTab && meta.title) currentTab.label = meta.title;
      if(currentTab && meta.url) currentTab.url = meta.url;
      if(currentTab && meta.favicon) currentTab.favicon = meta.favicon;
    });
  },
  methods: {
    addTab() {
      const key = 'tab' + Date.now();

      this.$refs.tabsRef.addTab({
        key,
        label: 'Loading...',
        url: this.settingsServer,
        type: 'page',
      });

      const newTab = this.tabs.find(x => x.key === key);
      this.tab = newTab.key;
      this.switchTab(newTab);
    },
    onTabClick(event, tab) {
      this.switchTab(tab);
    },
    onTabClose(tab, index) {
      console.log("Closed " + index + " of " + this.tabs.length);

      if(index === 0 && this.tabs.length === 0) {
        this.switchTab({
          type: 'empty'
        });
      } else if(index < this.tabs.length) {
        const newTab = this.tabs[index];
        this.switchTab(newTab);
      } else {
        const newTab = this.tabs[index - 1];
        this.switchTab(newTab);
      }
    },
    switchTab(newTab) {
      this.emitter.emit('tab-changed', newTab);
    },
    goToSettings() {
      this.emitter.emit('open-settings');
    }
  }
}
</script>

<style>
.tab-button {
  margin: 0 5px;
  width: 28px;
  height: 28px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
  border-radius: 100px;
  transition: 0.2s ease-in-out all;
  cursor: pointer;
}

/* LIGHT */
.theme-light .tab-button:hover {
  background: rgba(0,0,0,0.14);
}
.theme-light .tab-button:active {
  background: rgba(0,0,0,0.21);
}

/* DARK */
.theme-dark .tab-button:hover {
  background: rgba(255,255,255,0.14);
}
.theme-dark .tab-button:active {
  background: rgba(255,255,255,0.21);
}
</style>
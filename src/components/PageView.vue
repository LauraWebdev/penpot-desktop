<template>
  <webview
    src="about://blank"
    ref="webview"
    @load-commit="updatePageUrl"
    @page-title-updated="updatePageTitle"
    @page-favicon-updated="updatePageFavicon"
  />
</template>

<script>
export default {
  name: 'PageView',
  mounted() {
    this.emitter.on('tab-changed', (newTab) => {
      this.loadTab(newTab);
    });
  },
  methods: {
    loadTab(tab) {
      if(tab.type === 'page')
      this.$refs.webview.loadURL(tab.url);
    },
    updatePageUrl(event) {
      this.emitter.emit('page-update-meta', {
        url: event.url
      });
    },
    updatePageTitle(event) {
      this.emitter.emit('page-update-meta', {
        title: event.title
      });
    },
    updatePageFavicon(event) {
      this.emitter.emit('page-update-meta', {
        favicon: event.favicons[0]
      });
    }
  }
}
</script>
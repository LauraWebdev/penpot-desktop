<template>
  <div class="page-settings">
    <h1>Penpot Desktop Settings</h1>

    <div class="form">
      <label class="form-item">
        <div class="label">Application Version</div>
        <div class="text">Version {{ version }}</div>
      </label>
      <label class="form-item">
        <div class="label">Penpot Server</div>
        <input
            type="url"
            v-model="server"
            placeholder="https://design.penpot.app"
        />
      </label>
      <label class="form-item">
        <div class="label">Theme</div>
        <select v-model="theme">
          <option value="light">Light</option>
          <option value="dark">Dark</option>
        </select>
      </label>
    </div>

    <div class="actions">
      <div class="button button-primary" @click="handleSave">Save changes</div>
      <div class="button button-secondary" @click="handleCancel">Cancel</div>
    </div>

    <footer>
      Penpot Desktop by Laura Sofia Heimann (@LauraWebdev), Penpot by Kaleidos & contributors (@penpotapp), Icons by materialdesignicons.com, Developed using Nodejs, Electron, Vite, Vue and of course Penpot
    </footer>
  </div>
</template>

<script>
export default {
  name: 'PageSettings',
  data() {
    return {
      version: __APP_VERSION__,
      server: 'https://design.penpot.app',
      theme: 'light',
    }
  },
  mounted() {
    this.server = window.localStorage.getItem('settingsServer') ?? 'https://design.penpot.app';
    this.theme = window.localStorage.getItem('settingsTheme') ?? 'light';
  },
  methods: {
    handleSave() {
      this.emitter.emit('save-settings', {
        server: this.server,
        theme: this.theme,
      });
    },
    handleCancel() {
      this.emitter.emit('close-settings');
    }
  }
}
</script>

<style>
.page-settings {
  width: 90%;
  max-width: 650px;
  margin: 0 auto;
  padding: 50px 0;
}
.page-settings h1 {
  font-size: 18px;
  margin: 0 0 50px;
}
.page-settings .form {
  display: grid;
  grid-gap: 25px;
  margin-bottom: 50px;
}
.page-settings .form .form-item {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-gap: 15px;
  align-items: center;
}
.page-settings .form .form-item .label {
  font-weight: bold;
  justify-self: flex-end;
  line-height: 1em;
}
.page-settings .form .form-item .text {
  line-height: 1.5em;
  transition: 0.2s ease-in-out all;
}
.page-settings .form .form-item input {
  padding: 10px;
  outline: none;
  border: 0;
  border-radius: 5px;
  font-size: 14px;
  transition: 0.2s ease-in-out all;
}
.page-settings .form .form-item select {
  padding: 10px;
  outline: none;
  border: 0;
  border-radius: 5px;
  font-size: 14px;
  transition: 0.2s ease-in-out all;
}
.page-settings .form .form-item select option {
  color: #333;
}
.page-settings .actions {
  display: flex;
  gap: 15px;
  align-items: center;
  justify-content: flex-end;
  margin-bottom: 50px;
}
.page-settings .actions .button {
  font-size: 12px;
  font-weight: bold;
  text-transform: uppercase;
  line-height: 1em;
  padding: 10px;
  border-radius: 4px;
  transition: 0.2s ease-in-out background, 0.2s ease-in-out color;
  cursor: pointer;
}
.page-settings .actions .button.button-primary {
  background: #31efb8;
  color: #000;
}
.page-settings .actions .button.button-secondary {
  background: transparent;
}
.page-settings .actions .button:hover {
  background: #000;
  color: #31efb8;
}
.page-settings .actions .button:hover {
  background: #000;
  color: #31efb8 !important;
}
footer {
  opacity: 0.6;
  font-size: 12px;
  text-align: center;
}

/* LIGHT */
.theme-light .page-settings .actions .button.button-secondary {
  color: #333;
}
.theme-light .page-settings .form .form-item .text {
  color: rgba(0,0,0,0.6);
}
.theme-light .page-settings .form .form-item input {
  background: rgba(0,0,0,0.14);
  color: #333;
}
.theme-light .page-settings .form .form-item select {
  background: rgba(0,0,0,0.14);
  color: #333;
}
.theme-light .page-settings .form .form-item input::placeholder {
  color: rgba(0,0,0,0.3);
}

/* DARK */
.theme-dark .page-settings .actions .button.button-secondary {
  color: #31efb8;
}
.theme-dark .page-settings .form .form-item .text {
  color: rgba(255,255,255,0.4);
}
.theme-dark .page-settings .form .form-item input::placeholder {
  color: rgba(255,255,255,0.35);
}
.theme-dark .page-settings .form .form-item input {
  background: rgba(255,255,255,0.14);
  color: #fff;
}
.theme-dark .page-settings .form .form-item select {
  background: rgba(255,255,255,0.14);
  color: #fff;
}
</style>
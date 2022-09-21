<template>
  <div class="page-empty">
    <span class="mdi mdi-hand-wave"></span>
    <h1>Penpot Desktop</h1>
    <p>Click on the <span class="mdi mdi-plus"></span> in the tab bar to get started.</p>
  </div>
</template>

<style>
.page-empty {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.page-empty > .mdi {
  font-size: 48px;
  margin-bottom: 25px;
  animation: wave 0.5s infinite alternate;
  transform-origin: 20px 20px;
}
.page-empty h1 {
  font-size: 18px;
  margin: 15px 0;
}
.page-empty p {
  margin: 0;
}

/* LIGHT */
.theme-light .page-empty p {
  color: rgba(0,0,0,0.6);
}

/* DARK */
.theme-dark .page-empty p {
  color: rgba(255,255,255,0.4);
}

@keyframes wave {
  0% {
    transform: rotate(-20deg);
  }
  90% {
    transform: rotate(20deg);
  }
  100% {
    transform: rotate(20deg);
  }
}
</style>
<template>
  <div>
    <nav-bar-component />
    <banner-component v-if="$route.path === '/' || $route.path === '/menu'" />
    <router-view />
  </div>
</template>

<script>
import NavBarComponent from './components/NavBarComponent.vue';
import BannerComponent from './components/BannerComponent.vue';

export default {
    name: "App",
    components: {
      NavBarComponent,
      BannerComponent
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap');

:root {
  --bg-ivory: #f7f0e4;
  --bg-paper: #fffaf2;
  --panel: #fffdf9;
  --panel-strong: #f1e5cf;
  --red: #5b1f1c;
  --red-soft: #7d2f27;
  --brown: #2b1c16;
  --gold: #c89b3c;
  --olive: #586d3b;
  --olive-deep: #40512d;
  --shadow-soft: 0 12px 28px rgba(43, 28, 22, 0.08);
  --shadow-strong: 0 18px 36px rgba(43, 28, 22, 0.12);
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  background: linear-gradient(180deg, #f8f2e8 0%, #f1e7d7 100%);
  color: var(--brown);
  font-family: 'Inter', 'Segoe UI', sans-serif;
}

* {
  box-sizing: border-box;
}

img {
  display: block;
  max-width: 100%;
}

#app {
  min-height: 100vh;
  font-family: 'Inter', 'Segoe UI', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--brown);
  background:
    radial-gradient(circle at top, rgba(200, 155, 60, 0.12), transparent 32%),
    linear-gradient(180deg, var(--bg-ivory) 0%, #f1e7d7 100%);
}

nav a {
  font-weight: 600;
  color: var(--brown);
}

nav a.router-link-exact-active {
  color: var(--gold);
}
</style>
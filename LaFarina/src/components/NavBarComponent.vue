<template>
  <div class="nav-shell">
    <nav>
      <router-link to="/" id="logo-url" aria-label="La Farina home">
        <img :src="'/img/logo_lafarina.png'" id="logo" alt="La Farina Logo" />
      </router-link>

      <div class="nav-links">
        <router-link to="/menu">Menu</router-link>
        <span class="divider">|</span>
        <router-link to="/pedidos">Pedidos</router-link>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: 'NavBarComponent',
};
</script>

<style scoped>
.nav-shell {
  position: sticky;
  top: 0;
  z-index: 20;
  background: rgba(39, 23, 18, 0.94);
  backdrop-filter: blur(4px);
  border-bottom: 2px solid rgba(200, 155, 60, 0.8);
  box-shadow: 0 8px 20px rgba(39, 23, 18, 0.12);
}

nav {
  max-width: 1180px;
  margin: 0 auto;
  padding: 18px 28px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

#logo-url {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-right: auto;
  transition: transform 0.35s ease;
}

#logo-url:hover {
  transform: scale(1.04) rotate(-2deg);
}

#logo {
  width: 64px;
  height: 64px;
  object-fit: cover;
  border-radius: 50%;
  border: 2px solid rgba(200, 155, 60, 0.8);
  box-shadow: 0 8px 18px rgba(0, 0, 0, 0.18);
  background: #fff;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 0.96rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

nav a {
  color: #f4e8d4;
  text-decoration: none;
  transition: all 0.2s ease;
  position: relative;
}

nav a:hover,
nav a.router-link-exact-active {
  color: #f4d98d;
}

nav a::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -5px;
  width: 100%;
  height: 2px;
  background: var(--gold);
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.2s ease;
}

nav a:hover::after,
nav a.router-link-exact-active::after {
  transform: scaleX(1);
}

.divider {
  color: rgba(244, 232, 212, 0.8);
  font-weight: 700;
}
</style>
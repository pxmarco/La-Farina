<template>
    <div id="main-banner" :style="{ backgroundImage: 'url(' + bannerUrl + ')' }">
        <h1>La Tradizione Italiana</h1>
    </div>
</template>

<script>
export default {
    name: "BannerComponent",
    data() {
        return {
            bannerUrl: "/img/banner_pizza.jpg"
        }
    }
};
</script>

<style scoped>
#main-banner {
  position: relative;
  height: 420px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  background-position: center;
  background-size: cover;
  border-bottom: 4px solid rgba(200, 155, 60, 0.85);
  box-shadow: inset 0 0 0 2000px rgba(21, 16, 14, 0.25);
}

#main-banner::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, rgba(35, 17, 12, 0.12), rgba(35, 17, 12, 0.48));
}

#main-banner h1 {
  position: relative;
  z-index: 1;
  margin: 0 0 0 auto;
  padding: 26px 40px 26px 52px;
  font-family: 'Cormorant Garamond', 'Georgia', serif;
  font-size: clamp(2.4rem, 4vw, 4rem);
  font-weight: 700;
  line-height: 0.95;
  color: #fdf5e8;
  text-align: left;
  background: rgba(91, 31, 28, 0.82);
  border-left: 5px solid #d7af50;
  border-radius: 80px 0 0 80px;
  box-shadow: -12px 14px 30px rgba(0, 0, 0, 0.2);
  letter-spacing: 0.06em;
  text-transform: uppercase;
}
</style>
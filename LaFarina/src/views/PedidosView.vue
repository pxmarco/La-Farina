<template>
  <div class="pedidos-view">
    <div class="pedidos-header">
      <p class="pedidos-kicker">Sala de controle</p>
      <h1>Pedidos</h1>
    </div>
    <ListaPedidoComponent/>
  </div>
</template>
<script>
import ListaPedidoComponent from '@/components/ListaPedidoComponent.vue';

export default {
  name: "PedidosView",
  components: {
    ListaPedidoComponent,
  },
};
</script>
<style scoped>
.pedidos-view {
  padding: 36px 0 60px;
}

.pedidos-header {
  max-width: 1200px;
  margin: 0 auto 16px;
  padding: 0 16px;
  text-align: center;
}

.pedidos-kicker {
  margin: 0 0 8px;
  font-size: 0.76rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #8c5c24;
  font-weight: 700;
}

.pedidos-header h1 {
  margin: 0;
  font-family: 'Cormorant Garamond', 'Georgia', serif;
  font-size: clamp(2.5rem, 4vw, 4rem);
  color: var(--red);
  line-height: 1;
}
</style>
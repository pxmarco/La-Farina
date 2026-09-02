<template>
  <div>
    <mensagem-component v-if="msg" :msg="msg" :tipo="alertType" />

    <div id="pedidos-tabela">
      <div>
        <div id="pedidos-tabela-cabecalho">
          <div id="ordem-id">#ID</div>
          <div>Nome</div>
          <div>Pizza</div>
          <div>Tamanho</div>
          <div>Opcionais</div>
          <div>Status</div>
          <div id="div-acoes">Ações</div>
        </div>
      </div>
    </div>

    <div
      class="pedidos-tabela-linha"
      v-for="pedido in listaPedidosRealizados"
      :key="pedido.id"
    >
      <div id="ordem-numero">{{ pedido.id }}</div>
      <div>{{ pedido.nome }}</div>
      <div>{{ pedido.pizza && pedido.pizza.nome ? pedido.pizza.nome : '--' }}</div>
      <div>{{ pedido.tamanho && pedido.tamanho.descricao ? pedido.tamanho.descricao : '--' }}</div>
      <div>
        <ul>
          <li v-for="(complemento, index) in pedido.complemento" :key="index">
            {{ complemento.nome }}
          </li>
        </ul>
        <div class="divider"></div>
        <ul>
          <li v-for="(refri, index) in pedido.bebidas" :key="index">
            {{ refri.nome }}
          </li>
        </ul>
      </div>
      <div>
        <select
          @change="atualizarStatusPedido($event, pedido.id)"
          name="status"
          class="status"
        >
          <option value="">Selecione</option>
          <option
            v-for="status in listaStatusPedido"
            :key="status.id"
            :value="status.id"
            :selected="status.id == pedido.statusId"
          >
            {{ status.descricao }}
          </option>
        </select>
      </div>
      <div id="div-acoes">
        <img 
          src="/img/icone_lixeira.png" 
          width="35px" 
          height="35px"
          @click="deletarPedido($event, pedido.id)"
          style="cursor: pointer;"
        />
      </div>
    </div>
  </div>
</template>

<script>
import MensagemComponent from '@/components/MensagemComponent.vue';

export default {
  name: "ListaPedidoComponent",
  components: {
    MensagemComponent
  },
  data() {
    return {
      listaPedidosRealizados: [],
      listaStatusPedido: [],
      msg: null,
      alertType: "aviso" 
    };
  },
  methods: {
    async consultarPedidos() {
      const response = await fetch("https://api-mammamia.onrender.com/pedidos");
      this.listaPedidosRealizados = await response.json();
    },
    async consultarStatusPedido() {
      const response = await fetch("https://api-mammamia.onrender.com/status_pedido");
      this.listaStatusPedido = await response.json();
    },
    async atualizarStatusPedido(event, idPedido) {
      const idPedidoAtualizado = event.target.value;
      const atualizacaoJson = JSON.stringify({ statusId: Number(idPedidoAtualizado) });
      
      await fetch(`https://api-mammamia.onrender.com/pedidos/${idPedido}`, {
        method: "PATCH",
        headers: { "Content-type": "application/json" },
        body: atualizacaoJson,
      });
    },
    async deletarPedido(event, idPedido) {
      const req = await fetch(`https://api-mammamia.onrender.com/pedidos/${idPedido}`, {
        method: "DELETE",
      });

      if (req.ok) {
        this.listaPedidosRealizados = this.listaPedidosRealizados.filter(
          (pedido) => pedido.id !== idPedido
        );

        this.msg = `Aviso: O pedido #${idPedido} foi removido com sucesso.`;
        this.alertType = "aviso";

        setTimeout(() => {
          this.msg = null;
        }, 3000);
      }
    },
  },
  mounted() {
    this.consultarPedidos();
    this.consultarStatusPedido();
  },
};
</script>

<style scoped>
#pedidos-tabela {
  width: min(1200px, calc(100% - 32px));
  margin: 18px auto 0;
  border-radius: 22px;
  overflow: hidden;
  box-shadow: var(--shadow-soft);
  background: rgba(255, 253, 249, 0.92);
  border: 1px solid rgba(94, 68, 47, 0.12);
}

#pedidos-tabela-cabecalho,
.pedidos-tabela-linha {
  display: grid;
  grid-template-columns: 0.6fr 1.2fr 1.6fr 1.1fr 1.8fr 1.3fr 0.7fr;
  align-items: center;
  gap: 12px;
}

#pedidos-tabela-cabecalho {
  padding: 18px 16px;
  background: linear-gradient(180deg, #5b1f1c 0%, #3d1815 100%);
  color: #f9e7bf;
  font-size: 0.76rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.pedidos-tabela-linha {
  padding: 16px;
  border-bottom: 1px solid rgba(87, 63, 46, 0.14);
  background: rgba(255, 255, 255, 0.2);
  color: var(--brown);
}

.pedidos-tabela-linha:nth-of-type(even) {
  background: rgba(244, 232, 209, 0.2);
}

#pedidos-tabela-cabecalho #ordem-id,
.pedidos-tabela-linha #ordem-numero,
.pedidos-tabela-linha #div-acoes,
#pedidos-tabela-cabecalho #div-acoes {
  text-align: center;
}

.pedidos-tabela-linha .status {
  width: 100%;
  min-height: 40px;
  border: 1px solid rgba(43, 28, 22, 0.18);
  border-radius: 10px;
  background: #fff;
  padding: 8px 10px;
  color: var(--brown);
}

ul {
  padding-left: 18px;
  margin: 4px 0;
  list-style: disc;
}

li {
  font-size: 0.85rem;
  line-height: 1.5;
}

.divider {
  border-top: 1px solid rgba(87, 63, 46, 0.2);
  margin: 6px 0;
  width: 85%;
}

#div-acoes img {
  width: 32px;
  height: 32px;
  opacity: 0.9;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

#div-acoes img:hover {
  transform: scale(1.08);
  opacity: 1;
}
</style>
<template>
  <div>
    <mensagem-component v-if="msg" :msg="msg" :tipo="alertType" />

    <form id="pedido-form" @submit="criarPedido($event)">
      <div class="pizza-visual">
        <p id="nome-hamburguer-content">
          {{ pizza && pizza.nome ? pizza.nome : "--" }}
        </p>
        <img
          id="foto-content"
          :src="pizza && pizza.foto ? pizza.foto : ''"
        />
      </div>

      <div class="form-panel">
        <div class="inputs">
          <label>Nome</label>
          <input
            v-model="nomeCliente"
            type="text"
            placeholder="Digite seu Nome"
            id="nome-cliente"
          />
        </div>

        <div class="inputs">
          <label>Tamanho da Pizza</label>
          <select
            v-model="tamanhoSelecionado"
            name="tamanho-pizza"
            id="tamanho-pizza"
          >
            <option value="" selected>Selecione o tamanho</option>
            <option
              v-for="tamanho in listaTamanhos"
              :key="tamanho.id"
              :value="tamanho"
            >
              {{ tamanho.descricao }}
            </option>
          </select>
        </div>

        <div class="inputs">
          <label id="opcionais-titulo">Selecione os opcionais</label>
          <label id="opcionais-subtitulo">Bordas recheadas e adicionais</label>

          <div
            v-for="complemento in listaComplementos"
            :key="complemento.id"
            class="checkbox-container"
          >
            <input
              type="checkbox"
              :name="complemento.nome"
              :value="complemento"
              v-model="listaComplementosSelecionados"
            />
            <span>{{ complemento.nome }}</span>
          </div>

          <label>Adicione uma bebida</label>

          <div
            v-for="bebida in listaBebidas"
            :key="bebida.id"
            class="checkbox-container"
          >
            <input
              type="checkbox"
              :name="bebida.nome"
              :value="bebida"
              v-model="listaBebidasSelecionadas"
            />
            <span>{{ bebida.nome }}</span>
          </div>
        </div>

        <div class="inputs submit-wrap">
          <input type="submit" class="submit-btn" value="Confirmar Pedido" />
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import MensagemComponent from '@/components/MensagemComponent.vue';

export default {
  name: "PedidoComponent",
  components: {
    MensagemComponent
  },
  props: {
    pizza: null,
  },
  data() {
    return {
      listaTamanhos: [],
      listaComplementos: [],
      listaBebidas: [],
      nomeCliente: "",
      tamanhoSelecionado: "",
      listaComplementosSelecionados: [],
      listaBebidasSelecionadas: [],
      msg: null,
      alertType: "sucesso"
    };
  },
  methods: {
    async getTiposTamanhos() {
      const response = await fetch("https://api-mammamia.onrender.com/tipos_tamanhos");
      const dados = await response.json();
      this.listaTamanhos = dados;
    },
    async getOpcionais() {
      const response = await fetch("https://api-mammamia.onrender.com/opcionais");
      const dados = await response.json();
      this.listaComplementos = dados.complemento;
      this.listaBebidas = dados.bebidas;
    },
    async criarPedido(e) {
      e.preventDefault();

      if (!this.nomeCliente || !this.tamanhoSelecionado) {
        this.msg = "Erro de Preenchimento: Os campos 'Nome' e 'Tamanho da Pizza' precisam estar preenchidos!";
        this.alertType = "erro"; 
        return;
      }

      const dadosPedido = {
        nome: this.nomeCliente,
        tamanho: this.tamanhoSelecionado,
        bebidas: Array.from(this.listaBebidasSelecionadas),
        complemento: Array.from(this.listaComplementosSelecionados),
        pizza: this.pizza,
        statusId: 6,
      };

      const dadosJson = JSON.stringify(dadosPedido);

      try {
        const req = await fetch("https://api-mammamia.onrender.com/pedidos", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: dadosJson,
        });

        if (req.ok) {
          this.msg = `Molto bene! Pedido realizado com sucesso para ${dadosPedido.nome}!`;
          this.alertType = "sucesso";

          this.nomeCliente = "";
          this.tamanhoSelecionado = "";
          this.listaComplementosSelecionados = [];
          this.listaBebidasSelecionadas = [];

          setTimeout(() => {
            this.msg = null;
            this.$router.push("/pedidos");
          }, 2000);
        } else {
          this.msg = "Ocorreu um erro ao processar o envio.";
          this.alertType = "erro";
        }
      } catch (error) {
        this.msg = "Erro de conexão com o banco de dados.";
        this.alertType = "erro";
      }
    },
  },
  mounted() {
    this.getTiposTamanhos();
    this.getOpcionais();
  },
};
</script>

<style scoped>
#pedido-form {
  max-width: 1100px;
  margin: 28px auto 60px;
  padding: 0 20px;
  display: grid;
  grid-template-columns: minmax(280px, 1fr) minmax(320px, 1.15fr);
  gap: 28px;
  align-items: start;
}

.pizza-visual,
.form-panel {
  background: rgba(255, 253, 249, 0.92);
  border: 1px solid rgba(94, 68, 47, 0.14);
  border-radius: 24px;
  box-shadow: var(--shadow-soft);
}

.pizza-visual {
  padding: 16px 16px 12px;
}

#foto-content {
  position: relative;
  z-index: 0;
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 18px;
  border: 2px solid rgba(200, 155, 60, 0.5);
  box-shadow: 0 18px 30px rgba(43, 28, 22, 0.12);
  margin-bottom: 12px;
}

#nome-hamburguer-content {
  position: relative;
  z-index: 1;
  display: inline-block;
  margin: -12px 10px 12px;
  font-family: 'Cormorant Garamond', 'Georgia', serif;
  font-size: clamp(2.2rem, 4vw, 3.5rem);
  line-height: 0.9;
  color: #fffaf2;
  background: rgba(91, 31, 28, 0.8);
  border-left: 5px solid #d7af50;
  padding: 12px 18px;
  border-radius: 16px 16px 16px 0;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
}

.form-panel {
  padding: 28px;
}

.inputs {
  display: flex;
  flex-direction: column;
  margin-bottom: 18px;
}

label {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  font-weight: 700;
  color: var(--brown);
  padding: 6px 12px;
  border-left: 4px solid var(--gold);
  background: rgba(244, 232, 209, 0.38);
  border-radius: 0 10px 10px 0;
}

input,
select {
  width: 100%;
  min-height: 46px;
  padding: 12px 14px;
  border: 1px solid rgba(43, 28, 22, 0.18);
  border-radius: 12px;
  background: #fff;
  color: var(--brown);
  font-size: 0.95rem;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.03);
}

input:focus,
select:focus {
  outline: 2px solid rgba(200, 155, 60, 0.25);
  border-color: rgba(200, 155, 60, 0.7);
}

#opcionais-titulo,
#opcionais-subtitulo {
  width: 100%;
  background: transparent;
  border: none;
  padding-left: 0;
  margin-bottom: 10px;
}

#opcionais-subtitulo {
  font-size: 0.76rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #826134;
  font-weight: 600;
}

.checkbox-container {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 10px;
  padding: 8px 0;
}

.checkbox-container input {
  width: 18px;
  min-height: 18px;
  accent-color: var(--olive);
}

.checkbox-container span {
  font-weight: 600;
  color: rgba(43, 28, 22, 0.85);
}

.submit-wrap {
  margin-top: 14px;
}

.submit-btn {
  background: linear-gradient(180deg, #5f7739 0%, #40512d 100%);
  color: #fffdf9;
  font-weight: 700;
  border: none;
  border-radius: 14px;
  padding: 16px;
  font-size: 1rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.submit-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 12px 24px rgba(64, 81, 45, 0.2);
}
</style>
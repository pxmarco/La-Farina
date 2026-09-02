<template>
  <div class="menu-container">
    <div class="menu-header">
      <p class="menu-kicker">Trattoria italiana</p>
      <h1 class="menu-titulo">Menu La Farina</h1>
      <p class="menu-subtitulo">Tradizione e Passione in Ogni Fetta</p>
    </div>

    <div id="scroll-horizontal">
      <div id="card-content" v-for="pizza in listaMenuPizzas" :key="pizza.id">
        <div class="card-inner">
          <div class="foto-pizza">
            <img
              width="300"
              height="200"
              :src="pizza.foto"
              :alt="pizza.nome"
            />
          </div>
          <div class="card-coluna">
            <p id="nome-content">{{ pizza.nome }}</p>
            <p id="preco-content">R$ {{ pizza.valor }},00</p>
            <p id="descricao-content">{{ pizza.descricao }}</p>
            <button class="btn-selecionar" @click="selecionarPizza(pizza)">
              Selecionar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MenuView",
  data(){
    return{
      listaMenuPizzas: [],
    };
  },
  methods: {
    async consultarMenu() {
      try {
        const response = await fetch("https://api-mammamia.onrender.com/menu"); 
        const dados = await response.json();
        if (dados && dados.pizzas) {
          this.listaMenuPizzas = dados.pizzas;
        } else {
          this.listaMenuPizzas = dados;
        }
        console.log("Dados carregados com sucesso!", this.listaMenuPizzas);
      } catch (error) {
        console.error("Erro ao buscar o menu", error);
      }
    },
    selecionarPizza(pizzaSelecionada) {
      const param = JSON.stringify(pizzaSelecionada);
      const pizzaJson = encodeURIComponent(param);
      this.$router.push({path: "/config-pedido", query: {pizza: pizzaJson}})
    }
  },
  mounted() {
    this.consultarMenu();
  }
};
</script>

<style scoped>
.menu-container {
  background:
    radial-gradient(circle at top, rgba(200, 155, 60, 0.08), transparent 32%),
    linear-gradient(180deg, rgba(255, 250, 242, 0.98), rgba(244, 232, 209, 0.8));
  padding: 44px 0 64px;
  min-height: 100vh;
}

.menu-header {
  max-width: 1100px;
  margin: 0 auto 18px;
  text-align: center;
}

.menu-kicker {
  margin: 0 0 10px;
  font-size: 0.78rem;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #8c5c24;
  font-weight: 700;
}

.menu-titulo {
  margin: 0;
  font-family: 'Cormorant Garamond', 'Georgia', serif;
  font-size: clamp(2.5rem, 4vw, 4rem);
  line-height: 1;
  color: var(--red);
  font-weight: 700;
}

.menu-subtitulo {
  font-family: 'Inter', sans-serif;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.2em;
  color: #8f6730;
  margin: 10px 0 0;
}

#scroll-horizontal {
  display: flex;
  justify-content: flex-start;
  overflow-x: auto;
  gap: 18px;
  width: 95%;
  max-width: 1180px;
  margin: 0 auto;
  padding: 22px 10px 12px;
  scrollbar-width: thin;
  scrollbar-color: #c89b3c #f6f0e5;
}

#card-content {
  flex: 0 0 auto;
  display: inline-block;
  width: 300px;
  min-height: 520px;
  background: linear-gradient(180deg, rgba(255,255,255,0.92), rgba(249,240,227,0.9));
  border: 1px solid rgba(148, 122, 89, 0.22);
  border-radius: 18px;
  overflow: hidden;
  box-shadow: var(--shadow-soft);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

#card-content:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-strong);
}

.card-inner {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.foto-pizza {
  width: 100%;
  height: 215px;
  overflow: hidden;
  border-bottom: 3px solid rgba(200, 155, 60, 0.9);
}

.foto-pizza img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

#card-content:hover .foto-pizza img {
  transform: scale(1.06);
}

.card-coluna {
  padding: 20px 18px 18px;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  justify-content: space-between;
}

#nome-content {
  font-family: 'Cormorant Garamond', 'Georgia', serif;
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  color: var(--brown);
  margin: 0 0 10px 0;
  line-height: 1;
}

#preco-content {
  font-family: 'Inter', sans-serif;
  font-size: 1.6rem;
  font-weight: 700;
  text-align: center;
  color: var(--red);
  margin: 0 0 12px 0;
}

#descricao-content {
  font-size: 0.92rem;
  text-align: center;
  color: rgba(43, 28, 22, 0.75);
  margin: 0 0 18px 0;
  white-space: normal;
  text-overflow: ellipsis;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  line-height: 1.5;
}

.btn-selecionar {
  padding: 13px 16px;
  width: 100%;
  border-radius: 10px;
  border: 1px solid rgba(64, 81, 45, 0.8);
  color: #fffdf9;
  background: linear-gradient(180deg, #657f47 0%, #40512d 100%);
  font-family: 'Inter', sans-serif;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 0.76rem;
  letter-spacing: 0.12em;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
  margin-top: auto;
}

.btn-selecionar:hover {
  transform: translateY(-1px);
  box-shadow: 0 10px 18px rgba(64, 81, 45, 0.18);
  filter: brightness(1.04);
}
</style>
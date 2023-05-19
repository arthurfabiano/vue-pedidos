<template>
  <div>
      <Message :msg="msg" v-show="msg"/>
      <form id="pedido-form" @submit="createPedido">
        <div class="input-container">
          <label for="nome">Nome do Cliente:</label>
          <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite seu nome...">
        </div>
        <div class="input-container">
          <label for="pao">Escolha o Pão</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="">Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo}}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="carne">Escolha o Carne</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="">Selecione o tipo de carne</option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo}}</option>
          </select>
        </div>
        <div id="opcionais" class="input-container">
          <label id="opcionais-title" for="opcionais">Selecione os opcionais</label>
          <div v-for="opcional in opcionaisdata" :key="opcional.id" class="checkbox-container">
            <input type="checkbox" name="opcionais" v-model="opcionais" id="opcionais" :value="opcional.tipo">
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burguer">
        </div>
      </form>
  </div>
</template>

<script>
import Message from "@/components/Menssage.vue";

export default {
  name: "Form",
  components: {Message},
  data() {
    return {
      // vem do server
      paes: null,
      carnes: null,
      opcionaisdata: null,
      // dados que vao para o server
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes');
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createPedido(e) {
      e.preventDefault();

      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }
      const dataJson = JSON.stringify(data);

      const req = await  fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: dataJson
      });

      const res = await req.json();

      this.msg = `Pedido Número: #${res.id} para ${res.nome} realizado!`;
      setTimeout(() => this.msg = "", 5000);

      this.nome = "";
      this.carne = "";
      this.pao = "";

      this.opcionais = "";

    }
  },
  mounted() {
    this.getIngredientes();
  }
}
</script>

<style scoped>
  #pedido-form {
    max-width: 400px;
    margin: 0 auto;
  }
  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }
  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid darkorange;
  }
  input, select {
    padding: 5px 10px;
    width: 300px;
  }
  #opcionais {
    flex-direction: row;
    flex-wrap: wrap;
  }
  #opcionais-title {
    width: 100%;
  }
  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }
  .submit-btn {
    width: 100%;
    background-color: #222;
    color: darkorange;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
    background-color: transparent;
    color: #222222;
  }
</style>
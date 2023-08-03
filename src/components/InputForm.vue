<template>
  <div class="app-container">
    <h1 class="display-3 text-center my-5">Simulador de Investimentos Opções Equals9 </h1>

    <div class="row">
      <div class="col-md-6">
        <input v-model="valorInvestimento" type="number" class="input-box" placeholder="Valor do Investimento em R$">
      </div>

      <div class="col-md-6">
        <select v-model="tempoInvestimento" class="input-box">
          <option v-for="tempo in tempos" :value="tempo.value" :key="tempo.value">{{ tempo.text }}</option>
        </select>
      </div>
    </div>

    <button @click="simularInvestimento" class="btn-simular">Simular</button>

    <hr class="my-5">

    <div v-if="opcaoCall">
      <h3 class="text-center">Preço de exercício da EQ9:</h3>
      <p  class="text-center">R$ {{ strikePrice }}</p>
      <h3 class="text-center">Data de Exercício:</h3>
      <p  class="text-center">{{ dataExercicio }}</p>
      <h3 class="text-center">Opção de Venda - Valores em R$:</h3>
      <p class="text-center">R$ {{ opcaoPut }}</p>
    </div>

    <div v-if="opcaoPut !== null">
      <h3 class="text-center">Opção de Compra - Valores em EQ9:</h3>
      <p class="text-center">Tokens: {{ opcaoCall }} EQ9</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      valorInvestimento: "",
      tempoInvestimento: null,
      tempos: [
        { text: "6 meses", value: 6, opcaoCallDivider: 0.016, strikePrice: 0.03 },
        { text: "12 meses", value: 12, opcaoCallDivider: 0.022, strikePrice: 0.06 },
        { text: "18 meses", value: 18, opcaoCallDivider: 0.028, strikePrice: 0.10 },
        { text: "24 meses", value: 24, opcaoCallDivider: 0.051, strikePrice: 0.35 },
        { text: "36 meses", value: 36, opcaoCallDivider: 0.061, strikePrice: 0.60 },
        { text: "60 meses", value: 60, opcaoCallDivider: 0.064, strikePrice: 1.00 },
      ],
      opcaoCall: null,
      opcaoPut: null,
      strikePrice: null,
      dataExercicio: null,
      
    };
  },
  methods: {
    simularInvestimento() {
      if (!this.valorInvestimento || !this.tempoInvestimento) {
        alert("Por favor, insira o valor do investimento e escolha o tempo de investimento.");
        return;
      }

      const selectedTempo = this.tempos.find((tempo) => tempo.value === this.tempoInvestimento);
      if (!selectedTempo) {
        alert("Tempo de investimento inválido.");
        return;
      }

      const strikePrice = selectedTempo.strikePrice;
      const currentDate = new Date();
      const dataExercicio = new Date(currentDate);
      dataExercicio.setMonth(dataExercicio.getMonth() + selectedTempo.value);

      this.opcaoCall = (this.valorInvestimento / selectedTempo.opcaoCallDivider).toFixed(2);
      this.opcaoPut = (this.opcaoCall * strikePrice).toFixed(2);
      this.strikePrice = strikePrice.toFixed(2);
      this.dataExercicio = dataExercicio.toLocaleDateString("pt-BR").slice(0, 10); // Format as DD-MM-YYYY
    },
  },
};
</script>

<style>
.app-container {
  background-color: linear-gradient(70deg, #378805 5%, #c1c161 100%);;
  background-image: linear-gradient(70deg, #378805 5%, #c1c161 100%);;
  padding: 50px;
}

.input-box {
  color: #000;
  font-weight: bold;
  padding: 10px;
  width: 85%;
  box-sizing: border-box;
}

.btn-simular {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}
</style>
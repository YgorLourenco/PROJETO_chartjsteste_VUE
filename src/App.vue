<template>
  <div id="app" class="container">
    <div class="row mt-5" v-if="listaPositivo.length > 0">
      <div class="col">
        <h2>Positivo</h2>
        <line-chart :chartData="listaPositivo" :options="chartOptions" label="Positive"></line-chart>
      </div>
    </div>
  </div>
</template>

<script>
// Importando o axios para conectar com a API
import axios from 'axios';

// Biblioteca moment formata dados
import moment from 'moment';

// Importando o componente de grafico
import LineChart from './components/LineChart.vue';

export default {
  name: 'App',
  components: {
    // eslint-disable-next-line vue/no-unused-components
    LineChart
  },
  // Criando os arrays que vão armazenar as informações da api
  data(){
    return{
    listaPositvo: [],
    listaHospitalizado: [],
    listaEmUTI: [],
    listaVentiladores: [],
    listaRecuperados: [],
    listaMortes: [],
    chartOptions: {
      responsive: true,
      maintainAspectRatio: false
    }
    }
  },
  // Armazenando a requisição na variavel "data"
  async created() {
    const {data} = await axios.get("https://api.covidtracking.com/v1/us/daily.json")
    //console.log(data)

    // Formatando os dados de tipo data
    data.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD")

      // Armazenando cada tipo de dado da API
      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d;

      // Armazenando os objeto dos dados em arrays
      this.listaPositvo.push({date, total: positive})
      this.listaHospitalizado.push({date, total: hospitalizedCurrently})
      this.listaEmUTI.push({date, total: inIcuCurrently})
      this.listaVentiladores.push({date, total: onVentilatorCurrently})
      this.listaRecuperados.push({date, total: recovered})
      this.listaMortes.push({date, total: death})

      console.log(this.listaPositvo)
    });
  }
}
</script>

<style>

</style>

<template>
  <v-container>
    <v-row id="header">
      <v-col cols="12">
        <h1>EXCHANGE RATES AND CURRENCY API</h1>
      </v-col>
    </v-row>
    <v-row id="main">
      <v-col id="left">
        <v-select label="Selecciona la moneda" :items="currencies" item-value="text" item-title="value" v-model="currency"></v-select>
        <v-btn block class="primary" v-if="currency != null" @click="getHistoric">Mostrar historico</v-btn>
      </v-col>
      <v-col id="right">
        <v-card class="mx-auto text-center" color="green" max-width="600" dark v-if="info != null">
          <v-card-text>
            <v-sheet color="rgba(0, 0, 0, .12)">
              <v-sparkline :model-value="dataAnalist" color="rgba(255, 255, 255, .7)" height="100" padding="24" stroke-linecap="round" smooth>
                <template v-slot:label="item">
                  ${{ item.value }}
                </template>
              </v-sparkline>
            </v-sheet>
          </v-card-text>

          <v-card-text>
            <div class="text-h4 font-weight-thin">Price from 1999</div>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions class="justify-center">
            <v-btn variant="text" block>Go to Report</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      currencies: [],
      currency: null,
      loading: true,
      error: null,
      info: null,
      loading2: true,
      dataAnalist: null
    };
  },
  async created() {
    await this.getCurrencies();
  },
  methods: {
    async getCurrencies() {
      try {
        const response = await axios.get('https://api.frankfurter.app/currencies');
        // Convertir el objeto JSON en un array de pares clave-valor
        this.currencies = Object.entries(response.data).map(([key, value]) => ({ text: key, value }));
      } 
      catch (error) {
        this.error = 'Error al cargar las monedas.';
      } 
      finally {
        this.loading = false;
      }
    },
    async getHistoric() {
      try {
        const response = await axios.get(`https://api.frankfurter.app/1999-01-01..?to=${this.currency}`);
        const ratesData = response.data.rates; // Extraer las tasas de cambio
        const ratesArray = Object.values(ratesData); // Obtener los valores (tasas) como un array
        let newData = []
        this.info = ratesArray; 
        for(let i in this.info){
          newData.push(this.info[i][`${this.currency}`])
        }
        this.dataAnalist = newData;
      } 
      catch (error) {
        this.error = 'Error al cargar la información.';
      } 
      finally {
        this.loading2 = false;
      }
    }
  }
};
</script>

<style scoped>
#header{
  display: block;
  text-align: center;
  margin-bottom: 1rem;
}
#main{
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}
#left{
  padding: .5rem;
}
#right{
  padding: .5rem;
}
</style>

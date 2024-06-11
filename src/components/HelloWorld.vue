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
      </v-col>
      <v-col id="right">
        
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
      error: null
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
    async getHistoric(){
      try{

      }
      catch (error){

      }
      finally {
        
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

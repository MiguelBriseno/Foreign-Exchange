<template>
  <v-container>
    <v-row id="header">
      <v-col cols="12">
        <h1>EXCHANGE RATES AND CURRENCY API</h1>
      </v-col>
    </v-row>
    <v-row id="main">
      <v-col id="left">
        <v-select label="Selecciona la moneda"></v-select>
      </v-col>
      <v-col id="rigth">
        
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      currencies: null,
    };
  },
  async created() {
    await this.getCurrencies();
  },
  methods: {
    async getCurrencies() {
      try {
        const response = await axios.get('https://api.frankfurter.app/currencies');
        this.currencies = response.data;
      } 
      catch (error) {
        this.error = 'Error al cargar las monedas.';
      } 
      finally {
        this.loading = false;
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
#rigth{
  padding: .5rem;
}
</style>

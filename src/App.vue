<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col cols="10" md="12" lg="9" xl="6" class="bg--light mx-auto">
          <h1 class="text-center mb-4">Currency Converter</h1>
          <v-row justify="space-between" no-gutters>
            <v-col cols="12" md="5">
              <v-row no-gutters>
                <v-col class="px-1" cols="3">
                  <v-select @change="convert" v-model="selected[0]" :items="countries" label="I have"></v-select>
                </v-col>

                <v-col class="px-1" cols="9">
                  <v-text-field @input="convert" v-model="inputed" type="number"></v-text-field>
                </v-col>
              </v-row>
            </v-col>

            <v-col cols="2" offset="5" offset-md="0" class="d-flex justify-center align-center">
              <img class="exchange" :src="require('./assets/icons/exchange.png')" alt="Exchange">
            </v-col>

            <v-col cols="12" md="5">
              <v-row no-gutters>
                <v-col class="px-1" cols="9">
                  <v-text-field v-model="result" type="number" readonly></v-text-field>
                </v-col>

                <v-col class="px-1" cols="3">
                  <v-select @change="convert" v-model="selected[1]" :items="countries" label="I want to buy"></v-select>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>

const axios = require('axios').default;

export default {

  name: 'App',

  data() {
    return {
      valutes: {},
      selected: ['RUB', 'USD'],
      inputed: null,
      result: null,
      countries: ['RUB']
    }
  },

  methods: {
    convert() {
      // Default valute values / RUB
      let defaultValute = {
        Value: 1,
        Nominal: 1
      };

      // First selected valute details
      let firstValute = this.valutes[this.selected[0]] ?? defaultValute,
          firstValuteValue = firstValute.Value * Number(this.inputed),
          firstValuteNominal = firstValute.Nominal;

      // Second selected valute details
      let secondValute = this.valutes[this.selected[1]] ?? defaultValute,
          secondValuteValue = secondValute.Value,
          secondValuteNominal = secondValute.Nominal;

      // Result calculating
      let result = (firstValuteValue / firstValuteNominal) / (secondValuteValue / secondValuteNominal);

      // Rounding to ten thousandths
      this.result = result ? Math.floor(result * 10000) / 10000 : null;
    }
  },

  mounted() {
    // Request to API URL, getting response
    axios
      .get('https://www.cbr-xml-daily.ru/daily_json.js')
      .then(response => {
        this.valutes = response.data.Valute;
        // Adding all charCodes to array
        for (let code in response.data.Valute) {
          this.countries.push(code)
        }
      })
      .catch(error => {
        console.log(error)
      })
  },
};

</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap');

#app {
	background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
	background-size: 400% 400%;
	animation: gradient 15s ease infinite;

  @keyframes gradient {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }
}

h1, h2, h3 {
  font-family: 'Indie Flower';
}

.bg--light {
  background: #fcfcfc;
}

.exchange {
  max-width: 50px;
}

</style>
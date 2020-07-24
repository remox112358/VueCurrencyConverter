<template>
  <v-app>
    <v-container class="container">
      <v-row>
        <v-col cols="6" class="bg--light mx-auto" dark>
          <h2 class="text-center">Currency Converter</h2>
          <v-row justify="space-between" no-gutters>
            <v-col cols="5">
              <v-row no-gutters>
                <v-col class="px-1" cols="3">
                  <v-select v-model="selected[0]" :items="countries" return-object>
                    <template v-slot:selection="{ item }">
                      <img class="flag" :src="item.image">
                    </template>
                    <template v-slot:item="{ item }">
                      <img class="flag" :src="item.image">
                    </template>
                  </v-select>
                </v-col>

                <v-col class="px-1" cols="9">
                  <v-text-field @input="convert" v-model="inputed" type="number"></v-text-field>
                </v-col>
              </v-row>
            </v-col>

            <v-col cols="2" class="d-flex justify-center align-center">
              <img class="exchange" :src="require('./assets/icons/exchange.png')" alt="Exchange">
            </v-col>

            <v-col cols="5">
              <v-row no-gutters>
                <v-col class="px-1" cols="9">
                  <v-text-field type="number" disabled></v-text-field>
                </v-col>

                <v-col class="px-1" cols="3">
                  <v-select v-model="selected[1]" :items="countries" return-object>
                    <template v-slot:selection="{ item }">
                      <img class="flag" :src="item.image">
                    </template>
                    <template v-slot:item="{ item }">
                      <img class="flag" :src="item.image">
                    </template>
                  </v-select>
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

  data: () => ({
    valutes: {},
    selected: [ 
      { code: 'RUB', image: require('./assets/flags/228-russia.png') },
      { code: 'USD', image: require('./assets/flags/153-united-states-of-america.png') }
    ],
    inputed: null,
    countries: [
      { code: 'USD', image: require('./assets/flags/153-united-states-of-america.png') },
      { code: 'EUR', image: require('./assets/flags/066-germany.png') },
      { code: 'RUB', image: require('./assets/flags/228-russia.png') },
      { code: 'AMD', image: require('./assets/flags/121-armenia.png') },
      { code: 'KZT', image: require('./assets/flags/034-kazakhstan.png') },
    ]
  }),

  methods: {
    convert() {
      console.log(this.valutes[this.selected[0].code])
      var firstValuteValue = this.valutes[this.selected[0].code].Value / this.valutes[this.selected[0].code].Nominal * this.inputed;
      var secondValuteValue = this.valutes[this.selected[1].code].Value / this.valutes[this.selected[1].code].Nominal;
      console.log(firstValuteValue, secondValuteValue)
      console.log(firstValuteValue / secondValuteValue)
    }
  },

  mounted() {
    axios
      .get('https://www.cbr-xml-daily.ru/daily_json.js')
      .then(response => {
        this.valutes = response.data.Valute;
        console.log(response.data.Valute)
      })
      .catch(error => {
        console.log(error)
      })
  },
};

</script>

<style lang="scss">

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

.bg--light {
  background: #fcfcfc;
}

input {
  min-height: 48px;
}

.flag {
  width: 40px;
}

.exchange {
  max-width: 40px;
}

</style>

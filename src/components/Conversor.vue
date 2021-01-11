<template>
  <div class="conversor">
    <h2>{{ moedaA }} Para {{ moedaB }}</h2>
    <input type="text" v-model="moedaAValue" v-bind:placeholder="moedaA" />
    <input id="converter-btn" type="button" value="Converter" v-on:click="converter" />
    <h2>{{ moedaBValue }}</h2>
  </div>
</template>

<script>
export default {
  name: "Conversor",
  props: ["moedaA", "moedaB"],
    data() {
      return {
        moedaAValue: "",
        moedaBValue: 0
      };
    },
  methods: {
    converter() {
      let dePara = `${this.moedaA}_${this.moedaB}`
      let url = `https://free.currconv.com/api/v7/convert?q=${dePara}&compact=ultra&apiKey=${process.env.VUE_APP_API_KEY}`

      fetch(url).then(resp => { return resp.json() }).then(json => {
        let localeFormat = {"BRL": "pt-br", "USD": "en", "CAD": "en", "EUR": "en"}
        
        if ( this.moedaAValue ) {
          this.moedaBValue = (json[dePara] * parseFloat(this.moedaAValue))
          .toLocaleString(`${localeFormat[this.moedaB]}`, { style: "currency", currency: `${this.moedaB}`})
        } else { alert("Digite um valor :)") }
      })
    }
  }
}
</script>

<style scoped>
.conversor {
  padding: 20px;
  max-width: 300px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}
#converter-btn {
  background-color: rgb(18, 143, 14);
  color: white;
}
</style>

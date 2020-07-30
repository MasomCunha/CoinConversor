<template>
  <div class="converter_container">
    <b-jumbotron header="Coin Converter">
      <Input @clicked="onClickChild" />
      <br />
      <b-button variant="primary" @click="convert">Convert</b-button>
      <hr />
      <Anwser :value="converted_value" />
    </b-jumbotron>
  </div>
</template>

<script>
import Input from "./Input";
import Anwser from "./Answer";

export default {
  components: {
    Input,
    Anwser,
  },
  data() {
    return {
      input_value: "",
      converted_value: "",
      from: "",
      to: " ",
    };
  },
  methods: {
    onClickChild(value, from, to) {
      this.input_value = value;
      this.from = from;
      this.to = to;
    },
    convert() {
      fetch("https://api.exchangeratesapi.io/latest?base=" + this.from, {
        method: "get",
      })
        .then((response) => {
          return response.json();
        })
        .then((jsonData) => {
          let obj = jsonData.rates;
          Object.entries(obj).forEach((entry) => {
            const [key, value] = entry;
            if (key === this.to){
            this.converted_value = this.input_value * value;
            }
          });
        });
    },
  },
};
</script>

<style scoped>
.converter_container {
  display: flex;
  align-content: center;
  justify-content: center;
}
</style>
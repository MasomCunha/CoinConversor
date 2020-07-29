<template>
  <div class='converter_container'>
    <b-jumbotron header="Coin Converter">
      <Input @clicked="onClickChild" />
      <br/>
      <b-button variant="primary" @click="convert">Convert</b-button>
      <hr/>
      <Anwser :value="converted_value"/>
    </b-jumbotron>
  </div>
</template>

<script>
import Input from "./Input";
import Anwser from "./Answer";

export default {
  components: {
    Input,
    Anwser
  },
  data() {
    return {
      input_value: '',
      converted_value: ''
    }
  },
  methods: {
    onClickChild (value) {
      console.log(value)
      this.input_value = value
      console.log(this.input_value)
    },
    convert(){
      this.converted_value = this.input_value * 10
    }
  },
  mounted: function(){
    fetch('https://api.exchangeratesapi.io/latest', {
      method:'get'
    })
    .then((response) => {
      return(response.json())
    })
    .then((jsonData) => {
      console.log(jsonData)
      this.questions = jsonData.results
    })
  }
};
</script>

<style scoped>
.converter_container{
   display: flex;
   align-content: center;
   justify-content: center;
}
</style>
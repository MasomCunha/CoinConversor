<template>
  <div>
    <b-input-group>
      <b-form-input v-model="input_value" placeholder="value"></b-form-input>
      <template v-slot:append>
        <b-dropdown class="btn_from" :text="btn_name_from()" variant="success">
          <b-dropdown-item v-for="rate_from in all_rates" 
          :key="rate_from" 
          @click="select_from(rate_from)"
          >
          {{ rate_from }}
          </b-dropdown-item>
        </b-dropdown>
        <b-dropdown class="btn_to" :text="btn_name_to()" variant="success">
          <b-dropdown-item v-for="rate_to in all_rates" 
          :key="rate_to" 
          @click="select_to(rate_to)">
          {{ rate_to }}
          </b-dropdown-item>
        </b-dropdown>
      </template>
    </b-input-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input_value: "",
      from: "",
      to:"",
      all_rates: []
    };
  },
  methods: {
    select_from(rate) {
      this.$emit('clicked', this.input_value);
      this.from = rate;
    },
    select_to(rate) {
      this.to = rate;
    },
    btn_name_from(){
      if(this.from === "") {
        return "convert from"
      }else{
        return this.from
      }
    },
    btn_name_to(){
      if(this.from === "") {
        return "convert to"
      }else{
        return this.from
      }
    },
    changeOption(item){
      console.log(item)
      this.to = "batata"
      console.log(this.to)
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
      this.all_rates = Object.keys(jsonData.rates)
      console.log(this.all_rates)
    })
  }
};
</script>

<style scoped>
.btn_from {
  margin-left: 10px;
}

.btn_to {
  margin-left: 10px;
}
</style>
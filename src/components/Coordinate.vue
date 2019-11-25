<template>
  <div class="coordinate">
    <div v-bind:class="{hide:loading || converted}">
      <p><input v-model="address" class="form" placeholder="address"></p>
      <p>Range:</p>
      <p><input v-model="y" class="form" placeholder="Y km"></p>
      <p><input v-model="x" class="form" placeholder="X km"></p>
      <button v-on:click="convert()">Convert</button>
    </div>
    <div v-bind:class="{hide:!loading}"><img src="../assets/Preloader_5.gif" alt=""></div>
    <div v-bind:class="{hide:!converted}">
      <p>Top: {{ top }}</p>
      <p>Bottom: {{ bottom }}</p>
      <p>Left: {{ left }}</p>
      <p>Right: {{ right }}</p>
      <button v-on:click="reload()">Reload</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Coordinate',
  data () {
    return {
      loading: false,
      converted: false,
      address: "",
      y: null,
      x: null,
      top: null,
      bottom: null,
      left: null,
      right: null
    }
  },
  methods: {
    convert: function () {
      this.loading = true
      axios.get('https://ousr4ttj2l.execute-api.ap-northeast-1.amazonaws.com/default/getArea', {
        params: {
          address: this.address,
          y: this.y,
          x: this.x
        }
      }).then(response => {
        this.top = response.data.body.top
        this.bottom = response.data.body.bottom
        this.left = response.data.body.left
        this.right = response.data.body.right
        this.converted = true
        this.loading = false
      })
    },
    reload: function () {
      this.loading = false
      this.converted = false
    }
  }
}
</script>
<style scoped>
input {
  font-size: 20px;
}
button {
  color: white;
  font-weight: bold;
  height: 30px;
  background-color: #42b983;
  border: none;
  border-radius: 5px;
}
.hide {
  display: none;
}
</style>

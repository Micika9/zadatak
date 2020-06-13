<template>
  <div
    v-if="list"
    class="home">
    <h1>{{list.title}}</h1>

    <!-- Dropdown select with all Contract Length Options -->
    <paketi-dropdown
      v-model="contract_length"
      :options="contractOptions" />

    <section class="products">
      <!-- List of all products -->
      <paket
        v-for="paket in list.items"
        :key="paket.id"
        :data="list"
        :paket="paket"
        :contractLength="contract_length" />
    </section>

  </div>
</template>

<script>
import axios from 'axios'
import PaketiDropdown from './PaketiDropdown'
import Paket from './Paket'

export default {
  name: 'Home',

  components: {
    Paket,
    PaketiDropdown
  },

  data () {
    return {
      contract_length: null,
      list: null
    }
  },

  computed: {
    contractOptions () {
      if (this.list) {
        return this.list.contract_length.contract_length_options
      }
      return []
    }
  },

  created () {
    this.getData()
  },

  methods: {
    getData () {
      axios.get('http://www.mocky.io/v2/5ed511c53300005f00f7a790').then(response => {
        this.list = response.data
        // Sets Contract Length initial value to be as defined in response
        this.contract_length = this.list.contract_length.preselected_contract_length
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  max-width: 1366px;
  margin: 0 auto;
  height: 100%;
  h1 {
    font-size: 40px;
    color: #742d6c;
    text-align: center;
    margin: 10px 0;
  }
}
</style>

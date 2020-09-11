<template>
  <div>
  <v-text-field label="Network" solo dense v-model="network"/>
  <v-text-field label="Mask" solo dense v-model="mask"/>

  <v-btn color="primary" small outlined @click="tcp_scan()" class="ma-4">TCP Scan</v-btn>
  <v-simple-table>
    <thead></thead>
    <tbody>
      <tr v-for="result of results" v-bind:key="result.host">
        <td>{{result.host}}</td>
        <td>{{result.state}}</td>
        <td>{{result.protocol}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => {
    return {
      network: '192.168.1.0',
      mask: '24',
      results: {}
    }
  },
  methods: {
    tcp_scan() {
      // v-for 
      // v-if 
      // v-on="click" @click
      // let query = this.network + '/' + this.mask
      // let query = `${this.network}/${this.mask}`
      // console.log(query)
      axios({
        url: `http://192.168.1.10:8000/restapi/tcp-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.results = res.data
      })

    }
  }
}
</script>

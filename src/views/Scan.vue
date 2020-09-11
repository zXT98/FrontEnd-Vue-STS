<template>
  <div>
  <v-text-field label="Network" solo dense v-model="network"/>
  <v-text-field label="Mask" solo dense v-model="mask"/>

  <v-btn color="primary" small outlined @click="tcp_scan()" class="ma-4">TCP Scan</v-btn>
  <v-simple-table dark>
    <thead>
      <tr>
        <th class="text-left">Host</th>
        <th class="text-left">Protocol</th>
        <th class="text-left">Port ID</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
        <th class="text-left">Service Name</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(tcpResults).length !== 0">
      <tr v-for="tcp of tcpResults" v-bind:key="`tcp-scan-${tcp.portid}`">
        <td>{{tcp.host}}</td>
        <td>{{tcp.protocol}}</td>
        <td>{{tcp.portid}}</td>
        <td>{{tcp.state}}</td>
        <td>{{tcp.reason}}</td>
        <td v-if="tcp.service">{{tcp.service.name}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  <v-btn color="primary" small outlined @click="fin_scan()" class="ma-4">FIN Scan</v-btn>
  <v-simple-table dark>
    <thead>
      <tr>
        <th class="text-left">Protocol</th>
        <th class="text-left">Port ID</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(finResults).length !== 0">
      <tr v-for="fin of finResults" v-bind:key="`fin-scan-${fin.portid}`">
        <td>{{fin.protocol}}</td>
        <td>{{fin.portid}}</td>
        <td v-if="fin.state">{{fin.state.state}}</td>
        <td v-if="fin.state">{{fin.state.reason}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  <v-btn color="primary" small outlined @click="idle_scan()" class="ma-4">IDLE Scan</v-btn>
  <v-simple-table dark>
    <thead>
      <tr>
        <th class="text-left">Addresses</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
        <th class="text-left">Reason TTL</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(idleResults).length !== 0">
      <tr v-bind:key="`idle-scan-${+new Date}`">
        <td v-if="idleResults.addresses">{{idleResults.addresses[0].addr}}</td>
        <td>{{idleResults.state}}</td>
        <td>{{idleResults.reason}}</td>
        <td>{{idleResults.reason_ttl}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  <v-btn color="primary" small outlined @click="ping_scan()" class="ma-4">PING Scan</v-btn>
  <v-simple-table dark>
    <thead>
      <tr>
        <th class="text-left">Addresses</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
        <th class="text-left">Reason TTL</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(pingResults).length !== 0">
      <tr>
        <td v-if="pingResults.addresses">{{pingResults.addresses[0].addr}}</td>
        <td>{{pingResults.state}}</td>
        <td>{{pingResults.reason}}</td>
        <td>{{pingResults.reason_ttl}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  <v-btn color="primary" small outlined @click="syn_scan()" class="ma-4">SYN Scan</v-btn>
  <v-simple-table dark v-if="synResults !== {}">
    <thead>
      <tr>
        <th class="text-left">Host</th>
        <th class="text-left">Protocol</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
        <th class="text-left">Service</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(synResults).length !== 0">
      <tr v-for="syn of synResults" v-bind:key="`syn-scan-${syn.portid}`">
        <td>{{syn.host}}</td>
        <td>{{syn.protocol}}</td>
        <td>{{syn.state}}</td>
        <td>{{syn.reason}}</td>
        <td v-if="syn.service">{{syn.service.name}}</td>
      </tr>
    </tbody>
  </v-simple-table>

  <v-btn color="primary" small outlined @click="udp_scan()" class="ma-4">UDP Scan</v-btn>
  <v-simple-table dark v-if="udpResults !== {}">
    <thead>
      <tr>
        <th class="text-left">Host</th>
        <th class="text-left">Protocol</th>
        <th class="text-left">Port ID</th>
        <th class="text-left">State</th>
        <th class="text-left">Reason</th>
        <th class="text-left">Service Name</th>
      </tr>
    </thead>
    <tbody v-if="Object.keys(udpResults).length !== 0">
      <tr v-for="udp of udpResults" v-bind:key="`udp-scan-${udp.portid}`">
        <td>{{udp.host}}</td>
        <td>{{udp.protocol}}</td>
        <td>{{udp.portid}}</td>
        <td>{{udp.state}}</td>
        <td>{{udp.reason}}</td>
        <td v-if="udp.service">{{udp.service.name}}</td>
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
      tcpResults: {},
      finResults: [],
      idleResults: {},
      pingResults: {},
      synResults: {},
      udpResults: {},
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
        let hosts = []
        Object.keys(res.data).forEach(entry => {
          console.log(entry)
          console.log(res.data[entry])
          hosts.push(...res.data[entry])
        })
        console.log(hosts)
        this.tcpResults = hosts
      })

    },
    fin_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/fin-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.finResults.push(res.data.ports)
        console.log(this.finResults)
      })

    },
    idle_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/idle-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.idleResults = res.data[0]
      })

    },
    ping_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/ping-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.pingResults = res.data[0]
      })

    },
    syn_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/syn-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        let hosts = []
        Object.keys(res.data).forEach(entry => {
          console.log(entry)
          console.log(res.data[entry])
          hosts.push(...res.data[entry])
        })
        // console.log(hosts)
        this.synResults = hosts
      })

    },
    udp_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/udp-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        let hosts = []
        Object.keys(res.data).forEach(entry => {
          console.log(entry)
          console.log(res.data[entry])
          hosts.push(...res.data[entry])
        })
        // console.log(hosts)
        this.udpResults = hosts
      })

    }
  }
}
</script>


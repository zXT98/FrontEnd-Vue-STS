<template>
  <div>
  <v-text-field label="Network" solo dense v-model="network"/>
  <v-text-field label="Mask" solo dense v-model="mask"/>

    <v-btn color="primary" small outlined @click="top_port_scan()" class="ma-4">Top Port Scan</v-btn>
    <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">Protocol</th>
          <th class="text-left">Port ID</th>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Name</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(topResults).length !== 0">
        <tr v-for="port of topResults" v-bind:key="`top-port-scan-${port.portid}`">
          <td>{{port.protocol}}</td>
          <td>{{port.portid}}</td>
          <td>{{port.state}}</td>
          <td>{{port.reason}}</td>
          <td>{{port.service.name}}</td>

        </tr>
      </tbody>
  </v-simple-table>


    <v-btn color="primary" small outlined @click="list_scan()" class="ma-4">List Scan</v-btn>
    <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">Address</th>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Reason TTL</th>
          <th class="text-left">Address Type</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(listResults).length !== 0">
        <tr>
          <td>{{listResults.addr}}</td>
          <td>{{listResults.state}}</td>
          <td>{{listResults.reason}}</td>
          <td>{{listResults.reason_ttl}}</td>
          <td>{{listResults.addrtype}}</td>
        </tr>
      </tbody>
    </v-simple-table>

    <v-btn color="primary" small outlined @click="os_detection()" class="ma-4">OS Detection</v-btn>
    <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">Name</th>
          <td class="text-left">CPE</td>
          <th class="text-left">Accuracy</th>
          <th class="text-left">Type</th>
          <th class="text-left">Vendor</th>
          <th class="text-left">OS Family</th>
          <th class="text-left">OS Gen</th>

        </tr>
      </thead>
      <tbody v-if="Object.keys(osResults).length !== 0">
        <tr>
          <td>{{osResults.name}}</td>
          <td>{{osResults.cpe}}</td>
          <td>{{osResults.accuracy}}</td>
          <td v-if="osResults.osclass">{{osResults.osclass.type}}</td>
          <td v-if="osResults.osclass">{{osResults.osclass.vendor}}</td>
          <td v-if="osResults.osclass">{{osResults.osclass.osfamily}}</td>
          <td v-if="osResults.osclass">{{osResults.osclass.osgen}}</td>          
        </tr>
      </tbody>
    </v-simple-table>


    <v-btn color="primary" small outlined @click="subnet_scan()" class="ma-4">Subnet Scan</v-btn>
    <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">Address</th>
          <th class="text-left">Port</th>
          <th class="text-left">Protocol</th>
          <th class="text-left">State</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(subnetResults).length !== 0">
        <tr v-for="address of subnetResults" v-bind:key="`subnet-scan-${address.addr}-${address.port}`">
          <td>{{address.addr}}</td>
          <td>{{address.port}}</td>
          <td>{{address.protocol}}</td>   
          <td>{{address.state}}</td> 
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
      network: '192.168.1.10',
      mask: '24',
      topResults: {},
      dnsResults: {},
      listResults: {},
      osResults: [],
      subnetResults: {},
      versionResults: {},
      results: {}
    }
  },
  methods: {
    nmap_version() {
      axios({
        url: `http://192.168.1.10:8000/restapi/nmap-version/`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.results = res.data
      })

    },
    top_port_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/top-port-scan/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.topResults = res.data
      })

    },
    dns_brute_script() {
      axios({
        url: `http://192.168.1.10:8000/restapi/dns-brute-script/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.dnsResults = res.data
      })

    },
    list_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/list-scan/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.listResults = res.data[0]
      })

    },
    os_detection() {
      axios({
        url: `http://192.168.1.10:8000/restapi/os-detection/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        // console.log(res.data)
        // res.data.forEach(entry => {
        //   this.osResults.push(entry)
        // })
        this.osResults = res.data[0]
        // this.osResults.push(res.data)
        console.log(this.osResults)
      })

    },
    subnet_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/subnet-scan/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        // console.log(this.network)
        // console.log(res.data)

        let addresses = []
        res.data.forEach(el => {

          el.ports.forEach(port => {
            let entry = {}
            entry.port = port.port
            entry.protocol = port.protocol
            entry.state = port.state
            entry.addr = el.addr 
            addresses.push(entry)
          })
        })

        // console.log(r)
        this.subnetResults = addresses

        // this.subnetResults = res.data
        console.log(this.subnetResults)
      })

    },
    version_detection() {
      axios({
        url: `http://192.168.1.10:8000/restapi/version-detection/?ip=${this.network}`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.versioResults = res.data
      })

    },
  }
}
</script>

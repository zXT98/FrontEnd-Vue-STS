<template>
  <div>
  <v-text-field label="Network" solo dense v-model="network"/>
  <v-text-field label="Mask" solo dense v-model="mask"/>

  <v-btn color="primary" small outlined @click="port_scan_only()" class="ma-4">Port Scan Only</v-btn>
    <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Address</th>
          <th class="text-left">Service Name</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(portResults).length !== 0">
        <tr v-for="port of portResults" v-bind:key="`port-scan-only-${port.host}-${port.portid}`">
          <td>{{port.state}}</td>
          <td>{{port.reason}}</td>
          <td>{{port.host}}</td>
          <td v-if="port.service">{{port.service.name}}</td>

        </tr>
      </tbody>
    </v-simple-table>

  <v-btn color="primary" small outlined @click="no_port_scan()" class="ma-4">No Port Scan</v-btn>
  <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Address</th>
          <th class="text-left">Type</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(noportResults).length !== 0">
        <tr v-for="noport of noportResults.hosts" v-bind:key="`no-port-scan-${noport.addr}`">
          <td>{{noport.state}}</td>
          <td>{{noport.reason}}</td>
          <td>{{noport.addr}}</td>
          <td>{{noport.addrtype}}</td>

        </tr>
      </tbody>
    </v-simple-table>

      <v-btn color="primary" small outlined @click="arp_discovery()" class="ma-4">ARP Discovery</v-btn>
      <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Address</th>
          <th class="text-left">Service Name</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(arpResults).length !== 0">
        <tr v-for="arp of arpResults" v-bind:key="`arp-discovery-${arp.host}-${arp.portid}`">
          <td>{{arp.state}}</td>
          <td>{{arp.reason}}</td>
          <td>{{arp.host}}</td>
          <td v-if="arp.service">{{arp.service.name}}</td>

        </tr>
      </tbody>
    </v-simple-table>

      <v-btn color="primary" small outlined @click="disable_dns()" class="ma-4">Disable DNS</v-btn>
      <v-simple-table dark>
      <thead>
        <tr>
          <th class="text-left">State</th>
          <th class="text-left">Reason</th>
          <th class="text-left">Address</th>
          <th class="text-left">Service Name</th>
        </tr>
      </thead>
      <tbody v-if="Object.keys(disablednsResults).length !== 0">
        <tr v-for="dns of disablednsResults" v-bind:key="`disable-dns-${dns.host}-${dns.portid}`">
          <td>{{dns.state}}</td>
          <td>{{dns.reason}}</td>
          <td>{{dns.host}}</td>
          <td v-if="dns.service">{{dns.service.name}}</td>
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
      portResults: {},
      noportResults: {},
      arpResults: {},
      disablednsResults: {},
      results: {}
    }
  },
  methods: {
    port_scan_only() {

      axios({
        url: `http://192.168.1.10:8000/restapi/port-scan-only/?ip=192.168.1.10`,
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
        this.portResults = hosts
      })

    },
    no_port_scan() {
      axios({
        url: `http://192.168.1.10:8000/restapi/no-port-scan/?ip=192.168.1.10`,
        method: "get"
      })
      .then(res => {
        console.log(res.data)
        this.noportResults = res.data
      })

    },
    arp_discovery() {
      axios({
        url: `http://192.168.1.10:8000/restapi/arp-discovery/?ip=192.168.1.10&mask=24`,
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
        this.arpResults = hosts
      })

    },
    disable_dns() {
      axios({
        url: `http://192.168.1.10:8000/restapi/disable-dns/?ip=192.168.1.10`,
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
        this.disablednsResults = hosts
      })

    },
  }
}
</script>

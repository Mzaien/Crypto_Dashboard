<template>
    <v-card class="text-center" >
      <h2>
        Exchange {{ccvalue}} {{selectedcc}} to {{selectedpc}}
      </h2>
          <div class="my-4">
          <h4 v-if="ccvalue!==0 ||exCP!==[] " >
          <p v-for="(exC,id) in exCP" :key="id">
        Equals =  {{result= ccvalue* Object.values(exC)[4]}} {{selectedpc}}
          </p>
      </h4>
      </div>
  <v-row>
    <v-col class="text-center">
        <v-autocomplete
      v-model="coins"
      :disabled="isUpdating"
      :items="coin"
      chips
      hide-details
      hide-selected
      item-text="name"
      item-value="code"
      label="Search for a crypto coin..."
      solo
       @change="(event) => updateScc(event)"
    >
      <template v-slot:no-data>
        <v-list-item>
          <v-list-item-title>
            Search for your favorite
            <strong>Cryptocurrency</strong>
          </v-list-item-title>
        </v-list-item>
      </template>
      <template v-slot:selection="{ attr, on, item, selected }"
      >

        <v-chip
          v-bind="attr"
          :input-value="selected"
          color="blue-grey"
          class="white--text"
          v-on="on"
        >
        
          <v-icon left>
            mdi-bitcoin
          </v-icon>
          <span v-text="item.name"></span>
        </v-chip>
      </template>
      <template v-slot:item="{ item }"  >
        <v-list-item-avatar
          color="indigo"
          class="headline font-weight-light white--text"
        >
          {{ item.name.charAt(0) }}
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title v-text="item.name"></v-list-item-title>
          <v-list-item-subtitle v-text="item.code"></v-list-item-subtitle>
        </v-list-item-content>
        <v-list-item-action>
          <v-icon>mdi-bitcoin</v-icon>
        </v-list-item-action>
      </template>
    </v-autocomplete>

      </v-col>
      <v-col>
        <v-autocomplete
      v-model="pcoins"
      :disabled="isUpdating"
      :items="pcoin"
      chips
      hide-details
      hide-selected
      item-text="name"
      item-value="code"
      label="Search for a phisycal coin..."
      solo
      @change="(event) => updateSpc(event)"
    >
      <template v-slot:no-data>
        <v-list-item>
          <v-list-item-title>
            Search for your favorite
            <strong>Phiyscal</strong>
          </v-list-item-title>
        </v-list-item>
      </template>
      <template v-slot:selection="{ attr, on, item, selected }"
      >
        <v-chip
          v-bind="attr"
          :input-value="selected"
          color="blue-grey"
          class="white--text"
          v-on="on"
          @click="()=>{selected}"
        >
        
          <v-icon left>
            mdi-bitcoin
          </v-icon>
          <span v-text="item.name"></span>
        </v-chip>
      </template>
      <template v-slot:item="{ item }"  >
        <v-list-item-avatar
          color="indigo"
          class="headline font-weight-light white--text"
        >
          {{ item.name.charAt(0) }}
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title v-text="item.name"></v-list-item-title>
          <v-list-item-subtitle v-text="item.code"></v-list-item-subtitle>
        </v-list-item-content>
        <v-list-item-action>
          <v-icon>mdi-bitcoin</v-icon>
        </v-list-item-action>
      </template>
    </v-autocomplete>
    </v-col>
  </v-row>
  <v-row >
    <v-col >
      <v-card class="mx-4">
     <v-text-field
      label="Crypto currancy"
      hide-details="auto"
       outlined
      @change="(event) => {setvalue(event)}"
    ></v-text-field>
      </v-card>
    </v-col>
    <!-- <v-col class="mx-4">
      <v-card >
      <v-text-field
      v-model="result"
      label="Phiycal currancy"
      hide-details="auto"
      disabled
      outlined
    >
    </v-text-field>
      </v-card>
    </v-col> -->
  </v-row>
    <button v-if="selectedcc ==='' || selectedpc ===''"  disabled >Choose currencies</button>
    <button v-else-if="ccvalue===0 "  disabled >Fill the amount</button>
    <button v-else @click="()=>{$fetch()}" >Calculate</button>
      </v-card>

</template>
<script lang="ts">
import Vue from 'vue'
let cc= require('../static/cc.json')
let pc= require('../static/pc.json')
export default Vue.extend({
    head:{
    title:"Exchange page"
    },
     data: () => ({
       coins: [],
       pcoins: [],
        coin: cc,
      pcoin: pc,
      selectedpc:'',
      selectedcc:'',
      ccvalue:0,
      result:0,
      exCP:[],
      exPP:[],
      isUpdating: false,
    }),
        methods: {
    updateScc(value:any) {
      console.log(value)
       this.selectedcc =value;
    },
    updateSpc(value:any) {
      console.log(value)
       this.selectedpc =value;
    },
    setvalue(value:number) {
      console.log(value)
      if (value >0) this.ccvalue =value;
    }
},
     async fetch(){
      this.exCP = await fetch(
        `https://www.alphavantage.co/query?function=CURRENCY_EXCHANGE_RATE&from_currency=${this.selectedcc}&to_currency=${this.selectedpc}&apikey=${process.env.API_KEY}`
      ).then(res => res.json())
    }
})
</script>
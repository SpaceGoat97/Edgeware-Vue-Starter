<template>

  <div>
      <button class="button" v-on:click="greet">Connect wallet</button>
      <p >{{ name }}: {{ address }}</p>
  </div>
</template>
<style>
  @import './home.css';
</style>

<script>
// import Identicon from "@polkadot/vue-identicon";
// import keyring from "@polkadot/ui-keyring";
// import { cryptoWaitReady } from "@polkadot/util-crypto";
import {
  web3Accounts,
  web3Enable,
  //   web3ListRpcProviders,
  //   web3UseRpcProvider,
} from "@polkadot/extension-dapp";


export default {
  data: function () {
    let address;
    let name;
    return {
      address,
      name
    }
  },
  methods: {
    async greet() {
      // returns an array of all the injected sources
      //   (this needs to be called first, before other requests)
      await web3Enable("Salmon");
      await web3Accounts()
      .then((res) => {
        
        return ( this.name = res[0].meta.name, this.address = res[0].address )
      })
    },
  },
};
</script>
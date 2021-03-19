<template>
  <!-- <Identicon
    :size="128"
    :theme="'polkadot'"
    :value="'5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'"
  /> -->
  <div>
    <div id="example-2">
      <!-- `greet` 是在下面定义的方法名 -->
      <button v-on:click="greet">Greet</button>
    </div>
  </div>
</template>

<script>
// import Identicon from "@polkadot/vue-identicon";
// import keyring from "@polkadot/ui-keyring";
// import { cryptoWaitReady } from "@polkadot/util-crypto";
import {
  web3Accounts,
  web3Enable,
  web3FromAddress,
  //   web3ListRpcProviders,
  //   web3UseRpcProvider,
} from "@polkadot/extension-dapp";
import { ApiPromise, WsProvider } from "@polkadot/api";
import { options } from "@bifrost-finance/api";

export default {
  methods: {
    async greet(event) {
      console.log(event);
      // returns an array of all the injected sources
      //   (this needs to be called first, before other requests)
      const allInjected = await web3Enable("my cool dapp");

      // returns an array of { address, meta: { name, source } }
      // meta.source contains the name of the extension that provides this account
      const allAccounts = await web3Accounts();
      console.log(event, allInjected, allAccounts);

      // the address we use to use for signing, as injected
      const SENDER = "gPZ4o3vGBYJn321jVJ3ahGPTnz5TGMSACqZaAs9LsP8hFvo";

      // finds an injector for an address
      const injector = await web3FromAddress(SENDER);

      // create instance
      const wsProvider = new WsProvider("wss://bifrost-rpc.testnet.liebi.com");
      const api = await ApiPromise.create(options({ provider: wsProvider }));

      // Do something
      console.log(api.genesisHash.toHex());
      api.tx.balances
        .transfer("gGT6EDq2igApbW68wgKCjwDi12Vu3CZG4NBLWiNJGRrUUrk", 22000000000000)
        .signAndSend(SENDER, { signer: injector.signer }, (status) => {
          console.log(status);
        });
      // `this` 在方法里指向当前 Vue 实例
      //   alert("Hello " + this.name + "!");
      //   // `event` 是原生 DOM 事件
      //   if (event) {
      //     alert(event.target.tagName);
      //   }
    },
  },
};
</script>
---
title: Quick Start
sidebar_position: 2
---

Import and configure quirks plugin inside your `main.ts`.

```ts
import { osmosis, osmosisAssetList } from "@nabla-studio/chain-registry";
import { quirksPlugin } from "@quirks/vue";
import { Config, ssrPersistOptions } from "@quirks/store";
import { keplrExtension, leapExtension } from "@quirks/wallets";
import { createApp } from "vue";
import App from "./app/App.vue";

const app = createApp(App);

const config: Config = {
  wallets: [keplrExtension, leapExtension],
  chains: [osmosis],
  assetsLists: [osmosisAssetList],
};

app.use(quirksPlugin, config).mount("#root");
```

## How to connect a wallet

```vue
<script setup lang="ts">
import { useConnect } from '@quirks/vue';

const { connect, disconnect, connected } = useConnect();
</script>

<template>
  <button @click="disconnect" v-if="connected">DISCONNECT</button>
  <button @click="connect("keplr-extension")" v-else>CONNECT</button>
</template>
```

## How to allow user to choose a wallet

```vue
<script setup lang="ts">
import { useConnect, useConfig } from "@quirks/vue";

const { connect, disconnect, connected } = useConnect();
const { wallets } = useConfig();
</script>

<template>
  <button @click="disconnect" v-if="connected">DISCONNECT</button>
  <div v-else>
    <div v-for="wallet in wallets" :key="wallet.options.name">
      <button
        @click="
          {
            open(wallet.options.name);
          }
        "
      >
        <img
          :src="wallet.options.logoUrls?.light?.svg"
          :alt="wallet.options.prettyName"
          height="48px"
          width="48px"
          :style="{ width: '48px', height: '48px' }"
        />
      </button>

      <a
        :href="
          wallet.options.downloads && wallet.options.downloads.length > 0
            ? wallet.options.downloads[0].link
            : '#'
        "
        target="_blank"
        v-if="!wallet.injected"
      >
        Install
      </a>
    </div>
  </div>
</template>
```

### Available Hooks

- **[useConnect](./hooks/use-connect)**
- **[useChain](./hooks/use-chain)**
- **[useChains](./hooks/use-chains)**
- **[useConfig](./hooks/use-config)**
- **[useWalletConnect](./hooks/use-wallet-connect)**

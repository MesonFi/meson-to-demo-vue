<script setup>
import { onMounted } from 'vue'
import { ParticleNetwork, WalletEntryPosition } from '@particle-network/auth'
import { ParticleProvider } from '@particle-network/provider'

import MesonTo from '@mesonfi/to'
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

onMounted(() => {
  const particle = new ParticleNetwork({
    projectId: import.meta.env.VITE_PARTICLE_PROJECT_ID,
    clientKey: import.meta.env.VITE_PARTICLE_CLIENT_KEY,
    appId: import.meta.env.VITE_PARTICLE_APP_ID,
    chainName: 'Ethereum',
    chainId: 1,
    wallet: {
      displayWalletEntry: true,
      defaultWalletEntryPosition: WalletEntryPosition.BR,
      supportChains: [{ id: 1, name: 'Ethereum' }, { id: 42161, name: 'Arbitrum' }],
      customStyle: {},
    }
  })

  const particleProvider = new ParticleProvider(particle.auth)
  particleProvider.request({ method: 'eth_accounts' })
    .then(accounts => {
      const meson2 = new MesonTo(window)
      meson2.open({ id: 'opbnb', addr: accounts[0], for_gas: 1, provider: particleProvider }, 'iframe')
    })

})
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
